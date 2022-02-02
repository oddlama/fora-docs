# fora.operations.system

Provides operations related to the operating system such as user, group or service management.

## Functions

### <mark style="color:yellow;">def</mark> `system.user()`

```python
def system.user(user: str, present: bool = True, uid: Optional[int] = None, 
                group: Optional[str] = None, 
                groups: Optional[list[str]] = None, 
                append_groups: bool = False, system: bool = False, 
                password_hash: Optional[str] = None, 
                home: Optional[str] = None, shell: Optional[str] = None, 
                comment: Optional[str] = None, name: Optional[str] = None, 
                check: bool = True, 
                op: Operation = Operation.internal_use_only
                ) -> OperationResult:
```

Creates, modifies or deletes a unix user.

The home directory for the given user will never be created.
Use [`files.directory()`](files.md#def-files.directory) to do this.

When a user is deleted, it's primary group will also be deleted if no other user
has the same primary group. This is a quirk of the `userdel` tool, and applies when
USERGROUPS_ENAB is set to yes in `/etc/login.defs` (which is the case on most distributions).

Similarly, when a user is created and no primary group is specified, a new primary group with
the same name as the user will be created for it. As for deletion, this only applies when
USERGROUPS_ENAB is set to yes in `/etc/login.defs`.

You can generate a password hash by using the following code:

```python
import crypt, getpass
real_pw = getpass.getpass()
password_hash = crypt.crypt(real_pw, crypt.mksalt(crypt.METHOD_SHA512))
```

### Example:

```python
system.user(
    name="Create a new user for some service including a group of the same name",
    user="testuser")

system.user(
    name="Create a new user with an existing primary group",
    user="testuser",
    group="users")

system.user(
    name="Add myuser to the video group",
    user="myuser",
    groups=["video"],
    append_groups=True)

system.user(
    name="Delete testuser. Will also delete the corresponding primary group if it isn't used for anything else",
    user="testuser",
    present=False)
```

#### Parameters

 -  **user**: The name of the user.

 -  **present**: Whether the given user should exists. If False any existing user with that name will be deleted and all other parameters ignored.

 -  **uid**: The uid for the user. Automatically determined if not specified.

 -  **group**: The primary group (name or gid) for the user. If given, the group must already exists.
    Otherwise, a group will be created with the same name as the user, if USERGROUPS_ENAB is set in /etc/login.defs.

 -  **groups**: Secondary groups for the user.

 -  **append_groups**: Only applies when `groups` were given.
    If `False`, the user will be removed from any groups other than the given ones.
    Otherwise, the user will be appended to the given groups.

 -  **system**: If `True` the user will be created as a system user. This doesn't affect existing users.

 -  **password_hash**: The password hash for the user as given by `crypt(3)`.
    Defaults to '!' if not given but a user needs to be created.

 -  **home**: The home directory for the user. Defaults to `/dev/null` if not given but a user needs to be created.

 -  **shell**: Specifies the shell for the user. Defaults to `/sbin/nologin` if not given but a user needs to be created.

 -  **comment**: Specifies the GECOS comment for the user. Will be left empty if not given but a user needs to be created.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

### <mark style="color:yellow;">def</mark> `system.group()`

```python
def system.group(group: str, present: bool = True, 
                 gid: Optional[int] = None, system: bool = False, 
                 name: Optional[str] = None, check: bool = True, 
                 op: Operation = Operation.internal_use_only
                 ) -> OperationResult:
```

Creates, modifies or deletes a unix group.

### Example:

```python
system.group(
    name="Create a new group",
    user="testgroup")
```

#### Parameters

 -  **group**: The name of the group.

 -  **present**: Whether the given group should exists. If False any existing group with that name will be deleted and all other parameters ignored.

 -  **gid**: The gid for the group. Automatically determined if not specified.

 -  **system**: If `True` the group will be created as a system group. This doesn't affect existing groups.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

### <mark style="color:yellow;">def</mark> `system.package()`

```python
def system.package(packages: list[str], present: bool = True, 
                   name: Optional[str] = None, check: bool = True
                   ) -> OperationResult:
```

Adds or removes system packages by detecting a supported init system to execute the operation.

### Example:

```python
system.package(
    name="Install htop",
    packages=["htop"])

system.package(
    name="Install neovim and git",
    packages=["neovim", "git"])

system.package(
    name="Uninstall nano",
    packages=["nano"],
    present=False)
```

#### Parameters

 -  **packages**: The packages to modify.

 -  **present**: Whether the given package should be installed or uninstalled.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

### <mark style="color:yellow;">def</mark> `system.service()`

```python
def system.service(service: str, state: Optional[str] = None, 
                   enabled: Optional[bool] = None, 
                   name: Optional[str] = None, check: bool = True
                   ) -> OperationResult:
```

Manages a system service by detecting a supported init system to execute the operation.

### Example:

```python
system.service(
    name="Enable sshd to start on boot, and ensure it is started now",
    service="sshd",
    state="started",
    enable=True)

system.service(
    name="Just enable sshd to start on boot, but don't change anything about its current state",
    service="sshd",
    enable=True)

system.service(
    name="Restart the nginx service now",
    service="nginx",
    state="restarted")
```

#### Parameters

 -  **service**: The unit to manage.

 -  **state**: The desired state of the unit. Valid options are `started`, `restarted`, `reloaded` and `stopped`.
    If None, the service's current state will not be changed.

 -  **enabled**: Whether the unit should be started on boot.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.
