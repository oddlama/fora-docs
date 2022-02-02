# Inventories

An inventory is either an inventory module file like `inventory.py`, or an ad-hoc url like `example.com` or `root@localhost`. Usually an inventory is defined as a module.

All special attributes that can be defined by an inventory are documented in [`InventoryWrapper`](api/fora/inventory_wrapper.md#class-inventory_wrapper.InventoryWrapper).
For all means and purposes, you can regard your inventory module as an "instance" of this wrapper class. You can override the documented attributes and functions to modify it to your needs.

The two main inventory attributes are `hosts` and `groups` which are used to declare the hosts and groups that belong to the inventory.

### Declaring hosts

Hosts are declared by defining the `hosts` variable.

{% tabs %}
{% tab title="simple.py" %}
Hosts can be declared by just specifying the connection url. If this url is given without an connection schema, `ssh://` will be used as the default.

```python
hosts = ["host.example.com",    # same as ssh://host.example.com. The host's name will be deduced as `host.example.com`
         "root@localhost",      # same as ssh://root@localhost. The hosts' name will be deduced as `localhost`
         "ssh:root@localhost",  # same as ssh://root@localhost.
         "local:mymachine"]     # Uses a the local connector which runs on the current machine as the user executing fora.
```
{% endtab %}

{% tab title="inventory.py" %}
If you want to specify additional information, such as the host's groups, choosing a specific name or specifying the host module file, you can do so using a `dict` or by using the [`HostDeclaration`](api/fora/inventory_wrapper.md#class-inventory_wrapper.HostDeclaration) type.

Specifying either a url or a name is mandatory. If you leave the url empty, you need to specify the url or a connector later in the host module.

```python
# Instead of using `dict`, you can use this typed variant if you like.
from fora.types import HostDeclaration

hosts = [dict(url="host.example.com", groups=["desktops"]),
         dict(name="host1"),                     # Tries to use hosts/host1.py if it exists.
         dict(name="web1", file="hosts/web.py"), # Requires hosts/web.py
         dict(name="web2", file="hosts/web.py"), # Requires hosts/web.py
         dict(name="web3", file="hosts/web.py")] # Requires hosts/web.py
```
{% endtab %}
{% endtabs %}

### Declaring groups

Groups are declared by defining the `groups` variable. If you omit this definition, a list of groups is automatically gathered from the group names used in your `hosts`.

{% code title="inventory.py" %}
```python
# Instead of using `dict`, you can use these typed variants if you like.
from fora.types import HostDeclaration, GroupDeclaration

hosts = [dict(name="host1", groups=["desktops", "archlinux"]),
         dict(name="host2", groups=["desktops", "gentoo"])]

# We could omit this definition, if we didn't want to add inter-group
# dependencies. We for example want to make sure that the desktops group
# is evaluated after # any of the OS groups, to allow it to modify
# variables defined by the respective OS group.
groups = [dict(name="desktops", after=["archlinux", "gentoo"]),
          "archlinux",
          "gentoo"]
```
{% endcode %}

### Inspecting an inventory

To see whether your inventory behaves as expected, you can use

```bash
fora --inspect-inventory inventory.py
```

This will show a summary of all declared hosts and groups. For each host, it additionally lists the origin and effective value of all defined variables.

### Setting global variables from the inventory

Especially when managing several inventories (for example staging and production), you may want to set global variables (like API keys) from your inventory definition.
You can do this simply by defining a global variable or method that isn't a special inventory variable.
To keep variables private to your inventory, use a leading underscore like `_variable`.

{% code title="inventory_staging.py" %}
```python
import os

# Global
api_key = os.getenv("API_KEY_STAGING")

# You may of course import definitions from other inventories to avoid repetition.
from other_inventory import hosts
```
{% endcode %}

### Overwriting other behavior

You may overwrite any public method defined in the [`InventoryWrapper`](api/fora/inventory_wrapper.md#class-inventory_wrapper.InventoryWrapper) to further customize the behavior of the inventory.
