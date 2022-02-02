# operations.api

Provides API to define operations.

## <mark style="color:red;">class</mark> `api.OperationError`

An [exception ](git.md#def-git.repo)that indicates an error while executing an operation.

## <mark style="color:red;">class</mark> `api.OperationResult`

Stores the result of an operation.

### Attributes

#### <mark style="color:yellow;">attr</mark> `success`

```python
success: bool
```

Whether the operation succeeded.

#### <mark style="color:yellow;">attr</mark> `changed`

```python
changed: bool
```

Whether the operation changed something.

#### <mark style="color:yellow;">attr</mark> `initial`

```python
initial: dict[str, Any]
```

The initial state of the host.

#### <mark style="color:yellow;">attr</mark> `final`

```python
final: dict[str, Any]
```

The final state of the host.

#### <mark style="color:yellow;">attr</mark> `failure_message`

```python
failure_message: Optional[str] = None
```

The failure message, if success is False.

## <mark style="color:red;">class</mark> `api.Operation`

This class is used to ease the building of operations with consistent output and state tracking.

### Attributes

#### <mark style="color:yellow;">attr</mark> `internal_use_only`

```python
internal_use_only: 'Operation' = cast('Operation', None)
```

operation's op variable is defaulted to this value to indicate that it must not be given by the user.

### <mark style="color:yellow;">def</mark> `Operation.nested()`

```python
def Operation.nested(self, has_nested: bool) -> None:
```

Sets whet this operation spawns nested operations. In this case, this operation will not have separate state, and the printing will be handled differently.

#### Parameters

* **has\_nested**: Whether the operation has nested operations.

### <mark style="color:yellow;">def</mark> `Operation.add_nested_result()`

```python
def Operation.add_nested_result(self, key: str, result: OperationResult
                                ) -> None:
```

Adds initial and final state of a nested operation under the given key into this operation's state dictionaries.

#### Parameters

* **key**: The key under which to add the nested result.
* **result**: The result to add.

### <mark style="color:yellow;">def</mark> `Operation.desc()`

```python
def Operation.desc(self, description: str) -> None:
```

Sets the description of the operation, and prints an early status via the logger.

#### Parameters

* **description**: The new description.

### <mark style="color:yellow;">def</mark> `Operation.defaults()`

```python
def Operation.defaults(self, *args: Any, **kwargs: Any
                       ) -> RemoteDefaultsContext:
```

Sets defaults on the current script. See [`ScriptWrapper.defaults()`](api/fora/types.md#def-ScriptWrapper.defaults).

### <mark style="color:yellow;">def</mark> `Operation.initial_state()`

```python
def Operation.initial_state(self, **kwargs: Any) -> None:
```

Sets the initial state.

### <mark style="color:yellow;">def</mark> `Operation.final_state()`

```python
def Operation.final_state(self, **kwargs: Any) -> None:
```

Sets the final state.

### <mark style="color:yellow;">def</mark> `Operation.unchanged()`

```python
def Operation.unchanged(self, ignore_none: bool = False) -> bool:
```

Checks whether the initial and final states differ.

#### Parameters

* **ignore\_none**: Set to `True` to not count states where the final value is None.

#### Returns

* **bool**: Whether the states differ.

### <mark style="color:yellow;">def</mark> `Operation.changed()`

```python
def Operation.changed(self, key: str) -> bool:
```

Checks whether a specific key will change.

#### Parameters

* **key**: The key to check for changes.

#### Returns

* **bool**: Whether the states differ.

### <mark style="color:yellow;">def</mark> `Operation.diff()`

```python
def Operation.diff(self, file: str, old: Optional[bytes], 
                   new: Optional[bytes]) -> None:
```

Adds a file to the diffing output.

#### Parameters

* **file**: The filename which the diff belongs to.
* **old**: The previous content or None if the file didn't exist previously.
* **new**: The new content or None if the file was deleted.

### <mark style="color:yellow;">def</mark> `Operation.failure()`

```python
def Operation.failure(self, msg: str) -> OperationResult:
```

Returns a failed operation result.

#### Returns

* **OperationResult**: The OperationResult for this failed operation.

### <mark style="color:yellow;">def</mark> `Operation.success()`

```python
def Operation.success(self) -> OperationResult:
```

Returns a successful operation result.

#### Returns

* **OperationResult**: The OperationResult for this successful operation.

## Functions

### <mark style="color:yellow;">def</mark> `api.operation()`

```python
def api.operation(op_name: str):
```

Operation function decorator.
