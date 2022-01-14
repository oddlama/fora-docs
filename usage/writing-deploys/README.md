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
from fora import host
host.add_group("web") # Add to group
```
{% endtab %}

{% tab title="deploy.py" %}
```python
from fora.host import current_host as host
from fora.operations import system

if "web" in host.groups: # Install nginx if this is a web-host
	system.package(name="Install nginx", packages=["nginx"])
```
{% endtab %}
{% endtabs %}

## Deploy structure

The deploy structure is very flexible. There are only two things to keep in mind:

* The working directory of a script is always its containing folder, so relative paths will work as expected. There is no definite _root directory_ for a deploy.
* Host and group definitions are always expected to be in the `hosts/` and `groups/` folders relative to the inventory file.

While managing dotfiles can be done cleanly with a single `deploy.py` script, we recommend one of the following layouts for larger scale infrastructure management:

{% tabs %}
{% tab title="default.txt" %}
```python
deploy/
├─ hosts/        # Host specific configuration
├─ groups/       # Group specific configuration
│  └─ all.py     # Global variables
├─ tasks/        # Reusable scripts for performing specific tasks
│  └─ nginx.py
├─ files/        # Static configuration files uploaded via `files.upload()`
├─ templates/    # Templated configuration files uploaded via `files.template()`
├─ inventory.py  # A list of all managed hosts
└─ deploy.py     # The main deploy script
```
{% endtab %}

{% tab title="staging-production.txt" %}
```python
deploy/
├─ inventories/
│  ├─ hosts/        # Host specific configuration
│  ├─ groups/       # Group specific configuration
│  │  └─ all.py     # Global variables
│  ├─ staging.py    # Only staging hosts
│  └─ production.py # Only production host
├─ tasks/           # Reusable scripts for performing specific tasks
│  └─ nginx.py
├─ nginx/           # This could be an external repository
│   ├─ files/       # Static configuration files uploaded via `files.upload()`
│   ├─ templates/   # Templated configuration files uploaded via `files.template()`
│   └─ nginx.py
└─ deploy.py        # The main deploy script
```
{% endtab %}
{% endtabs %}

You can use `fora --init` in an empty directory to initialize it with a default deploy structure. See the included [Examples](../TODO/) to see how different layouts may be used.
