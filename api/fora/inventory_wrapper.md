# fora.inventory_wrapper

Provides the inventory wrapper for all inventory related functionality.

## <mark style="color:red;">class</mark> `inventory_wrapper.HostDeclaration`

A declaration of a host in an inventory.

### Attributes

#### <mark style="color:yellow;">attr</mark> `url`

```python
url: Optional[str] = None
```

The default url used to connect to this host. If this is given without an connection
schema (like `schema:...`), `ssh://` will be used as the default. The function responsible
for this is `qualify_url`. If this is None, it can still be defined by the host module
later.

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: Optional[str] = None
```

The name that will be used to refer to this specific host.

If this is `None`, the name is extracted from the `url` after
qualification by the means of `extract_hostname`. In that case, the connector
determined by the `url` is responsible to parse it and provides a hostname.
This means that both of the urls `localhost` and `ssh://root@localhost:22` will
result in a host named `localhost` by default.

Beware that hostname extraction naturally needs to occurr before the corresponding
host module is loaded. The module can theoretically overwrite its intially assigned
`url` or even specify an explicit connector implementation, which can never be used
to assign a different name to the host in retrospect. Therefore, specifying the final
`url` in the inventory is preferred.

#### <mark style="color:yellow;">attr</mark> `file`

```python
file: Optional[str] = None
```

The module file for this host, relative to the `base_dir`.

If `None`, this will default to `{hosts_dir}/{name}.py`. In that case,
the file is optional and will only be loaded when it exists.
If this attribute is set explicitly the file must exist, otherwise an error will be thrown.

#### <mark style="color:yellow;">attr</mark> `groups`

```python
groups: list[str] = field(default_factory=list)
```

The groups for this host. Duplicate entries are ignored.
All hosts will always be added to the global `all` group,
regardless of whether it is part of this list.

## <mark style="color:red;">class</mark> `inventory_wrapper.GroupDeclaration`

A declaration of a group in an inventory.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name that will be used to refer to this specific group.

#### <mark style="color:yellow;">attr</mark> `file`

```python
file: Optional[str] = None
```

The module file for this group, relative to the `base_dir`.

If `None`, this will default to `{groups_dir}/{name}.py`. In that case,
the file is optional and will only be loaded when it exists.
If this attribute is set explicitly the file must exist, otherwise an error will be thrown.

#### <mark style="color:yellow;">attr</mark> `after`

```python
after: list[str] = field(default_factory=list)
```

This group will be loaded _after_ this given list of groups.
The global `all` group will always be added to this list.
Duplicates are ignored.

#### <mark style="color:yellow;">attr</mark> `before`

```python
before: list[str] = field(default_factory=list)
```

This group will be loaded _before_ this given list of groups.
Duplicates are ignored.

## <mark style="color:red;">class</mark> `inventory_wrapper.InventoryWrapper`

A wrapper class for inventory modules. This will wrap any instanciated
inventory to provide default attributes and methods for the inventory.

### Attributes

#### <mark style="color:yellow;">attr</mark> `groups_dir`

```python
groups_dir: str = 'groups'
```

The directory where to search for group module files, relative to the inventory.

#### <mark style="color:yellow;">attr</mark> `hosts_dir`

```python
hosts_dir: str = 'hosts'
```

The directory where to search for host module files, relative to the inventory.

#### <mark style="color:yellow;">attr</mark> `hosts`

```python
hosts: list[Union[str, HostDeclaration, dict[str, Any]]] = field(default_factory=list)
```

The list of hosts in this inventory. See `HostDeclaration` for an explanation
of the parameters for individual hosts. If a `dict` is given, it is automatically
used to construct a HostDeclaration. Providing a single `str` is equivalent to
`HostDeclaration(url=the_str)`.

Duplicate entries (same name) will cause an exception to be raised when the
inventory is loaded.

Example:

    hosts = [HostDeclaration(url="localhost", groups=["desktops"])),
             dict(url="host.example.com", name="myhost"),
             "example.com"]

#### <mark style="color:yellow;">attr</mark> `groups`

```python
groups: Optional[list[Union[str, GroupDeclaration, dict[str, Any]]]] = None
```

The list of groups in this inventory. See `GroupDeclaration` for an explanation
of the parameters for individual groups. If a `dict` is given, it is automatically
used to construct a GroupDeclaration. Providing a single `str` is equivalent to
`GroupDeclaration(name=the_str)`.

The global `all` group will always be added to this list, if it isn't already.

Duplicate entries (same name) will cause an exception to be raised when the
inventory is loaded.

Example:

    groups = [GroupDeclaration(name="desktops", after=["archlinux"]),
              dict(name="servers", after=["archlinux"]),
              "archlinux"]

#### <mark style="color:yellow;">attr</mark> `loaded_hosts`

```python
loaded_hosts: dict[str, HostWrapper] = field(default_factory=dict)
```

All loaded hosts. Set when the inventory is processed.

### <mark style="color:yellow;">def</mark> `is_initialized()`

```python
def is_initialized(self) -> bool:
```

Returns True if the inventory is fully initialized.

### <mark style="color:yellow;">def</mark> `available_groups()`

```python
def available_groups(self) -> set[str]:
```

Returns the set of available groups in this inventory.
By default each module file in `groups_dir` (relative to the inventory module)
creates a group of the same name, disregarding the `.py` extension.

Note that the `all` group will always be made available, even if it isn't explicitly
returned by this function. This function should only return groups that have a
corresponding module file.

#### Returns

 -  **set[str]**: The available group definitions.

#### Raises

 -  **RuntimeError**: The inventory has no associated module file.

### <mark style="color:yellow;">def</mark> `base_dir()`

```python
def base_dir(self) -> str:
```

Returns absolute path of this inventory's base directory, which
is usually its containing folder.

#### Returns

 -  **str**: The absolute base directory path.

#### Raises

 -  **RuntimeError**: The inventory has no associated module file.

### <mark style="color:yellow;">def</mark> `base_remote_settings()`

```python
def base_remote_settings(self) -> RemoteSettings:
```

Returns the base remote settings that will be used when connections to hosts
from this inventory are created. Usually the host connection will override
certain parameters such as the default executing and owning user and group,
to match the privileges the remote dispatcher is running under.

#### Returns

 -  **RemoteSettings**: The base remote settings.

### <mark style="color:yellow;">def</mark> `group_module_file()`

```python
def group_module_file(self, name: str) -> Optional[str]:
```

Returns the absolute group module file path given the group's name.
Returning None associates no group module file to the group by default.

#### Parameters

 -  **name**: The group name to return the module file path for.

#### Returns

 -  **Optional[str]**: The group module file path.

### <mark style="color:yellow;">def</mark> `host_module_file()`

```python
def host_module_file(self, name: str) -> Optional[str]:
```

Returns the absolute host module file path given the host's name.
Returning None associates no host module file to the host by default.

#### Parameters

 -  **name**: The host name to return the module file path for.

#### Returns

 -  **Optional[str]**: The host module file path.

### <mark style="color:yellow;">def</mark> `qualify_url()`

```python
def qualify_url(self, url: str) -> str:
```

Returns a valid url for any given url from the hosts array if possible.

By default this function selects `ssh://` as the default schema
for any url that has no explicit schema.

#### Parameters

 -  **url**: The url to qualify.

#### Returns

 -  **str**: The qualified url.

#### Raises

 -  **ValueError**: The provided url was invalid.

### <mark style="color:yellow;">def</mark> `extract_hostname()`

```python
def extract_hostname(self, url: str) -> str:
```

Extracts the hostname from a given url. By default
this is done via the the responsible connector.

#### Parameters

 -  **url**: The url to extract the hostname from.

#### Returns

 -  **str**: The extracted hostname.

#### Raises

 -  **ValueError**: The provided url was invalid.

### <mark style="color:yellow;">def</mark> `load()`

```python
def load(self) -> None:
```

This function preprocesses the declared hosts and groups, calculates
dependent variables like `_topological_order` and actually instanciates
required modules. This should be called after wrapping a module to ensure
the wrapped module didn't supply any bogus declarations and that all
dynamic definitions are fully loaded.

#### Raises

 -  **ValueError**: An invalid supplied value caused an error while processing.

### <mark style="color:yellow;">def</mark> `load_group()`

```python
def load_group(self, name: str, initializer: Optional[GroupWrapper]
               ) -> GroupWrapper:
```

Creates a new instance of the given group.

#### Parameters

 -  **name**: The group to instanciate.

 -  **initializer**: A previously loaded group module that should be used to initialize
    this module's global variables before its code is executed.

#### Returns

 -  **GroupWrapper**: A new instance of the declared group.

### <mark style="color:yellow;">def</mark> `load_host()`

```python
def load_host(self, name: str, initializer: Optional[GroupWrapper]
              ) -> HostWrapper:
```

Creates a new instance of the given host.

#### Parameters

 -  **name**: The host to instanciate.

 -  **initializer**: A previously loaded host module that should be used to initialize
    this module's global variables before its code is executed.

#### Returns

 -  **HostWrapper**: A new instance of the declared host.

### <mark style="color:yellow;">def</mark> `instanciate_host()`

```python
def instanciate_host(self, host: str) -> HostWrapper:
```

This function instanciates the given host by recursively loading all groups
in the correct topological order and propagating variables until finally the
host module is instanciated.

Different hosts don't share group instanciations, as the groups may modify
variables from previously loaded groups (e.g. add to dictionaries). As two
different hosts can share just a single group out of many, and because groups
need to modify global state in-place, we cannot reuse a group instanciated for
another host.

This loader asserts that a group doesn't redefine (as in completely overwrite)
an existing variable, when this group doesn't have a dependency on the group
from where the instanciation is overwritten. This prevents ambiguous definitions
caused by the two groups having arbitrary relative ordering. Although this does
assume that a group only modifies existing variables by adding information
(e.g. appending to list or adding keys to a dict) instead of removing information.

#### Parameters

 -  **host**: The name of the host to instanciate.
