# fora.connectors.connector

Defines the connector interface.

## <mark style="color:red;">class</mark> `connector.CompletedRemoteCommand`

The return value of `Connector.run()`, representing a finished remote process.

### Attributes

#### <mark style="color:yellow;">attr</mark> `stdout`

```python
stdout: Optional[bytes]
```

The stdout of the remote command. If you need a string, but are not sure that the formatting is utf-8,
    be sure to decode with `errors="surrogateescape"` or `errors=backslashreplace` depending on your use.

#### <mark style="color:yellow;">attr</mark> `stderr`

```python
stderr: Optional[bytes]
```

The stderr of the remote command. If you need a string, but are not sure that the formatting is utf-8,
    be sure to decode with `errors="surrogateescape"` or `errors=backslashreplace` depending on your use.

#### <mark style="color:yellow;">attr</mark> `returncode`

```python
returncode: int
```

The return code of the remote command.

## <mark style="color:red;">class</mark> `connector.StatResult`

The return value of stat(), representing information about a remote file.
The type will be one of [ "dir", "chr", "blk", "file", "fifo", "link", "sock", "other" ].
If requested, the sha512sum of the file will be included.

## <mark style="color:red;">class</mark> `connector.UserEntry`

The result of a user query.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the user

#### <mark style="color:yellow;">attr</mark> `uid`

```python
uid: int
```

The numerical user id

#### <mark style="color:yellow;">attr</mark> `group`

```python
group: str
```

The name of the primary group

#### <mark style="color:yellow;">attr</mark> `gid`

```python
gid: int
```

The numerical primary group id

#### <mark style="color:yellow;">attr</mark> `groups`

```python
groups: list[str]
```

All names of the supplementary groups this user belongs to

#### <mark style="color:yellow;">attr</mark> `password_hash`

```python
password_hash: Optional[str]
```

The password hash from shadow, if requested.

#### <mark style="color:yellow;">attr</mark> `gecos`

```python
gecos: str
```

The comment (GECOS) field of the user

#### <mark style="color:yellow;">attr</mark> `home`

```python
home: str
```

The home directory of the user

#### <mark style="color:yellow;">attr</mark> `shell`

```python
shell: str
```

The default shell of the user

## <mark style="color:red;">class</mark> `connector.GroupEntry`

The result of a group query.

### Attributes

#### <mark style="color:yellow;">attr</mark> `name`

```python
name: str
```

The name of the group

#### <mark style="color:yellow;">attr</mark> `gid`

```python
gid: int
```

The numerical group id

#### <mark style="color:yellow;">attr</mark> `members`

```python
members: list[str]
```

All the group member's user names

## <mark style="color:red;">class</mark> `connector.Connector`

The base class for all connectors.

### Attributes

#### <mark style="color:yellow;">attr</mark> `schema`

```python
schema: str
```

The schema of the connector. Must match the schema used in urls of this connector,
such as `ssh` for `ssh:...`. May also appear in log messages. A schema is the part
of the url until (but not including) the first colon. Set by the @connector decorator.

#### <mark style="color:yellow;">attr</mark> `registered_connectors`

```python
registered_connectors: dict[str, Type[Connector]] = {}
```

The list of all registered connectors.

### <mark style="color:yellow;">def</mark> `Connector.open()`

```python
def Connector.open(self) -> None:
```

Opens the connection to the remote host.

### <mark style="color:yellow;">def</mark> `Connector.close()`

```python
def Connector.close(self) -> None:
```

Closes the connection to the remote host.

### <mark style="color:yellow;">def</mark> `Connector.run()`

```python
def Connector.run(self, command: list[str], input: Optional[bytes] = None, 
                  capture_output: bool = True, check: bool = True, 
                  user: Optional[str] = None, group: Optional[str] = None, 
                  umask: Optional[str] = None, cwd: Optional[str] = None
                  ) -> CompletedRemoteCommand:
```

Runs the given command on the remote, returning a CompletedRemoteCommand
containing the returned information (if any) and the status code.

#### Parameters

 -  **command**: The command to be executed on the remote host.

 -  **input**: Input to the remote command.

 -  **capture_output**: Whether the output of the command should be captured.

 -  **check**: Whether to raise an exception if the remote command returns with a non-zero exit status.

 -  **user**: The remote user under which the command should be run. Also sets the group
    to the primary group of that user if it isn't explicitly given. If not given, the command
    is run as the user under which the remote dispatcher is running (usually root).

 -  **group**: The remote group under which the command should be run. If not given, the command
    is run as the group under which the remote dispatcher is running (usually root),
    or in case the user was explicitly specified, the primary group of that user.

 -  **umask**: The umask to use when executing the command on the remote system. Defaults to "077".

 -  **cwd**: The remote working directory under which the command should be run.

#### Returns

 -  **CompletedRemoteCommand**: The result of the remote command.

#### Raises

 -  **subprocess.CalledProcessError**: If check is True and the process returned a non-zero exit status.

 -  **ValueError**: A parameter was invalid.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.resolve_user()`

```python
def Connector.resolve_user(self, user: Optional[str]) -> str:
```

Resolves the given user on the remote, returning
the canonicalized username. If the given user is None, instead
returns the user as which the remote command is running.

#### Parameters

 -  **user**: The username or uid that should be resolved, or None to query the current user.

#### Returns

 -  **str**: The resolved username or None if the input was None.

#### Raises

 -  **ValueError**: If the user could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.resolve_group()`

```python
def Connector.resolve_group(self, group: Optional[str]) -> str:
```

Resolves the given group on the remote, returning
the canonicalized groupname. If the given group is None, instead
returns the group as which the remote command is running.

#### Parameters

 -  **group**: The groupname or gid that should be resolved, or None to query the current group.

#### Returns

 -  **str**: The resolved groupname or None if the input was None.

#### Raises

 -  **ValueError**: If the group could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.stat()`

```python
def Connector.stat(self, path: str, follow_links: bool = False, 
                   sha512sum: bool = False) -> Optional[StatResult]:
```

Runs stat() on the given path on the remote. Follows links if follow_links
is true. Includes the sha512sum if desired and if the path is a file.

Returns None if the remote path doesn't exist.

#### Parameters

 -  **path**: The path to stat.

 -  **follow_links**: Whether to follow symbolic links instead of running stat on the link.

 -  **sha512sum**: Whether to include the sha512sum if the path is a file.

#### Returns

 -  **Optional[StatResult]**: The stat result or None if the path didn't exist.

#### Raises

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails for any reason other than file not found.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.upload()`

```python
def Connector.upload(self, file: str, content: bytes, 
                     mode: Optional[str] = None, 
                     owner: Optional[str] = None, 
                     group: Optional[str] = None) -> None:
```

Uploads the given content to the remote system and saves it under the given file path. Overwrites existing files.

#### Parameters

 -  **file**: The file where the content will be saved.

 -  **content**: The file content.

 -  **owner**: The owner for the file. Defaults to root if not given.

 -  **group**: The group for the file. If the owner is given, defaults to the primary
    group of the owner, otherwise defaults to root.

 -  **mode**: The mode for the file. Defaults to '600' if not given.

#### Raises

 -  **ValueError**: A parameter was invalid.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.download()`

```python
def Connector.download(self, file: str) -> bytes:
```

Downloads the given file from the remote system.

#### Parameters

 -  **file**: The file to download.

#### Raises

 -  **ValueError**: If the file was not found.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails for any reason other than file not found.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.query_user()`

```python
def Connector.query_user(self, user: str, query_password_hash: bool = False
                         ) -> UserEntry:
```

Queries information about a user on the reomte system.

#### Parameters

 -  **user**: The username or uid that should be queried.

 -  **query_password_hash**: Whether the password hash should also be returned. Requires elevated privileges.

#### Returns

 -  **UserEntry**: The information about the user.

#### Raises

 -  **ValueError**: If the user could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.query_group()`

```python
def Connector.query_group(self, group: str) -> GroupEntry:
```

Queries information about a group on the reomte system.

#### Parameters

 -  **group**: The groupname or gid that should be queried.

#### Returns

 -  **GroupEntry**: The resolved groupname or None if the input was None.

#### Raises

 -  **ValueError**: If the group could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.getenv()`

```python
def Connector.getenv(self, key: str) -> Optional[str]:
```

Return's an environment variable from the remote host.

#### Parameters

 -  **key**: The variable to get.

#### Returns

 -  **str**: The corresponding variable if found, None otherwise.

#### Raises

 -  **ValueError**: If the user could not be resolved.

 -  **fora.connectors.tunnel_dispatcher.RemoteOSError**: If the remote command fails because of an remote OSError.

 -  **IOError**: An error occurred with the connection.

### <mark style="color:yellow;">def</mark> `Connector.extract_hostname()`

```python
def Connector.extract_hostname(cls, url: str) -> str:
```

Extracts the hostname from a given url where the schema matches this connector.

#### Parameters

 -  **url**: The url to extract the hostname from.

#### Returns

 -  **str**: The extracted hostname.

#### Raises

 -  **ValueError**: The provided url was invalid.

## <mark style="color:yellow;">def</mark> `connector.connector()`

```python
def connector.connector(schema: str
                        ) -> Callable[[Type[Connector]], Type[Connector]]:
```

The @connector class decorator used to register the connector
to the global registry.

### Parameters

 -  **schema**: The schema for the connector, for example 'ssh'.
