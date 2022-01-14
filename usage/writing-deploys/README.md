# Writing deploys

A deploy is a collection of several components:

* [**Inventories.**](TODO/) Collections of hosts, against which your scripts are run. Either defined by a file, or by specifying a single hostname like `root@example.com` for ad-hoc runs.
* [**Hosts.**](TODO/) Host definitions allow you to assign variables and groups to specific hosts.
* [**Groups.**](TODO/) Hosts inherit the variables defined by the groups they belong to. All hosts implicitly belong to the `all` group, which can be used to define global variables.
* [**Scripts.**](TODO/) Regular python scripts specifying what should be done on the remote hosts.

Fora always requires at least one inventory and a script to run. Here is an example using each of the above components:

{% tabs %}
{% tab title="inventory.py" %}
```python
# A list of your hosts.
hosts = ["myhost"]
```
{% endtab %}

{% tab title="groups/web.py" %}
```python
# A group can define variables, but doesn't need to.
```
{% endtab %}

{% tab title="hosts/myhost.py" %}
```python
add_group("web") # Add to group
```
{% endtab %}

{% tab title="deploy.py" %}
```python
from fora import host
from fora.operations import system

if "web" in host.groups: # Install nginx if this is a web-host
    system.package(name="Install nginx", packages=["nginx"])
```
{% endtab %}
{% endtabs %}

## Deploy structure

The general deploy structure is very flexible. There are only two things to keep in mind:

* The working directory of a script is always its containing folder, so relative paths will work as expected. There is no definite _root directory_ for a deploy.
* Host and group definitions are always expected to be in the `hosts/` and `groups/` folders relative to the inventory file.

We recommend one of the following layouts for larger scale infrastructure management:

{% tabs %}
{% tab title="minimal.txt" %}
While a single `deploy.py` would be truly minimal, this is a reasonable starting point to extend from.

```python
deploy/
├─ hosts/           # Host specific configuration
│  └─ localhost.py
├─ inventory.py     # A list of all managed hosts
└─ deploy.py        # The main deploy script
```
{% endtab %}

{% tab title="flat.txt" %}
A flat layout for smaller deploys where all files and templates are stored in the main directory.

```python
deploy/
├─ hosts/           # Host specific configuration
│  └─ localhost.py
├─ groups/          # Group specific configuration
│  └─ all.py        # Global variables
├─ tasks/           # Reusable scripts for performing specific tasks
│  └─ nginx.py
├─ files/           # Static configuration files uploaded via `files.upload()`
├─ templates/       # Templated configuration files uploaded via `files.template()`
├─ inventory.py     # A list of all managed hosts
└─ deploy.py        # The main deploy script
```
{% endtab %}

{% tab title="dotfiles.txt" %}
Creates just a `deploy.py` intended to be run locally with `fora local: deploy.py` to install dotfiles for the current user.

```python
dotfiles/
├─ zsh/             # zsh shell
│  └─ zshrc
├─ kitty/           # kitty terminal
│  └─ kitty.conf
├─ neovim/          # neovim configuration
│  └─ init.lua
└─ deploy.py        # The main deploy script that will deploy all dotfiles
```
{% endtab %}

{% tab title="modular.txt" %}
The arguably most versatile of the single-inventory layouts. Functionality should be separated into modular tasks. Tasks can for example manage a specific service, and the main deploy just calls the task scripts.

```python
deploy/
├─ hosts/           # Host specific configuration
│  └─ localhost.py
├─ groups/          # Group specific configuration
│  └─ all.py        # Global variables
├─ tasks/           # Modular and reusable scripts for performing specific tasks
│  ├─ nginx/        # A task to manage nginx. This could also be an external repository.
│  │  ├─ files/     # Static configuration files uploaded via `files.upload()`
│  │  ├─ templates/ # Templated configuration files uploaded via `files.template()`
│  │  ├─ nginx.py   # The main nginx configuration task
│  │  └─ add_server.py # A modular script that can be called multiple times to add server blocks
│  └─ [...]
├─ inventory.py     # A list of all managed hosts
└─ deploy.py        # The main deploy script
```
{% endtab %}

{% tab title="staging_prod.txt" %}
A variation of the modular layout that defines two separate inventories to allow deploying against a set of staging and/or production hosts.

```python
deploy/
├─ inventories/
│  ├─ hosts/        # Host specific configuration
│  ├─ groups/       # Group specific configuration
│  │  └─ all.py     # Global variables
│  ├─ staging.py    # Only staging hosts
│  └─ production.py # Only production host
├─ tasks/           # Modular and reusable scripts for performing specific tasks
│  ├─ nginx/        # A task to manage nginx. This could also be an external repository.
│  │  ├─ files/     # Static configuration files uploaded via `files.upload()`
│  │  ├─ templates/ # Templated configuration files uploaded via `files.template()`
│  │  │─ nginx.py   # The main nginx configuration task
│  │  └─ add_server.py # A modular script that can be called multiple times to add server blocks
│  └─ [...]
└─ deploy.py        # The main deploy script
```
{% endtab %}
{% endtabs %}

You can use `fora --init <layout_name>` in an empty directory to initialize it with a specific deploy structure. See the included [Examples](../TODO/) to see how different layouts may be used.
