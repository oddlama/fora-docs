# Scripts

### Using operations

### Accessing the host directly

```python3
# Check whether the host belongs to a specific group. This includes transitive group dependencies.
if "desktops" in host.groups:
	# ...

# Execute a command on the remote
print(host.connection.run(["echo", "Hello from the other side"]).stdout.decode("utf-8"))
```

### Default variables

### Parameters

### Remote defaults
