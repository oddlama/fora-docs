# Writing deploys

A deploy is a collection of several components:

* [**Inventories.**](TODO/) Collections of hosts, against which your scripts are run. Standalone hostnames like `root@example.com` are valid inventories.
* [**Hosts.**](TODO/) Host definitions allow you to assign variables and groups to specific hosts.
* [**Groups.**](TODO/) Hosts inherit the variables defined by the groups they belong to. All hosts implicitly belong to the `all` group, which can be used to define global variables.
* [**Scripts.**](TODO/) Regular python scripts specifying what should be done on the remote hosts.

Fora always requires at least one inventory and a main script to run.

## Deploy structure

The deploy structure is very flexible. There are only two things to keep in mind:

* The working directory of a script is always its containing folder, so relative paths will work as expected. There is no definite _root directory_ for a deploy.
* Host and group definitions are always expected to be in the `hosts/` and `groups/` folders relative to the inventory file.

While the specific layout is up to you, we recommend the following:

{% tabs %}
{% tab title="default.txt" %}
```python
deploy/
  hosts/        # Host specific configuration
  groups/       # Group specific configuration
    |-- all.py        # Global variables
  tasks/        # Reusable scripts for performing specific tasks
    |-- nginx.py
  files/        # Static configuration files uploaded via `files.upload()` 
  templates/    # Templated configuration files uploaded via `files.template()`
    |-- inventory.py  # A list of all managed hosts
  |-- deploy.py     # The main deploy script
```
{% endtab %}

{% tab title="staging-production.txt" %}
```python
inventories/
  hosts/        # Host specific configuration
  groups/       # Group specific configuration
    |-- all.py        # Global variables
  |-- staging.py      # All staging hosts
  |-- production.py   # All production hosts
tasks/        # Reusable scripts for performing specific tasks
  nginx/          # This could be an external repository
    files/        # Static configuration files uploaded via `files.upload()` 
    templates/    # Templated configuration files uploaded via `files.template()`
    |-- nginx.py
|-- deploy.py         # The main deploy script
```
{% endtab %}
{% endtabs %}

TOdOOOOOOOO Depending on your specific use-case, it might be beneficial to organize your deploy. Managing dotfiles be done cleanly with a single `deploy.py` script and no explicit inventory. For real infrastructure management we recommend keeping things organized and modular.

See the included [Examples](../TODO/) for more details on different layouts for different use-cases.
