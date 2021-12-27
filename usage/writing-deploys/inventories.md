# Inventories

An inventory defines 

{% code title="inventory.py" %}
```python
hosts = [
    "host1.example.com",
    "host2.example.com",
    "host3.example.com",
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
