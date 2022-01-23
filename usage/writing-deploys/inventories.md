## Inventories

An inventory is either an inventory module file like `inventory.py`, or an ad-hoc
url like `example.com` or `root@localhost`. Usually an inventory is defined as a module.

All special attributes that can be defined by an inventory are documented in [InventoryWrapper](TODO).
For simplicity, you can regard your inventory module like an "instance" of this wrapper class.
You can override the documented attributes and functions to modify it to you needs.

The most important ones being `hosts` and `groups` which are used to declare
the hosts and groups that belong to the inventory.

### Declaring hosts

Hosts are declared by defining the `hosts` variable.

{% tabs %}
{% tab title="simple.py" %}
Hosts can be declared by just specifying the connection url.
If this url is given without an connection schema, `ssh://` will be used as the default.

```python
hosts = ["host.example.com",    # same as ssh://host.example.com. The host's name will be deduced as `host.example.com`
         "root@localhost",      # same as ssh://root@localhost. The hosts' name will be deduced as `localhost`
         "ssh:root@localhost",  # same as ssh://root@localhost.
         "local:mymachine"]     # Uses a the local connector which runs on the current machine as the user executing fora.
```
{% endtab %}
{% tab title="inventory.py" %}
If you want to specify additional information, such as the host's groups,
choosing a specific name or specifying the host module file,
you can do so using a `dict` or by using the [`Host Declaration`](TODO) type.

Specifying either a url or a name is mandatory. If you leave the url empty, you
need to specify the url or a connector later in the host module.

```python
# Instead of using `dict` below, you can use this typed variant if you like.
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

Groups are declared by defining the `groups` variable.
If you omit this definition, a list of groups is automatically gathered from
the group names used in your `hosts`.

{% tabs %}
{% tab title="inventory.py" %}
```python
# Instead of using `dict` below, you can use these typed variants if you like.
from fora.types import HostDeclaration, GroupDeclaration

hosts = [dict(name="host1", groups=["desktops", "archlinux"]),
         dict(name="host2", groups=["desktops", "gentoo"])]

# We could omit this definition, if we didn't want to add inter-group dependencies.
# We for example want to make sure that the desktops group is evaluated after
any of the OS groups, to allow it to modify variables defined by the respective OS group.
groups = [dict(name="desktops", after=["archlinux", "gentoo"]),
          "archlinux",
          "gentoo"]
```
{% endtab %}
{% endtabs %}

### Setting global variables from the inventory

### Overwriting other behavior

### Inspecting an inventory

{% code title="inventory.py" %}
```python
hosts = [
    "host1.example.com",
    ("host2", "ssh://root@host2.example.com"),
    ("host3", "ssh://root@host3.example.com", "file/to/host.py"), # Useful for shared host definition files
    "host4",
    "ssh://host3.example.com",
    ""]
# Hosts are tuples of (name, host_definition_file) where host_definition_file is the
# python module that will be instanciated for the host, or just a name, in which case the
# instanciated module will default to "hosts/{name}.py".
#
# The instanciated module has access to the name of the host via fora.host_definition.name
# and may adapt it's behavior dynamically based on this information.
hosts = [ "localhost"
#        , ("mail1", "hosts/mail.py")
#        , ("mail2", "hosts/mail.py")
        ]

# The inventory is loaded early, so you may modify the parsed options of
# fora here, to enforce certain settings for your inventory.
# from fora import globals as G
# G.args.diff = True
```
{% endcode %}

You can run scripts against specific hosts of an inventory by specifying `--hosts`.

- --show-inventory , --debug
