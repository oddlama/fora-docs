# fora.types

## <mark style="color:red;">class</mark> types.ScriptWrapper
## <mark style="color:gray;">class</mark> types.ScriptWrapper
## <mark style="color:grey;">class</mark> types.ScriptWrapper
## <mark style="color:black;">class</mark> types.ScriptWrapper

A mockup type for script modules. This is not the actual type of an instanciated module, but will reflect some of it's properties better than ModuleType. While this class is mainly used to aid type-checking, its properties are transferred to the actual instanciated module before the module is executed.

When writing a script module, you can use the API exposed in `fora.script` to access/change meta information about your module.

### <mark style="color:red;">def</mark> defaults()

```python
def files.line(path: str, line: str,
        present: bool = True,
        regex: Optional[str] = None,
        ignore_whitespace: bool = True,
        backup: Union[bool, str] = False,
        name: Optional[str] = None,
        check: bool = True,
        op: Operation = Operation.internal_use_only
    ) -> OperationResult:
```

