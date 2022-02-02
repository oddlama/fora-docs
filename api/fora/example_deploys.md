# fora.example_deploys

Provides example deploys, which can be used as a starting point.

## <mark style="color:yellow;">def</mark> `example_deploys.init_structure_minimal()`

```python
def example_deploys.init_structure_minimal() -> None:
```

Creates a minimal deploy structure.

## <mark style="color:yellow;">def</mark> `example_deploys.init_structure_flat()`

```python
def example_deploys.init_structure_flat() -> None:
```

Creates a flat deploy structure.

## <mark style="color:yellow;">def</mark> `example_deploys.init_structure_dotfiles()`

```python
def example_deploys.init_structure_dotfiles() -> None:
```

Creates a dotfiles deploy structure.

## <mark style="color:yellow;">def</mark> `example_deploys.init_structure_modular()`

```python
def example_deploys.init_structure_modular() -> None:
```

Creates a modular deploy structure.

## <mark style="color:yellow;">def</mark> `example_deploys.init_structure_staging_prod()`

```python
def example_deploys.init_structure_staging_prod() -> None:
```

Creates a staging_prod deploy structure.

## <mark style="color:yellow;">def</mark> `example_deploys.init_deploy_structure()`

```python
def example_deploys.init_deploy_structure(
        layout: Literal['minimal', 'flat', 'dotfiles', 'modular', 'staging_prod']
        ) -> NoReturn:
```

Initializes the current directory with a default deploy structure, if it is empty.
Prompts the user to confirm operation if the current directory is not empty.

### Parameters

 -  **layout**: The layout for the deploy.

### Raises

 -  **ValueError**: Invalid layout.
