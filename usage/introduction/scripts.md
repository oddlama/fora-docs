# Scripts

Scripts are in fact just regular python scripts executed for each host.
Surprising, I know. In a script, Fora behaves like a regular library.
Just remember that your inventory will already have been been loaded,
and a connection to the current host will have been established.

Scripts are always executed in their containing folder. This makes
it easy to bundle files related to the script in folders relative to it.

### Accessing global state

You can access the inventory and the current host via the
exposed global state in the main `fora` module. You can use
these to directly execute commands on the host, or to retrieve
variables from a host or inventory.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora import host, inventory

# You have full access to the current host and inventory,
# as well as all attribute of them.
print(inventory.loaded_hosts)

# Check whether the host belongs to a specific group.
if "desktops" in host.groups:
	# ...

# Execute a command on the remote. Remember you have full access to all variables of the host.
print(host.connection.run(["echo", f"Hello from the other side ({host.name})"]).stdout.decode("utf-8"))
```
{% endtab %}
{% endtabs %}

### Using operations

The most important part of your deploy scripts will be calling operations to modify the remote host.
Operations examine the host's current state and execute just the neccessary commands to bring it to the target state.
Operations are idempotent functions, so calling them multiple times doesn't affect the final outcome.

- All operations return a [`OperationResult`](api/fora/operations/api.md#class-api.operationresult) object, which can be used to
examine the initial and target state of the host regarding this operation.
- If an operation fails and `check=False` has not been passed to the operation,
the script is automatically aborted.
- All operations support an optional `name="Description of what is being done"` parameter, which will be printed
on execution so it is easier to follow what is being done.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora.operations import local, files, system

# Upload a file, if it doesn't already exist and has the same content.
files.upload(name="Upload some file", src="files/testfile", dest="/tmp/testfile")

# Install a program using a supported package manager
system.package(name="Install nginx", packages=["neovim"])

# Run a different script.
local.script(name="Run a different script", script="install nginx.py")

# We rarely need to check `ret.changed` when using operations to program in
# terms of target state, as this will likely compromise idempotency.
ret = system.user(name="Delete an obsolete user", user="ciao")
if ret.changed:
	# ...
```
{% endtab %}
{% endtabs %}

An overview of all available operations can be found in the [Operation Index](api/index\_operations.md "mention") section.
You may of course also write your own operations. For this I recommend reading the implementation
of some existing operations.

### Variables and fallback values

You can customize your script's behavior based on host variables.
Usually, scripts will have a set of variables that can be customized by the host,
but also need a fallback value. For this, you can define a global variable in your script
with the same name as the expected host variable. If the host has no such variable, accessing
`host.myvariable` will automatically return your global fallback value in that case.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora.operations import files

motd = f"Hello from {host.name}!"
files.upload_content(dest="/etc/motd", content=host.motd)
```
{% endtab %}
{% tab title="hosts/myhost.py" %}
```python
# If this variable was removed, the fallback value in the script would be used
motd = "(╯°□°）╯︵ ┻━┻"
```
{% endtab %}
{% endtabs %}

### Parameters

Instead of customizing script behavior based on host variables, you might want
to write a script that can be reused in your deploy and should accept parameters.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora.operations import local

local.script("add_user_with_ssh.py", params=dict(user="someuser", authorized_keys=["ssh-ed25519 AAAA..."]))
local.script("add_user_with_ssh.py", params=dict(user="seconduser"))
```
{% endtab %}
{% tab title="add_user_with_ssh.py" %}
```python
from fora import host
from fora.operations import files, system

@Params
class params:
    user: str                       # Required parameter
    authorized_keys: list[str] = [] # Optional parameter

# Create user
system.user(user=params.user)
user_home = host.connection.home_dir(params.user)

# Upload authorized_keys
if len(params.authorized_keys) > 0:
	files.directory(f"{user_home}/.ssh/authorized_keys", owner=params.user, group=params.user, mode="700")
	files.upload_content(dest=f"{user_home}/.ssh/authorized_keys", "\n".join(params.authorized_keys), owner=params.user, group=params.user, mode="600")
```
{% endtab %}
{% endtabs %}

### Remote defaults

If parameters like `owner=`, `group=` or `mode=` are not given, they will default
to some value initially specified by the inventory as [`base_remote_settings()`](api/fora/types.md#def-InventoryWrapper.base_remote_settings).

When configuring services, you often need to create many files with the same specific owner, group and mode.
Scripts provide a context manager [`defaults()`](api/fora/types.md#def-ScriptWrapper.defaults) to temporarily change these defaults
to avoid repetition. These defaults also specify process information, such as which user is used to
run commands on the remote. Here is an overview over what can be modified:

- `owner` - the owner for new files and directories
- `group` - the group for new files and directories
- `file_mode` - the mode for new files (determines `mode=` on operations such as `files.upload`)
- `dir_mode` - the mode for new directories (determines `mode=` on operations such as `files.directory`)
- `umask` - the effective umask for remote commands
- `cwd` - the working directory for remote commands
- `as_user` - the user as which the remote commands are executed
- `as_group` - the group as which the remote commands are executed

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

	# Executed scripts will always start with clean defaults, so this context
	# will have no effect on this operation:
	local.script("otherdeploy.py")

with defaults(as_user="nginx"):
	# Defaults affect the connection itself. This prints uid=...(nginx)
	print(host.connection.run(["id"]).stdout.decode())
```
{% endtab %}
{% endtabs %}
