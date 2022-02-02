# fora.connectors.tunnel_connector

Contains a connector base which handles communication via any spawned subprocess command that can run a tunnel dispatcher on the remote host.

## <mark style="color:red;">class</mark> `tunnel_connector.TunnelConnector`

A connector that handles requests via an externally supplied subprocess running a tunnel dispatcher.
Any subclass must override command().

### <mark style="color:yellow;">def</mark> `TunnelConnector.command()`

```python
def TunnelConnector.command(self) -> list[str]:
```

Returns the command that should be executed to open a tunnel dispatcher to the destination.

### <mark style="color:yellow;">def</mark> `TunnelConnector.open()`

```python
def TunnelConnector.open(self) -> None:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.close()`

```python
def TunnelConnector.close(self) -> None:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.run()`

```python
def TunnelConnector.run(self, command: list[str], 
                        input: Optional[bytes] = None, 
                        capture_output: bool = True, check: bool = True, 
                        user: Optional[str] = None, 
                        group: Optional[str] = None, 
                        umask: Optional[str] = None, 
                        cwd: Optional[str] = None
                        ) -> CompletedRemoteCommand:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.stat()`

```python
def TunnelConnector.stat(self, path: str, follow_links: bool = False, 
                         sha512sum: bool = False) -> Optional[StatResult]:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.resolve_user()`

```python
def TunnelConnector.resolve_user(self, user: Optional[str]) -> str:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.resolve_group()`

```python
def TunnelConnector.resolve_group(self, group: Optional[str]) -> str:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.query_user()`

```python
def TunnelConnector.query_user(self, user: str, 
                               query_password_hash: bool = False
                               ) -> UserEntry:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.query_group()`

```python
def TunnelConnector.query_group(self, group: str) -> GroupEntry:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.getenv()`

```python
def TunnelConnector.getenv(self, key: str) -> Optional[str]:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.upload()`

```python
def TunnelConnector.upload(self, file: str, content: bytes, 
                           mode: Optional[str] = None, 
                           owner: Optional[str] = None, 
                           group: Optional[str] = None) -> None:
```

### <mark style="color:yellow;">def</mark> `TunnelConnector.download()`

```python
def TunnelConnector.download(self, file: str) -> bytes:
```
