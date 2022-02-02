# fora.types

Provides a mockup of loadable module types. They are used to store metadata
that can be accessed by the module that is currently being loaded. These
types also help the static type checker, as it then has a better understanding
of the expected contents of the dynamically loaded modules.

## <mark style="color:red;">class</mark> `types.RemoteDefaultsContext`

A context manager to overlay remote defaults on a stack of defaults.

## <mark style="color:red;">class</mark> `types.VariableActionSnapshot`

A snapshot for variable tracking.

### Attributes

#### <mark style="color:yellow;">attr</mark> `action`

```python
action: Literal['definition', 'modification']
```

Whether the variable was modified or redefined

#### <mark style="color:yellow;">attr</mark> `actor`

```python
actor: ModuleWrapper
```

The owner of this variable

#### <mark style="color:yellow;">attr</mark> `value`

```python
value: Any
```

The snapshot value.

## <mark style="color:red;">class</mark> `types.ModuleWrapper`

A module wrapper, that defaults attribute lookups to this object if the module doesn't define it.
Derived classes must be annotated with @dataclass.

### Attributes

#### <mark style="color:yellow;">attr</mark> `module`

```python
module: Optional[ModuleType] = None
```

The dynamically loaded inventory module

### <mark style="color:yellow;">def</mark> `ModuleWrapper.is_exported_variable()`

```python
def ModuleWrapper.is_exported_variable(self, attr: str, value: Any) -> bool:
```

Returns True if the the given variable doesn't inherently belong to this group.

### <mark style="color:yellow;">def</mark> `ModuleWrapper.exported_variables()`

```python
def ModuleWrapper.exported_variables(self):
```

Returns a list of exported variables, which are variables that don't inherently belong to this group.

#### Returns

 -  **dict[str, Any]**: Global exported variables of the wrapped module

### <mark style="color:yellow;">def</mark> `ModuleWrapper.is_overloaded()`

```python
def ModuleWrapper.is_overloaded(self, attr: str) -> Optional[bool]:
```

Returns NonoTrue if the given attribute exists as a variable on this wrapper but is overloaded by the wrapped module,
False if the attribute exists on this wrapper but isn't overloaded and None if the attribute doesn't exist on this wrapper.

### <mark style="color:yellow;">def</mark> `ModuleWrapper.is_overridden()`

```python
def ModuleWrapper.is_overridden(self, attr: str) -> bool:
```

Returns True if a variable has both been overloaded and changed.

### <mark style="color:yellow;">def</mark> `ModuleWrapper.wrap()`

```python
def ModuleWrapper.wrap(self, module: Any, copy_members: bool = False, 
                       copy_functions: bool = False) -> None:
```

Replaces the currently wrapped module (if any) with the given object.
The module should be an instance of ModuleType in most cases, but doesn't need to be.
Any object is supported.

#### Parameters

 -  **module**: The new module to wrap.

 -  **copy_members**: Copy all current member variables of this wrapper to the wrapped module.
    Excludes members starting with an underscore (`_`) and members of ModuleWrapper.

 -  **copy_functions**: Copy all current member functions of this wrapper to the wrapped module,
    such that calling module.function(...) is forwarded to this wrapper's self.function(...).
    Excludes functions starting with an underscore (`_`) and functions of ModuleWrapper.

### <mark style="color:yellow;">def</mark> `ModuleWrapper.definition_file()`

```python
def ModuleWrapper.definition_file(self) -> str:
```

Returns the file from where the associated module has been loaded,
or "<internal>" if no module file is associated with this wrapper.

#### Returns

 -  **str**: The file.

## <mark style="color:red;">class</mark> `types.GroupWrapper`

A wrapper class for group modules. This will wrap any instanciated
group to provide default attributes and methods for the group.

All functions and members from this wrapper will be implicitly available
on the wrapped group module. This means you can do the following

    print(name)       # Access this group's name

instead of having to first import the wrapper API:

    from fora import group as this
    print(this.name)

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the group. Must not be changed.

## <mark style="color:red;">class</mark> `types.HostWrapper`

A wrapper class for host modules. This will wrap any instanciated
host to provide default attributes and methods for the host.

All functions and members from this wrapper will be implicitly available
on the wrapped host module. This means you can do the following

    url = "ssh://root@localhost" # Use this url to connect
    print(name)                  # Access this hosts's name

instead of having to first import the wrapper API:

    from fora import group as this
    print(this.name)

### Attributes

#### <mark style="color:yellow;">attr</mark> `inventory`

```python
inventory: InventoryWrapper
```

A back reference to the parent inventory which created this host.

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name that used to refer to this specific host. Must not be changed.

#### <mark style="color:yellow;">attr</mark> `url`

```python
url: Optional[str]
```

The url used to connect to this host. The schema in this url will be used to
determine which connector implementation is used to establish a connection,
if the `connector` has not been explicitly overridden by the module.
This is determined just before a connection is initiated.

By default, this field will reflect the value specified in the inventory,
after url qualification. If this is None, `connector` must be set explicitly.

#### <mark style="color:yellow;">attr</mark> `groups`

```python
groups: list[str] = field(default_factory=list)
```

The set of groups this host belongs to.

#### <mark style="color:yellow;">attr</mark> `connector`

```python
connector: Optional[Callable[[Optional[str], HostWrapper], Connector]] = None
```

The connector class to use. If `None`, the connector will be determined by the schema in the `url` when needed.

#### <mark style="color:yellow;">attr</mark> `connection`

```python
connection: Connection = cast('Connection', None)
```

The active connection to this host, if one is opened.

### <mark style="color:yellow;">def</mark> `HostWrapper.create_connector()`

```python
def HostWrapper.create_connector(self) -> Connector:
```

Creates a connector for this host.

#### Returns

 -  **Connector**: A connector for this host

#### Raises

 -  **FatalError**: The connector could not resolved because either an invalid connector was specified
    or the scheme could not be matched against existing connectors.

### <mark style="color:yellow;">def</mark> `HostWrapper.vars_hierarchical()`

```python
def HostWrapper.vars_hierarchical(self) -> dict[str, Any]:
```

Returns `vars(self)` but adds all variables defined by the current script that
are not overwritten by this host.

#### Returns

 -  **dict[str, Any]**: The variables of this object.

## <mark style="color:red;">class</mark> `types.ScriptWrapper`

A mockup type for script modules. This is not the actual type of an instanciated
module, but will reflect some of it's properties better than ModuleType. While this
class is mainly used to aid type-checking, its properties are transferred to the
actual instanciated module before the module is executed.

When writing a script module, you can use the API exposed in [`fora.script`](api/fora/\_\_init\_\_.md#attr-fora.script)
to access/change meta information about your module.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the script. Must not be changed.

### <mark style="color:yellow;">def</mark> `ScriptWrapper.defaults()`

```python
def ScriptWrapper.defaults(self, as_user: Optional[str] = None, 
                           as_group: Optional[str] = None, 
                           owner: Optional[str] = None, 
                           group: Optional[str] = None, 
                           file_mode: Optional[str] = None, 
                           dir_mode: Optional[str] = None, 
                           umask: Optional[str] = None, 
                           cwd: Optional[str] = None
                           ) -> RemoteDefaultsContext:
```

Returns a context manager to incrementally change the remote execution defaults.

This function is implicitly available on the wrapped script module.
This means you can do the following

    with defaults(owner="root", file_mode="644", dir_mode="755"):
        # ... execute some operations

instead of having to first import the wrapper API:

    from fora import script
    with script.defaults(owner="root", file_mode="644", dir_mode="755"):
        # ... execute some operations

### <mark style="color:yellow;">def</mark> `ScriptWrapper.current_defaults()`

```python
def ScriptWrapper.current_defaults(self) -> RemoteSettings:
```

Returns the fully resolved currently active defaults.

#### Returns

 -  **RemoteSettings**: The currently active remote defaults.

### <mark style="color:yellow;">def</mark> `ScriptWrapper.Params()`

```python
def ScriptWrapper.Params(self, params_cls: Type[T]) -> Type[T]:
```

Decorator used to declare script parameters.

This function is implicitly available on the wrapped script module.
This means you can do the following

    @Params
    class params:
        my_parameter: str

instead of having to first import the wrapper API:

    from fora import script

    @script.Params
    class params:
        my_parameter: str
