# Hosts

A host declares a single remote machine, on which scripts can be executed.
Hosts can override and set variables in their respective host module file.

By default, the path to this file is `hosts/<name>.py`, relative to the inventory.
If it exists, it will be loaded and it's globals will become the host's variables.

All special attributes of a host are documented in [`HostWrapper`](../writing-deploys/TODO/).
As for the inventories, you can regard host modules as an "instance" of this wrapper class.
You can override the documented attributes and functions to modify it to your needs.

The primary attributes of a host (`url`, `name` and `groups`) were already declared
in the inventory and will be transferred to the module's global variables before it is loaded.
Of those, only the `url` may be changed in the module file.

### Variables

Variables are simply the module's global variables. Any variable starting with
an underscore is regarded as private to the module and will not be visible to your scripts.
All global variables from the loaded groups will be transferred to the host before it is loaded,
allowing you to modify them.

{% tabs %}
{% tab title="hosts/myhost.py" %}
```python
# Overwrites variable from the "all" group
motd = "Hello from myhost!"

# Adds an entry to the variable previously defined in the "all" group
packages_to_install.append("nginx")

# This variable will not be visible to scripts
_temporary = "hello i'm private"
```
{% endtab %}
{% tab title="groups/all.py" %}
```python
motd = "Hello!"
packages_to_install = ["neovim"]
```
{% endtab %}
{% endtabs %}

### Connection

When the connection to a host is established, its `connection` variable will be
set to the active connection. This means you can directly interact with the host
in a script by using the functions given by this connection object.
Have a look at [`Connection`](TODO) class for a reference.
