# fora.operations.utils

Provides utiliy functions for operations.

## Attributes

### <mark style="color:yellow;">attr</mark> `utils.package_managers`

```python
utils.package_managers: dict[str, Any] = {}
```

All registered package managers as a map from (command name -> package function).

### <mark style="color:yellow;">attr</mark> `utils.service_managers`

```python
utils.service_managers: dict[str, Any] = {}
```

All registered service managers as a map from (command name -> service function).

## Functions

### <mark style="color:yellow;">def</mark> `utils.find_command()`

```python
def utils.find_command(conn: Connection, 
                       command_to_result_map: dict[str, Any]
                       ) -> Optional[Any]:
```

Searches for any of the commands provided as keys in `command_to_result_map`,
and if found on the target system, returns the associated value from the map.

### <mark style="color:yellow;">def</mark> `utils.package_manager()`

```python
def utils.package_manager(command: str) -> Callable[[Callable], Callable]:
```

Operation function decorator to denote that this operation constitutes the package() operation of a package manager.
This will cause it to be registered such that system.package() can call this package function if the given command is detected on the remote system.

See [`pacman.package()`](pacman.md#def-pacman.package) for an example usage.

### <mark style="color:yellow;">def</mark> `utils.service_manager()`

```python
def utils.service_manager(command: str) -> Callable[[Callable], Callable]:
```

Operation function decorator to denote that this operation constitutes the service() operation of a service manager.
This will cause it to be registered such that system.service() can call this service function if the given command is detected on the remote system.

See [`systemd.service()`](systemd.md#def-systemd.service) for an example usage.

### <mark style="color:yellow;">def</mark> `utils.generic_package()`

```python
def utils.generic_package(op: Operation, packages: list[str], present: bool, 
                          is_installed: Callable[[str], bool], 
                          install: Callable[[str], None], 
                          uninstall: Callable[[str], None]
                          ) -> OperationResult:
```

A generic package operation that will query the current system state and
call install/uninstall on each of the packages where an action is required
to reach the target state.

#### Parameters

 -  **op**: The operation wrapper.

 -  **packages**: The packages to modify.

 -  **present**: Whether the given package should be installed or uninstalled.

 -  **is_installed**: A function that returns whether a given package is installed.

 -  **install**: A function that installs the given package on the remote system.

 -  **uninstall**: A function that uninstalls the given package on the remote system.

### <mark style="color:yellow;">def</mark> `utils.save_content()`

```python
def utils.save_content(op: Operation, content: Union[bytes, str], dest: str, 
                       mode: Optional[str] = None, 
                       owner: Optional[str] = None, 
                       group: Optional[str] = None) -> OperationResult:
```

Saves the given content as dest on the remote host. Only for use within an operation,
if save_content is the main functionality. You must supply the op parameter.

#### Parameters

 -  **op**: The operation wrapper.

 -  **content**: The file content.

 -  **dest**: The remote destination path.

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

### <mark style="color:yellow;">def</mark> `utils.check_absolute_path()`

```python
def utils.check_absolute_path(path: str, path_desc: str) -> None:
```

Asserts that a given path is non empty and absolute.

#### Parameters

 -  **path**: The path to check.

 -  **path_desc**: Will be printed in case of error as a substitute for the invalid variable

### <mark style="color:yellow;">def</mark> `utils.new_op_fail()`

```python
def utils.new_op_fail(op_name: str, name: Optional[str], desc: str, 
                      error: str) -> OperationError:
```

Creates a new operation with given name and description and immediately
returns a failed status with the given error message. Also returns a OperationError in case
the callee want's to raise and exception.

This is useful for meta-operations, that have a failure condition before the
required sub-operation is determined (e.g. [`system.package()`](system.md#def-system.package) can call different package
manager's package() operation, but can also fail to find a suitable one).
