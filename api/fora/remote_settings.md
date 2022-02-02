# fora.remote_settings

Provides a class that represents execution defaults for a remote host.

## <mark style="color:red;">class</mark> `remote_settings.ResolvedRemoteSettings`

This class stores a resolved version of the RemoteSettings object,
it only has more strict types for typechecking and is otherwise
identical to the original object.

## <mark style="color:red;">class</mark> `remote_settings.RemoteSettings`

This class stores certain values that determine how things are executed on
the remote host. This includes things such as the owner and group of newly
created files, or the user as which commands are run.

### <mark style="color:yellow;">def</mark> `RemoteSettings.overlay()`

```python
def RemoteSettings.overlay(self, settings: RemoteSettings
                           ) -> RemoteSettings:
```

Overlays settings on top of this. Values will only be overwritten
if the new value is not None, effectively overlaying the given settings
on top of the current settings.

#### Parameters

 -  **settings**: The setting values to overwrite

#### Returns
