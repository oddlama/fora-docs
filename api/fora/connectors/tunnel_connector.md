# fora.connectors.tunnel_connector

Contains a connector base which handles communication via any spawned subprocess command that can run a tunnel dispatcher on the remote host.

## <mark style="color:red;">`class`</mark>` tunnel_connector.TunnelConnector`

A connector that handles requests via an externally supplied subprocess running a tunnel dispatcher.
Any subclass must override command().

### <mark style="color:yellow;">`def`</mark> `command()`

```python
def command(self) -> list[str]:
```

Returns the command that should be executed to open a tunnel dispatcher to the destination.

### <mark style="color:yellow;">`def`</mark> `open()`

```python
def open(self) -> None:
```

### <mark style="color:yellow;">`def`</mark> `close()`

```python
def close(self) -> None:
```

### <mark style="color:yellow;">`def`</mark> `run()`

```python
def run(self, command: list[str], input: Optional[bytes] = None, 
        capture_output: bool = True, check: bool = True, 
        user: Optional[str] = None, group: Optional[str] = None, 
        umask: Optional[str] = None, cwd: Optional[str] = None
        ) -> CompletedRemoteCommand:
```

### <mark style="color:yellow;">`def`</mark> `stat()`

```python
def stat(self, path: str, follow_links: bool = False, 
         sha512sum: bool = False) -> Optional[StatResult]:
```

### <mark style="color:yellow;">`def`</mark> `resolve_user()`

```python
def resolve_user(self, user: Optional[str]) -> str:
```

### <mark style="color:yellow;">`def`</mark> `resolve_group()`

```python
def resolve_group(self, group: Optional[str]) -> str:
```

### <mark style="color:yellow;">`def`</mark> `query_user()`

```python
def query_user(self, user: str, query_password_hash: bool = False
               ) -> UserEntry:
```

### <mark style="color:yellow;">`def`</mark> `query_group()`

```python
def query_group(self, group: str) -> GroupEntry:
```

### <mark style="color:yellow;">`def`</mark> `getenv()`

```python
def getenv(self, key: str) -> Optional[str]:
```

### <mark style="color:yellow;">`def`</mark> `upload()`

```python
def upload(self, file: str, content: bytes, mode: Optional[str] = None, 
           owner: Optional[str] = None, group: Optional[str] = None
           ) -> None:
```

### <mark style="color:yellow;">`def`</mark> `download()`

```python
def download(self, file: str) -> bytes:
```