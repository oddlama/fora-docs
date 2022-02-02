# fora.main

Provides the top-level logic of fora such as
the CLI interface and main script dispatching.

## <mark style="color:red;">class</mark> `main.ArgumentParserError`

Error class for argument parsing errors.

## <mark style="color:red;">class</mark> `main.ThrowingArgumentParser`

An argument parser that throws when invalid argument types are passed.

### <mark style="color:yellow;">def</mark> `error()`

```python
def error(self, message: str) -> NoReturn:
```

Raises an exception on error.

## <mark style="color:red;">class</mark> `main.ActionImmediateFunction`

An action that calls a function immediately when the argument is encountered.

## <mark style="color:yellow;">def</mark> `main.main_run()`

```python
def main.main_run(args: argparse.Namespace) -> None:
```

Main method used to run a script on an inventory.

### Parameters

 -  **args**: The parsed arguments

## <mark style="color:yellow;">def</mark> `main.show_inventory()`

```python
def main.show_inventory(inventory: str) -> None:
```

Display a summary of the given inventory.

### Parameters

 -  **inventory**: The inventory argument

## <mark style="color:yellow;">def</mark> `main.main()`

```python
def main.main(argv: Optional[list[str]] = None) -> None:
```

The main program entry point. This will parse arguments, load inventory and task
definitions and run the given user script. Defaults to sys.argv[1:] if argv is None.
