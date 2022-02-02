# fora.connectors.tunnel_dispatcher

Provides a stdin/stdout based protocol to safely dispatch commands and return their
results over any connection that forwards both stdin/stdout, as well as some other
needed remote system related utilities.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.RemoteOSError`

An exception type for remote OSErrors.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.Connection`

Represents a connection to this dispatcher via an input and output buffer.

### <mark style="color:yellow;">def</mark> `Connection.flush()`

```python
def Connection.flush(self) -> None:
```

Flushes the output buffer.

### <mark style="color:yellow;">def</mark> `Connection.read()`

```python
def Connection.read(self, count: int) -> bytes:
```

Reads exactly the given amount of bytes.

### <mark style="color:yellow;">def</mark> `Connection.write()`

```python
def Connection.write(self, data: bytes, count: int) -> None:
```

Writes exactly the given amount of bytes from data.

### <mark style="color:yellow;">def</mark> `Connection.write_packet()`

```python
def Connection.write_packet(self, packet: Any) -> None:
```

Writes the given packet.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketOk`

This packet is used by some requests as a generic successful status indicator.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketAck`

This packet is used to acknowledge a previous PacketCheckAlive packet.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketCheckAlive`

This packet is used to check whether a connection is alive.
The receiver must answer with PacketAck immediately.

### <mark style="color:yellow;">def</mark> `PacketCheckAlive.handle()`

```python
def PacketCheckAlive.handle(self, conn: Connection) -> None:
```

Responds with PacketAck.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketExit`

This packet is used to signal the server to close the connection and end the dispatcher.

### <mark style="color:yellow;">def</mark> `PacketExit.handle()`

```python
def PacketExit.handle(self, conn: Connection) -> None:
```

Signals the connection to close.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketOSError`

This packet is sent when an OSError occurs.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketInvalidField`

This packet is used when an invalid value was given in a previous packet.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketProcessCompleted`

This packet is used to return the results of a process.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketProcessError`

This packet is used to indicate an error when running a process or when running the preexec_fn.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketProcessRun`

This packet is used to run a process.

### <mark style="color:yellow;">def</mark> `PacketProcessRun.handle()`

```python
def PacketProcessRun.handle(self, conn: Connection) -> None:
```

Runs the requested command.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketStatResult`

This packet is used to return the results of a stat packet.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketStat`

This packet is used to retrieve information about a file or directory.

### <mark style="color:yellow;">def</mark> `PacketStat.handle()`

```python
def PacketStat.handle(self, conn: Connection) -> None:
```

Stats the requested path.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketResolveResult`

This packet is used to return the results of a resolve packet.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketResolveUser`

This packet is used to canonicalize a user name / uid and to ensure it exists.
If None is given, it queries the current user.

### <mark style="color:yellow;">def</mark> `PacketResolveUser.handle()`

```python
def PacketResolveUser.handle(self, conn: Connection) -> None:
```

Resolves the requested user.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketResolveGroup`

This packet is used to canonicalize a group name / gid and to ensure it exists.
If None is given, it queries the current group.

### <mark style="color:yellow;">def</mark> `PacketResolveGroup.handle()`

```python
def PacketResolveGroup.handle(self, conn: Connection) -> None:
```

Resolves the requested group.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketUpload`

This packet is used to upload the given content to the remote and save it as a file.
Overwrites existing files. Responds with PacketOk if saving was successful, or PacketInvalidField if any
field contained an invalid value.

### <mark style="color:yellow;">def</mark> `PacketUpload.handle()`

```python
def PacketUpload.handle(self, conn: Connection) -> None:
```

Saves the content under the given path.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketDownloadResult`

This packet is used to return the content of a file.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketDownload`

This packet is used to download the contents of a given file.
Responds with PacketDownloadResult if reading was successful, or PacketInvalidField if any
field contained an invalid value.

### <mark style="color:yellow;">def</mark> `PacketDownload.handle()`

```python
def PacketDownload.handle(self, conn: Connection) -> None:
```

Reads the file.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketUserEntry`

This packet is used to return information about a user.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the user

#### <mark style="color:yellow;">attr</mark> `uid`

```python
uid: i64
```

The numerical user id

#### <mark style="color:yellow;">attr</mark> `group`

```python
group: str
```

The name of the primary group

#### <mark style="color:yellow;">attr</mark> `gid`

```python
gid: i64
```

The numerical primary group id

#### <mark style="color:yellow;">attr</mark> `groups`

```python
groups: list[str]
```

All names of the supplementary groups this user belongs to

#### <mark style="color:yellow;">attr</mark> `password_hash`

```python
password_hash: Optional[str]
```

The password hash from shadow

#### <mark style="color:yellow;">attr</mark> `gecos`

```python
gecos: str
```

The comment (GECOS) field of the user

#### <mark style="color:yellow;">attr</mark> `home`

```python
home: str
```

The home directory of the user

#### <mark style="color:yellow;">attr</mark> `shell`

```python
shell: str
```

The default shell of the user

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketQueryUser`

This packet is used to get information about a group via pwd.getpw*.

### Attributes

#### <mark style="color:yellow;">attr</mark> `user`

```python
user: str
```

User name or decimal uid

#### <mark style="color:yellow;">attr</mark> `query_password_hash`

```python
query_password_hash: bool
```

Whether the current password hash from shadow should also be returned

### <mark style="color:yellow;">def</mark> `PacketQueryUser.handle()`

```python
def PacketQueryUser.handle(self, conn: Connection) -> None:
```

Queries the requested user.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketGroupEntry`

This packet is used to return information about a group.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the group

#### <mark style="color:yellow;">attr</mark> `gid`

```python
gid: i64
```

The numerical group id

#### <mark style="color:yellow;">attr</mark> `members`

```python
members: list[str]
```

All the group member's user names

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketQueryGroup`

This packet is used to get information about a group via grp.getgr*.

### Attributes

#### <mark style="color:yellow;">attr</mark> `group`

```python
group: str
```

Group name or decimal gid

### <mark style="color:yellow;">def</mark> `PacketQueryGroup.handle()`

```python
def PacketQueryGroup.handle(self, conn: Connection) -> None:
```

Queries the requested group.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketEnvironVar`

This packet is used to return an environment variable.

### Attributes

#### <mark style="color:yellow;">attr</mark> `value`

```python
value: Optional[str]
```

The value of the environment variable, if it was set.

## <mark style="color:red;">class</mark> `tunnel_dispatcher.PacketGetenv`

This packet is used to get an environment variable.

### Attributes

#### <mark style="color:yellow;">attr</mark> `key`

```python
key: str
```

The environment variable to retrieve

### <mark style="color:yellow;">def</mark> `PacketGetenv.handle()`

```python
def PacketGetenv.handle(self, conn: Connection) -> None:
```

Gets the requested environment variable.

## <mark style="color:yellow;">def</mark> `tunnel_dispatcher.Packet()`

```python
def tunnel_dispatcher.Packet(type: str) -> Callable[[Type[Any]], Any]:
```

Decorator for packet types. Registers the packet and generates read and write methods.

## <mark style="color:yellow;">def</mark> `tunnel_dispatcher.receive_packet()`

```python
def tunnel_dispatcher.receive_packet(conn: Connection, request: Any = None
        ) -> Any:
```

Receives the next packet from the given connection.

### Parameters

 -  **conn**: The connection

 -  **request**: The corresponding request packet, if any.

### Returns

 -  **Any**: The received packet

### Raises

 -  **RemoteOSError**: An OSError occurred on the remote host.

 -  **IOError**: When an issue on the connection occurs.

 -  **ValueError**: When an PacketInvalidField is received as the response and a corresponding request packet was given.
