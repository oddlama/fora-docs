# fora.utils

Provides utility functions.

## <mark style="color:red;">`class`</mark>` utils.FatalError`

An exception type for fatal errors, optionally including a file location.

## <mark style="color:red;">`class`</mark>` utils.CycleError`

An error that is throw to report a cycle in a graph that must be cycle free.

## <mark style="color:yellow;">`def`</mark> `utils.print_status()`

```python
def utils.print_status(status: str, msg: str) -> None:
```

Prints a message with a (possibly colored) status prefix.

## <mark style="color:yellow;">`def`</mark> `utils.print_warning()`

```python
def utils.print_warning(msg: str) -> None:
```

Prints a message with a (possibly colored) 'warning: ' prefix.

## <mark style="color:yellow;">`def`</mark> `utils.print_error()`

```python
def utils.print_error(msg: str, loc: Optional[str] = None) -> None:
```

Prints a message with a (possibly colored) 'error: ' prefix.

## <mark style="color:yellow;">`def`</mark> `utils.len_ignore_leading_ansi()`

```python
def utils.len_ignore_leading_ansi(s: str) -> int:
```

Returns the length of the string or 0 if it starts with `[`

## <mark style="color:yellow;">`def`</mark> `utils.ansilen()`

```python
def utils.ansilen(ss: Collection[str]) -> int:
```

Returns the length of all strings combined ignoring ansi control sequences

## <mark style="color:yellow;">`def`</mark> `utils.ansipad()`

```python
def utils.ansipad(ss: Collection[str], pad: int = 0) -> str:
```

Joins an array of string and ansi codes together and pads the result with spaces to at least `pad` characters.

## <mark style="color:yellow;">`def`</mark> `utils.print_fullwith()`

```python
def utils.print_fullwith(left: Optional[list[str]] = None, 
                         right: Optional[list[str]] = None, pad: str = '─', 
                         **kwargs: Any) -> None:
```

Prints a message padded to the terminal width to stderr.

## <mark style="color:yellow;">`def`</mark> `utils.print_table()`

```python
def utils.print_table(header: Collection[Collection[str]], 
                      rows: Collection[Collection[Collection[str]]], 
                      box_color: str = '\x1b[90m', 
                      min_col_width: Optional[list[int]] = None) -> None:
```

Prints the given rows as an ascii box table.

## <mark style="color:yellow;">`def`</mark> `utils.die_error()`

```python
def utils.die_error(msg: str, loc: Optional[str] = None, 
                    status_code: int = 1) -> NoReturn:
```

Prints a message with a colored 'error: ' prefix, and exit with the given status code afterwards.

## <mark style="color:yellow;">`def`</mark> `utils.load_py_module()`

```python
def utils.load_py_module(file: str, 
                         pre_exec: Optional[Callable[[ModuleType], None]] = None
                         ) -> ModuleType:
```

Loads a module from the given filename and assigns a unique module name to it.
Calling this function twice for the same file will yield distinct instances.

## <mark style="color:yellow;">`def`</mark> `utils.rank_sort()`

```python
def utils.rank_sort(vertices: Iterable[T], 
                    preds_of: Callable[[T], Iterable[T]], 
                    childs_of: Callable[[T], Iterable[T]]) -> dict[T, int]:
```

Calculates the top-down rank for each vertex. Supports graphs with multiple components.
The graph must not have any cycles or a CycleError will be thrown.

### Parameters

 -  **vertices**: A list of vertices

 -  **preds_of**: A function that returns a list of predecessors given a vertex

 -  **childs_of**: A function that returns a list of successors given a vertex

### Returns

 -  **dict[T, int]**: A dict associating a rank to each vertex

### Raises

 -  **CycleError**: The given graph is cyclic.

## <mark style="color:yellow;">`def`</mark> `utils.script_trace()`

```python
def utils.script_trace(script_stack: list[tuple[Any, inspect.FrameInfo]], 
                       include_root: bool = False) -> str:
```

Creates a script trace similar to a python backtrace.

### Parameters

 -  **script_stack**: The script stack to print

 -  **include_root**: Whether or not to include the root frame in the script trace.

## <mark style="color:yellow;">`def`</mark> `utils.print_exception()`

```python
def utils.print_exception(exc_type: Optional[Type[BaseException]], 
                          exc_info: Optional[BaseException], 
                          tb: Optional[TracebackType]) -> None:
```

A function that hook that prints an exception traceback beginning from the
last dynamically loaded module, but including a script stack so the error
location is more easily understood and printed in a cleaner way.

## <mark style="color:yellow;">`def`</mark> `utils.install_exception_hook()`

```python
def utils.install_exception_hook() -> None:
```

Installs a new global exception handler, that will modify the
traceback of exceptions raised from dynamically loaded modules
so that they are printed in a cleaner and more meaningful way (for the user).

## <mark style="color:yellow;">`def`</mark> `utils.import_submodules()`

```python
def utils.import_submodules(package: Union[str, ModuleType], 
                            recursive: bool = False
                            ) -> dict[str, ModuleType]:
```

Import all submodules of a module, possibly recursively including subpackages.

### Parameters

 -  **package**: The package to import all submodules from.

 -  **recursive**: Whether to recursively include subpackages.

### Returns

## <mark style="color:yellow;">`def`</mark> `utils.check_host_active()`

```python
def utils.check_host_active() -> None:
```

Asserts that an inventory has been loaded and a host is active.