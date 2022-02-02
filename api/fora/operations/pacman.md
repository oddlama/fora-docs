# fora.operations.pacman

Provides operations related to the pacman package manager.

## <mark style="color:yellow;">def</mark> `pacman.package()`

```python
def pacman.package(packages: list[str], present: bool = True, 
                   opts: Optional[list[str]] = None, 
                   name: Optional[str] = None, check: bool = True, 
                   op: Operation = Operation.internal_use_only
                   ) -> OperationResult:
```

Adds or removes system packages with pacman.

### Parameters

 -  **packages**: The packages to modify.

 -  **present**: Whether the given package should be installed or uninstalled.

 -  **opts**: Extra options passed to pacman when installing/uninstalling.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.
