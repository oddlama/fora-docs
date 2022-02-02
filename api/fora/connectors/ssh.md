# fora.connectors.ssh

Contains a connector which handles connections to hosts via SSH.

## <mark style="color:red;">class</mark> `ssh.SshConnector`

A tunnel connector that provides remote access via SSH.

### <mark style="color:yellow;">def</mark> `SshConnector.command()`

```python
def SshConnector.command(self) -> list[str]:
```

Constructs the full ssh command needed to execute a
tunnel dispatcher on the remote host.

#### Returns

 -  **list[str]**: The required ssh command.

### <mark style="color:yellow;">def</mark> `SshConnector.extract_hostname()`

```python
def SshConnector.extract_hostname(cls, url: str) -> str:
```
