# Outlining the differences

## How is Fora different from existing tools?

You are probably here, asking yourself why I wrote this when there already are tools doing the same things. The answer is mainly that I liked writing it while I could address some issues I had with existing tools. Is Fora perfect? No. Certainly far from it. But it is my shot at creating something I personally enjoy using.

{% hint style="info" %}
I took a lot of inspiration from amazing existing tools like [Ansible](https://www.ansible.com) and [pyinfa](https://pyinfra.com). I urge you to check them out if you haven't already. If you require large-scale deploys with many hosts, they are most likely a better choice and definitely more mature tools.
{% endhint %}

## Outlining the differences

To better illustrate the actual differences, I have compiled a list of short examples showing how Fora solves different problems I encountered with other tools. Basic knowledge about general infrastructure tools will be beneficial to fully understand this section.

In no particular order, I wanted to create a tool that...

### ...is more developer friendly.

I can already code python and I want access to the full capabilities of python at my fingertips when writing scripts or defining variables. In Fora, everything is a regular python script. Need to fetch information from a REST API to define your inventory? Just do it like you would in any other python script.

### ...can detect variable definition conflicts.

Groups are ultimately a concept that has no total ordering. Defining a variable on two unrelated groups can result in ambiguity depending on the load order. Which definition wins? Fora introduces inter-group dependencies to allow specifying the load order. If an ambiguous variable definition is detected, Fora will raise an error.

### ...properly deals with the variable merging versus overwriting.

You probably have experienced the issue before. Especially with dictionaries, sometimes you want to replace it, and other times you just want to add a key to it. As group evaluation order is well-defined in Fora, it can just give you full access to the variable definitions up to that point. As all variables are just module-level global variables, you can decide whether to overwrite or extend a definition.

{% tabs %}
{% tab title="groups/all.py" %}
```python
# We always want to change root's login shell.
system_users = {"root": {"shell": "/bin/zsh"}}
```
{% endtab %}

{% tab title="groups/desktops.py" %}
```python
# Desktops should additionally have a user for ourselves.
system_users["myuser"] = {"id": 1000}
```
{% endtab %}

{% tab title="groups/servers.py" %}
```python
# Servers need a user for www content. Even though the desktops and servers
# group have no defined ordering, concurrent modification is allowed.
# This doesn't create ambiguities, as long as the group doesn't remove
# or modify existing information (which is hard to detect).
# Altough, if the desktop group overwrites `system_users`, Fora
# would detect this as an error.
system_users["www"] = {"home": "/var/www", "system": True}
```
{% endtab %}

{% tab title="hosts/proxy.py" %}
```python
# The proxy machine is special and should not modify users at all.
# So we just overwrite the dictionary with an empty one.
system_users = {}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Fora also provides the `--inspect-inventory <inventory>` option, which can be used to analyze the origin and effective value of each host variable.
{% endhint %}

### ...reduces boilerplate operation arguments.

When configuring services, you often need to create many files with a specific owner, group and mode. Therefore, I want an option to change the defaults for some of those regularly used parameters. Fora accomplishes this by providing a context manager:

{% tabs %}
{% tab title="deploy_website.py" %}
```python
from fora.operations import files

# Every script starts with the same set of defaults specified in the inventory.
with defaults(file_mode="640", dir_mode="750", owner="root", group="nginx"):
    files.template(src="files/index.html", dest="/var/www/")
    # ... 20 other files

    # This can also be nested and will build upon the currently active defaults
    with defaults(owner="www"):
        files.upload(...)
```
{% endtab %}
{% endtabs %}

### ...doesn't interface with the remote using shell commands.

Did you ever find yourself in need to shell-escape command arguments? Me neither.

What I want is the equivalent of a remote `subprocess.run`, for both internal use and for scripts. That way you don't have to worry about escaping command arguments, but still have the option to spawn a shell with `["bash", "-c", "<shellcommand>"]` when needed.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora import host

# Perfectly safe, arguments will directly be passed to subprocess.run on the remote.
ret = host.connection.run(["somecommand", "set", host.somevariable])
# You also automatically have access to the captured output.
print(ret.returncode, ret.stdout, ret.stderr)
```
{% endtab %}
{% endtabs %}

### ...allows scripts to define variable defaults.

Scripts should be able to easily define defaults for host variables, without needing to create a separate configuration file or requiring the user to add a default definition to `groups/all.py`.

Fora does this transparently by simply falling back to a lookup on the current script's globals when a host variable is unset.

{% tabs %}
{% tab title="add_users.py" %}
```python
from fora import host

users_to_add = []

# If the host has defined the variable, this will return it.
# Otherwise, it will automatically fall back to the global defined above.
for user in host.users_to_add:
    print(user)
```
{% endtab %}
{% endtabs %}

### ...allows proper script reuse.

Some scripts are more like functions and I often want to reuse a script with just slightly different variables. Consider for example a `deploy_website.py` script that automatically creates nginx configuration for a website. It maybe just requires the path to a webroot that should be copied.

Fora allows you to pass regular python objects as script parameters by adding a simple global namespace:

{% tabs %}
{% tab title="with_params.py" %}
```python
@Params
class params:
    path: str
    users: list[str] = [] # Also allows default parameters

print(params.path)
print(params.users)
```
{% endtab %}

{% tab title="deploy.py" %}
```python
from fora import local

# Execute a script with parameters
local.script("with_params.py", params=dict(path="/path/to/somewhere"))
```
{% endtab %}
{% endtabs %}

### ...reuses a single ssh connection for all commands.

This is the only sane choice, and it is infinitely faster than the alternative. I also really don't fancy babysitting my deploys and needing to authorize ssh access 50 times.

Fora usually establishes a connection by running a small dispatcher script over ssh, which is reused for all commands.

{% hint style="danger" %}
Currently you cannot escalate privileges on the remote systems. You either need to login as root, or be restricted to the user you logged in as. _(This will be possible in the future)_
{% endhint %}

### ...provides useful output.

I want to see the important information (applied changes, errors, ...) highlighted over the purely informational output. Especially instead of being presented with a terminal-filling block of text in a yellowish tint that appears to have come from `/dev/urandom`.

By default, Fora displays a short hint for each executed and prints each state parameter that changed. You can pass `--diff` to additionally get a diff for each changed file.

Don't like colors? Fora respects the `NO_COLOR` environment variable.

### ...behaves more like a dumb API.

Smart tools are always great, but sometimes dumb tools are better. A tool with a lot of hidden internal state can create surprising situations and make it hard to reason about what is actually going on.

I wanted to encourage writing clean, simple and self-contained deploy scripts, which meant removing all internal state beyond what is absolutely necessary. After inventory loading, Fora behaves like a regular python libary with some public global variables to access the loaded information. No caching, and no gathering of facts that could change later.

### ...focuses on being a remote scripting API and not an entire language and environment.

Keeping the tool lightweight increases maintainability for everyone involved. Fewer things can change or break, and fewer things need to be kept updated.

If you for example need to store a secret, you can already choose from a lot of good python libraries. There's no need for Fora to implement any of this. Refer to the [Using secrets](examples/using-secrets.md) section for more information.

### ...generally reduces boilerplate where possible.

When I just want to write a deploy for my dotfiles, a single (and short) `deploy.py` script should be all that is needed, and shouldn't include information specific to my system.

Fora can run against ad-hoc inventories such as `root@localhost` or `local:` (the current user on the local system).

{% tabs %}
{% tab title="command" %}
To run a deploy against the current user on the local system, you can simply execute:

```bash
fora local: deploy.py
```
{% endtab %}

{% tab title="deploy.py" %}
```python
from fora import host
from fora.operations import files

# Get the home directory of the current remote user
home = host.home_dir()

# Install neovim dotfiles
files.directory(f"{home}/.config/nvim")
files.upload(src="neovim/init.lua", f"{home}/.config/nvim/init.lua")
```
{% endtab %}
{% endtabs %}

### ...stops on errors by default.

When an operation fails for whatever reason, the whole deploy will be aborted. If an error is expected, these automatic checks can be disabled by passing `check=False` to any operation or catching the exception. This makes deploy scripts behave sanely, like any regular script.

## Drawbacks

Here are a few drawbacks compared to other approaches:

* There is no inherent synchronization between hosts. While parallelization is theoretically possible, hosts are provisioned one-by-one, currently sequentially. This also means the output is grouped by host, which makes it harder to spot differences between hosts in a particular operation.
* There currently is no way of selecting a subset of operations by a tag. While the user can recreate this behavior by modularizing scripts or by introducing a tag variable, it is not a builtin feature of Fora.
* Group definitions are executed once per relevant host. This is necessary to ensure correct variable modification behavior. Slow operations in a group module will therefore contribute more to execution time when there are many hosts.
* Fora requires all managed hosts to already have python3.9 installed. _(This situation might improve in the future)_
* Currently you cannot escalate privileges on the remote systems. You either need to login as root, or be restricted to the user you logged in as. _(Will be possible in the future)_
