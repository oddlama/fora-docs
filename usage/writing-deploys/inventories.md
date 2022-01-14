# Inventories

Essentially, an inventory is a list of hosts on which Fora executes scripts.

- either single names
- or a file defining an array of hosts.

A list of hosts in this inventory. Entries are either a single host url,
or a tuple of `(url, file)`, where `file` is the module file for that host.
Module files are searched relative to the inventory and default to `{hosts_dir}/{name}.py`
if not given. Host module files are optional, except when explicitly specified.

If the host url is given without an connection schema (like `ssh://`),
by default `ssh://` will be prepended as each url is passed through `qualify_url`.

The host's "friendly" name is extracted from the url after qualification
by the means of `extract_hostname`. By default the responsible connector
will be asked to provide a hostname. This means that both `localhost` and
`ssh://root@localhost:22` will result in a host named `localhost`.

Beware that a host module could possibly overwrite its assigned url
or specify an explicit connector in its module file. This means the
connector which extracted the hostname before host instanciation could
possibly be a different one than the connector used later for the connection.

{% code title="inventory.py" %}
```python
hosts = [
    "host1.example.com",
    ("host2", "ssh://root@host2.example.com"),
    ("host3", "ssh://root@host3.example.com", "file/to/host.py"), # Useful for shared host definition files
    "host4"
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
