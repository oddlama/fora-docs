# fora.loader

Provides the dynamic module loading utilities.

## Attributes

### <mark style="color:yellow;">`attr`</mark>` loader.script_stack`

```python
loader.script_stack: list[tuple[ScriptWrapper, inspect.FrameInfo]] = []
```

A stack of all currently executed scripts ((name, file), frame).

## <mark style="color:red;">`class`</mark>` loader.ImmediateInventory`

A temporary inventory just for a single run, without the ability to load host or group module files.

### <mark style="color:yellow;">`def`</mark> `base_dir()`

```python
def base_dir(self) -> str:
```

An immediate inventory has no base directory.

### <mark style="color:yellow;">`def`</mark> `group_module_file()`

```python
def group_module_file(self, name: str) -> Optional[str]:
```

An immediate inventory has no group modules.

### <mark style="color:yellow;">`def`</mark> `host_module_file()`

```python
def host_module_file(self, name: str) -> Optional[str]:
```

An immediate inventory has no host modules.

### <mark style="color:yellow;">`def`</mark> `available_groups()`

```python
def available_groups(self) -> set[str]:
```

An immediate inventory has no groups.

## <mark style="color:yellow;">`def`</mark> `loader.load_inventory()`

```python
def loader.load_inventory(inventory_file_or_host_url: str) -> None:
```

Loads the global inventory from the given filename or single-host url
and validates the definintions.

### Parameters

 -  **inventory_file_or_host_url**: Either a single host url or an inventory module file (`*.py`). If a single host url
    is given without a connection schema (like `ssh://`), ssh will be used.

### Raises

 -  **FatalError**: The loaded inventory was invalid.

## <mark style="color:yellow;">`def`</mark> `loader.run_script()`

```python
def loader.run_script(script: str, frame: inspect.FrameInfo, 
                      params: Optional[dict[str, Any]] = None, 
                      name: Optional[str] = None) -> None:
```

Loads and implicitly runs the given script by creating a new instance.

### Parameters

 -  **script**: The path to the script that should be instanciated

 -  **frame**: The FrameInfo object as given by inspect.getouterframes(inspect.currentframe())[?]
    where the script call originates from. Used to keep track of the script invocation stack,
    which helps with debugging (e.g. cyclic script calls).

 -  **name**: A printable name for the script. Defaults to the script path.