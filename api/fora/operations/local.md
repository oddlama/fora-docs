# fora.operations.local

Provides operations that are related to the local system on which the fora scripts are executed.

## Functions

### <mark style="color:yellow;">def</mark> `local.script()`

```python
def local.script(script: str, recursive: bool = False, 
                 params: Optional[dict[str, Any]] = None, 
                 name: Optional[str] = None) -> None:
```

Executes the given script for the current host.
Useful to split functionality into smaller sub-scripts.

Scripts can take parameters. Parameters to scripts are passed by
supplying a `params` dictionary. The script declares its parameters
by annotating them. (The annotation then transparently extracts the
value from a separately passed global variable).

```python
@Params
class params:
    username: str
    website_title: str = "Default website title."

# Use a parameter
print(params.username)
```

#### Parameters

 -  **script**: The local path to the script to execute.

 -  **recursive**: Whether recursive calls should be allowed.

 -  **params**: The parameters for the script.

 -  **name**: The name for the script execution (used for logging).
