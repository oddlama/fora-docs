# Scripts

Scripts are in fact just regular python scripts executed for each host.
Surprising, I know. To your scripts, Fora mostly behaves just like a regular library.
Your inventory will already have been been loaded, and a connection to the
current host will have been established.

### Global state

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
# This includes transitive group dependencies.
if "desktops" in host.groups:
	# ...

# Execute a command on the remote. Remember you have full access to all variables of the host.
print(host.connection.run(["echo", f"Hello from the other side ({host.name})"]).stdout.decode("utf-8"))
```
{% endtab %}
{% endtabs %}

### Using operations

The most important part of your deploy scripts will be calling operations to modify the remote host.
Operations are idempotent functions, which means that calling them multiple times has no different effect then calling them once.
They examine the hosts current state and execute just the neccessary commands to bring it to the target state.
This allows you to simply specify the target state instead of having to repeatedly write complex logic to achieve this manually.

An overview of all availabe operations can be found in the [Operations](TODO) section.

### Default variables

### Parameters

### Remote defaults
