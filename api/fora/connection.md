# fora.connection

Provides a class to manage a remote connection via the host's connector.
Stores state along with the connection.

## <mark style="color:red;">`class`</mark>` connection.Connection`

The connection class represents a connection to a host.
It consists of a connector, which is actually responsible for
providing remote access, and some state, which determines defaults
for the commands executed on the remote system.

### <mark style="color:yellow;">`def`</mark> `resolve_defaults()`

```python
def resolve_defaults(self, settings: RemoteSettings) -> RemoteSettings:
```

Resolves (and verifies) the given settings against the current defaults,
and returns tha actual values that should now be in effect. Verification
means that this method will fail if e.g. the cwd doesn't exist on the remote.

#### Parameters

 -  **settings**: Additional overrides for the current defaults

#### Returns

 -  **RemoteSettings**: The resolved settings

### <mark style="color:yellow;">`def`</mark> `run()`

```python
def run(self, command: list[str], input: Optional[bytes] = None, 
        capture_output: bool = True, check: bool = True, 
        user: Optional[str] = None, group: Optional[str] = None, 
        umask: Optional[str] = None, cwd: Optional[str] = None
        ) -> CompletedRemoteCommand:
```

See [`Connector.run()`](api/fora/connectors/connector.md#Connector.run).

### <mark style="color:yellow;">`def`</mark> `resolve_user()`

```python
def resolve_user(self, user: Optional[str]) -> str:
```

See [`Connector.resolve_user()`](api/fora/connectors/connector.md#Connector.resolve_user).

### <mark style="color:yellow;">`def`</mark> `resolve_group()`

```python
def resolve_group(self, group: Optional[str]) -> str:
```

See [`Connector.resolve_group()`](api/fora/connectors/connector.md#Connector.resolve_group).

### <mark style="color:yellow;">`def`</mark> `stat()`

```python
def stat(self, path: str, follow_links: bool = False, 
         sha512sum: bool = False) -> Optional[StatResult]:
```

See [`Connector.stat()`](api/fora/connectors/connector.md#Connector.stat).

### <mark style="color:yellow;">`def`</mark> `upload()`

```python
def upload(self, file: str, content: bytes, mode: Optional[str] = None, 
           owner: Optional[str] = None, group: Optional[str] = None
           ) -> None:
```

See [`Connector.upload()`](api/fora/connectors/connector.md#Connector.upload).

### <mark style="color:yellow;">`def`</mark> `download()`

```python
def download(self, file: str) -> bytes:
```

See [`Connector.download()`](api/fora/connectors/connector.md#Connector.download).

### <mark style="color:yellow;">`def`</mark> `download_or()`

```python
def download_or(self, file: str, default: Optional[bytes] = None
                ) -> Optional[bytes]:
```

Same as [`Connection.download()`](api/fora/connection.md#Connection.download), but returns the given default in case the file doesn't exist.

#### Parameters

 -  **file**: The file to download.

 -  **default**: The alternative to return if the file doesn't exist.

#### Returns

 -  **Optional[bytes]**: The downloaded file or the default if the file didn't exist.

#### Raises

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails for any reason other than file not found.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">`def`</mark> `query_user()`

```python
def query_user(self, user: str, query_password_hash: bool = False, 
               default: Optional[UserEntry] = None) -> Optional[UserEntry]:
```

See [`Connector.query_user()`](api/fora/connectors/connector.md#Connector.query_user), but returns the given default in case the user doesn't exist.

### <mark style="color:yellow;">`def`</mark> `query_group()`

```python
def query_group(self, group: str, default: Optional[GroupEntry] = None
                ) -> Optional[GroupEntry]:
```

See [`Connector.query_group()`](api/fora/connectors/connector.md#Connector.query_group), but returns the given default in case the group doesn't exist.

### <mark style="color:yellow;">`def`</mark> `home_dir()`

```python
def home_dir(self, user: Optional[str] = None) -> str:
```

Return's the home directory of the given user. If the user is None,
it defaults to the current user.

#### Parameters

 -  **user**: The user.

#### Returns

 -  **str**: The home directory of the requested user.

#### Raises

 -  **ValueError**: If the user could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">`def`</mark> `getenv()`

```python
def getenv(self, key: str, default: Optional[str] = None) -> Optional[str]:
```

See [`Connector.getenv()`](api/fora/connectors/connector.md#Connector.getenv), but returns the given default in case the key doesn't exist.

## <mark style="color:yellow;">`def`</mark> `connection.open_connection()`

```python
def connection.open_connection(host: HostWrapper) -> Connection:
```

Returns a connection (context manager) that opens the connection when it is entered and
closes it when it is exited. The connection can be obtained via host.connection,
as long as it is opened.

### Parameters

 -  **host**: The host to which a connection should be opened

### Returns

 -  **Connection**: The connection (context manager)