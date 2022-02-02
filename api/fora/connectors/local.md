# fora.connectors.local

Contains a connector which handles connections to hosts via SSH.

## <mark style="color:red;">class</mark> `local.LocalConnector`

A tunnel connector that provides remote access to the current local machine via a subprocess.

### <mark style="color:yellow;">def</mark> `command()`

```python
def command(self) -> list[str]:
```

Constructs the full command needed to execute a tunnel dispatcher on this machine.

#### Returns

 -  **list[str]**: The required ssh command.

### <mark style="color:yellow;">def</mark> `extract_hostname()`

```python
def extract_hostname(cls, url: str) -> str:
```
