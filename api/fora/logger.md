# fora.logger

Provides logging utilities.

## Attributes

### <mark style="color:yellow;">attr</mark> `logger.state`

```python
logger.state: State = State()
```

The global logger state.

## <mark style="color:red;">class</mark> `logger.State`

Global state for logging.

### Attributes

#### <mark style="color:yellow;">attr</mark> `indentation_level`

```python
indentation_level: int = 0
```

The current global indentation level.

## <mark style="color:red;">class</mark> `logger.IndentationContext`

A context manager to modify the indentation level.

## Functions

### <mark style="color:yellow;">def</mark> `logger.use_color()`

```python
def logger.use_color() -> bool:
```

Returns true if color should be used.

### <mark style="color:yellow;">def</mark> `logger.col()`

```python
def logger.col(color_code: str) -> str:
```

Returns the given argument only if color is enabled.

### <mark style="color:yellow;">def</mark> `logger.ellipsis()`

```python
def logger.ellipsis(s: str, width: int) -> str:
```

Shrinks the given string to width (including an ellipsis character).

#### Parameters

 -  **s**: The string.

 -  **width**: The maximum width.

#### Returns

 -  **str**: A modified string with at most `width` characters.

### <mark style="color:yellow;">def</mark> `logger.indent()`

```python
def logger.indent() -> IndentationContext:
```

Retruns a context manager that increases the indentation level.

### <mark style="color:yellow;">def</mark> `logger.indent_prefix()`

```python
def logger.indent_prefix() -> str:
```

Returns the indentation prefix for the current indentation level.

### <mark style="color:yellow;">def</mark> `logger.debug()`

```python
def logger.debug(msg: str) -> None:
```

Prints the given message only in debug mode.

### <mark style="color:yellow;">def</mark> `logger.debug_args()`

```python
def logger.debug_args(msg: str, args: dict[str, Any]) -> None:
```

Prints all given arguments when in debug mode.

### <mark style="color:yellow;">def</mark> `logger.print_indented()`

```python
def logger.print_indented(msg: str, **kwargs: Any) -> None:
```

Same as print(), but prefixes the message with the indentation prefix.

### <mark style="color:yellow;">def</mark> `logger.connection_init()`

```python
def logger.connection_init(connector: Any) -> None:
```

Prints connection initialization information.

### <mark style="color:yellow;">def</mark> `logger.connection_failed()`

```python
def logger.connection_failed(error_msg: str) -> None:
```

Signals that an error has occurred while establishing the connection.

### <mark style="color:yellow;">def</mark> `logger.connection_established()`

```python
def logger.connection_established() -> None:
```

Signals that the connection has been successfully established.

### <mark style="color:yellow;">def</mark> `logger.run_script()`

```python
def logger.run_script(script: str, name: Optional[str] = None) -> None:
```

Prints the script file and name that is being executed next.

### <mark style="color:yellow;">def</mark> `logger.print_operation_title()`

```python
def logger.print_operation_title(op: Any, title_color: str, end: str = '\n'
        ) -> None:
```

Prints the operation title and description.

### <mark style="color:yellow;">def</mark> `logger.print_operation_early()`

```python
def logger.print_operation_early(op: Any) -> None:
```

Prints the operation title and description before the final status is known.

### <mark style="color:yellow;">def</mark> `logger.decode_escape()`

```python
def logger.decode_escape(data: bytes, encoding: str = 'utf-8') -> str:
```

Tries to decode the given data with the given encoding, but replaces all non-decodeable
and non-printable characters with backslash escape sequences.

Example:

```python
>>> decode_escape(b'It is Wednesday\nmy dudes\r\n🐸\xff\0')
'It is Wednesday\\nMy Dudes\\r\\n🐸\\xff\\0'
```

#### Parameters

 -  **content**: The content that should be decoded and escaped.

 -  **encoding**: The encoding that should be tried. To preserve utf-8 symbols, use 'utf-8',
    to replace any non-ascii character with an escape sequence use 'ascii'.

#### Returns

 -  **str**: The decoded and escaped string.

### <mark style="color:yellow;">def</mark> `logger.diff()`

```python
def logger.diff(filename: str, old: Optional[bytes], new: Optional[bytes], 
                color: bool = True) -> list[str]:
```

Creates a diff between the old and new content of the given filename,
that can be printed to the console. This function returns the diff
output as an array of lines. The lines in the output array are not
terminated by newlines.

If color is True, the diff is colored using ANSI escape sequences.

If you want to provide an alternative diffing function, beware that
the input can theoretically contain any bytes and therefore should
be decoded as utf-8 if possible, but non-decodeable
or non-printable charaters should be replaced with human readable
variants such as `\x00`, `^@` or similar represenations.

Your diffing function should still be able to work on the raw bytes
representation, after you aquire the diff and before you apply colors,
your output should be made printable with a function such as [`logger.decode_escape()`](#def-logger.decode\_escape):

```python
# First decode and escape
line = logger.decode_escape(byteline)
# Add coloring afterwards so ANSI escape sequences are not escaped
```

#### Parameters

 -  **filename**: The filename of the file that is being diffed.

 -  **old**: The old content, or None if the file didn't exist before.

 -  **new**: The new content, or None if the file was deleted.

 -  **color**: Whether the output should be colored (with ANSI color sequences).

#### Returns

 -  **list[str]**: The lines of the diff output. The individual lines will not have a terminating newline.

### <mark style="color:yellow;">def</mark> `logger.print_operation()`

```python
def logger.print_operation(op: Any, result: Any) -> None:
```

Prints the operation summary after it has finished execution.
