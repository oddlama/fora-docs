# Hosts

A host declares a single remote machine, on which scripts can be executed.
Hosts can override and set variables in their respective host module file.

By default, the path to this file is `hosts/<name>.py`, relative to the inventory.
If it exists, it will be loaded and its globals will become the host's variables.

All special attributes of a host are documented in [`HostWrapper`](api/fora/types.md#class-types.HostWrapper).
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
Have a look at the [`Connection`](api/fora/connection.md#class-connection.Connection) class for a reference.

# Groups

A group defines variables that and can be inherited by other groups or hosts.
The groups of a host are first sorted by load-order respecting inter-group dependencies.
A group only has access to variables from the groups loaded before them, and the
host always inherits all variables from the last loaded group (which will include the variables
from all previously loaded groups). Is this done to make sure all ambiguous or
conflicting variables definitions are detected.

In principle, group modules function entirely analogous to host modules,
except that they are only used to define shared global variables.
By default, an inventory tries to load group modules from `groups/<name>.py`, relative to the inventory.
