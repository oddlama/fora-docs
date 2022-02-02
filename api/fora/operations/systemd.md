# fora.operations.systemd

Provides operations related to the systemd init system.

## Functions

### <mark style="color:yellow;">def</mark> `systemd.daemon_reload()`

```python
def systemd.daemon_reload(user_mode: bool = False, 
                          name: Optional[str] = None, check: bool = True, 
                          op: Operation = Operation.internal_use_only
                          ) -> OperationResult:
```

Manages a systemd unit.

#### Parameters

 -  **user_mode**: Whether `systemctl --user` should be used to make user specific changes.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

### <mark style="color:yellow;">def</mark> `systemd.service()`

```python
def systemd.service(service: str, state: Optional[str] = None, 
                    enabled: Optional[bool] = None, user_mode: bool = False, 
                    name: Optional[str] = None, check: bool = True, 
                    op: Operation = Operation.internal_use_only
                    ) -> OperationResult:
```

Manages a systemd unit.

#### Parameters

 -  **service**: The unit to manage.

 -  **state**: The desired state of the unit. Valid options are `started`, `restarted`, `reloaded` and `stopped`.
    If None, the service's current state will not be changed.

 -  **enabled**: Whether the unit should be started on boot.

 -  **user_mode**: Whether `systemctl --user` should be used to make user specific changes.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.
