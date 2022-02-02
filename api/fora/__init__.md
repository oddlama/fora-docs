# fora

The main module of fora.

## Submodules

 -  **fora.utils**: Provides utility functions.

 -  **fora.version**: *No description.*

 -  **fora.main**: Provides the top-level logic of fora such as
    the CLI interface and main script dispatching.

 -  **fora.remote_settings**: Provides a class that represents execution defaults for a remote host.

 -  **fora.loader**: Provides the dynamic module loading utilities.

 -  **fora.inventory_wrapper**: Provides the inventory wrapper for all inventory related functionality.

 -  **fora.connection**: Provides a class to manage a remote connection via the host's connector.
    Stores state along with the connection.

 -  **fora.types**: Provides a mockup of loadable module types. They are used to store metadata
    that can be accessed by the module that is currently being loaded. These
    types also help the static type checker, as it then has a better understanding
    of the expected contents of the dynamically loaded modules.

 -  **fora.logger**: Provides logging utilities.

 -  **fora.example_deploys**: Provides example deploys, which can be used as a starting point.

## Attributes

### <mark style="color:yellow;">`attr`</mark>` fora.args`

```python
fora.args: argparse.Namespace = cast(argparse.Namespace, None)
```

The global logger. Should be used for all user-facing information logging to ensure
that this information is displayed in a proper format and according to the user's
verbosity preferences.

### <mark style="color:yellow;">`attr`</mark>` fora.inventory`

```python
fora.inventory: InventoryWrapper = cast('InventoryWrapper', None)
```

The inventory module we are operating on.
This is loaded from the inventory definition file.

### <mark style="color:yellow;">`attr`</mark>` fora.group`

```python
fora.group: GroupWrapper = cast('GroupWrapper', None)
```

This variable wraps the currently loaded group module.
It must not be accessed anywhere else but inside the
definition (source) of the actual group module.

### <mark style="color:yellow;">`attr`</mark>` fora.host`

```python
fora.host: HostWrapper = cast('HostWrapper', None)
```

This variable wraps the currently loaded hosts module (in case a host is just being defined),
or the currently active host while executing a script. It must not be used anywhere else
but inside the definition (source) of the actual module or inside of a script.

### <mark style="color:yellow;">`attr`</mark>` fora.script`

```python
fora.script: ScriptWrapper = cast('ScriptWrapper', None)
```

This variable wraps the currently executed script module (if any).