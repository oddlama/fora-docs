# Best practices

### Use `name="..."` on everything.

All operations support the `name=` parameter which allows you to describe what it is doing. This will make following the output much easier.

### Never modify host or group variables in your scripts.

This will lead to a strange coupling between an inventory and scripts. If you need to track global state, please create an additional python module just for this purpose, or avoid it in the first place.

### Use the type checked API.

Fora is fully typed. While it is more verbose, this allows you to use certain typed variables from an imported wrapper instead of implicitly defined globals to allow for proper type-checking with `mypy` or similar tools.

{% tabs %}
{% tab title="script.py" %}
```python
from fora import script

#@Params       # Implicitly defined, no type available to type-checkers.
@script.Params # Properly typed.
class params:
	#...
```
{% endtab %}

{% tab title="hosts/host.py" %}
```python
# The host variable will always be the current host while loading.
from fora import host as this

#print(groups) <-- unchecked
print(this.groups)
```
{% endtab %}

{% tab title="groups/group.py" %}
```python
from fora import group as this

#print(name) <-- unchecked
print(this.name)
```
{% endtab %}
{% endtabs %}
