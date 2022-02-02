# fora.operations.files

Provides operations related to creating and modifying files and directories.

## <mark style="color:yellow;">`def`</mark> `files.directory()`

```python
def files.directory(path: str, present: bool = True, touch: bool = False, 
                    mode: Optional[str] = None, owner: Optional[str] = None, 
                    group: Optional[str] = None, name: Optional[str] = None, 
                    check: bool = True, 
                    op: Operation = Operation.internal_use_only
                    ) -> OperationResult:
```

Manages the state of an directory on the remote host.
If the path already exists but isn't a directory, the operation will fail.

### Parameters

 -  **path**: The directory path.

 -  **present**: Whether the directory should exist. If False, an existing directory (and its contents) will be deleted.
    If the path exists, but isn't a directory (but a file, link, ...) the operation will fail.

 -  **touch**: Whether the directory should be touched (access and modification times will be updated).

 -  **mode**: The directory mode. Uses the remote execution defaults if None.

 -  **owner**: The directory owner. Uses the remote execution defaults if None.

 -  **group**: The directory group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.file()`

```python
def files.file(path: str, present: bool = True, touch: bool = False, 
               mode: Optional[str] = None, owner: Optional[str] = None, 
               group: Optional[str] = None, name: Optional[str] = None, 
               check: bool = True, 
               op: Operation = Operation.internal_use_only
               ) -> OperationResult:
```

Creates, deletes or updates the given file.

### Parameters

 -  **path**: The remote file path.

 -  **present**: Whether the file should exist. If False, an existing file will be deleted.
    If the path exists, but isn't a file (but a directory, link, ...) the operation will fail.

 -  **touch**: Whether the file should be touched (access and modification times will be updated).

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.link()`

```python
def files.link(path: str, target: str, present: bool = True, 
               touch: bool = False, owner: Optional[str] = None, 
               group: Optional[str] = None, name: Optional[str] = None, 
               check: bool = True, 
               op: Operation = Operation.internal_use_only
               ) -> OperationResult:
```

Creates, deletes or updates the given symbolic link.

### Parameters

 -  **path**: The path of the link.

 -  **target**: The target path which the link points to.

 -  **present**: Whether the link should exist. If False, an existing link will be deleted.
    If the path exists, but isn't a link (but a directory, file, ...) the operation will fail.

 -  **touch**: Whether the link should be touched (access and modification times will be updated). This affects the link itself, not the content!

 -  **owner**: The link owner. Uses the remote execution defaults if None.

 -  **group**: The link group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.upload_content()`

```python
def files.upload_content(content: Union[str, bytes], dest: str, 
                         mode: Optional[str] = None, 
                         owner: Optional[str] = None, 
                         group: Optional[str] = None, 
                         name: Optional[str] = None, check: bool = True, 
                         op: Operation = Operation.internal_use_only
                         ) -> OperationResult:
```

Uploads the given content as a file to the remote host.

### Parameters

 -  **content**: The content to upload.

 -  **dest**: The remote destination path.

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.upload()`

```python
def files.upload(src: str, dest: str, mode: Optional[str] = None, 
                 owner: Optional[str] = None, group: Optional[str] = None, 
                 name: Optional[str] = None, check: bool = True, 
                 op: Operation = Operation.internal_use_only
                 ) -> OperationResult:
```

Uploads the given file or to the remote host. Overwrites existing files.

### Parameters

 -  **src**: The local file to upload.

 -  **dest**: The remote destination path. If this ends in a slash, the basename of the source file is automatically appended.

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.upload_dir()`

```python
def files.upload_dir(src: str, dest: str, dir_mode: Optional[str] = None, 
                     file_mode: Optional[str] = None, 
                     owner: Optional[str] = None, 
                     group: Optional[str] = None, 
                     name: Optional[str] = None, check: bool = True, 
                     op: Operation = Operation.internal_use_only
                     ) -> OperationResult:
```

Uploads the given directory to the remote host. Unrelated files
in an existing destination directories will be left untouched.
This will only upload files and directories, not links or other special files.

Given the following source directory:

     example/
    └ something.conf

A trailing slash will cause the folder to become a child of the destination directory.

    upload_dir("example", "/var/")

     /var/
    └  example/
      └ something.conf

No trailing slash will cause the folder to become the specified folder.

    upload_dir("example", "/var/myexample")

     /var/
    └  myexample/
      └ something.conf

### Parameters

 -  **src**: The local directory to upload.

 -  **dest**: The remote destination path for the source directory. If this path
    ends with a slash, the source directory will be uploaded as a child
    of the denoted directory. Otherwise, the uploaded directory will be
    renamed accordingly.

 -  **dir_mode**: The mode for uploaded directories. Includes the base folder. Uses the remote execution defaults if None.

 -  **file_mode**: The mode for uploaded files. Uses the remote execution defaults if None.

 -  **owner**: The owner for all files and directories. Uses the remote execution defaults if None.

 -  **group**: The group for all files and directories. Uses the remote execution defaults if None.

## <mark style="color:yellow;">`def`</mark> `files.template_content()`

```python
def files.template_content(content: str, dest: str, 
                           context: Optional[dict] = None, 
                           mode: Optional[str] = None, 
                           owner: Optional[str] = None, 
                           group: Optional[str] = None, 
                           name: Optional[str] = None, check: bool = True, 
                           op: Operation = Operation.internal_use_only
                           ) -> OperationResult:
```

Templates the given content and uploads the result to the remote host.
See [`files.template()`](api/fora/operations/files.md#template) for more information about the available variables in the template.

### Parameters

 -  **content**: The content to template.

 -  **dest**: The remote destination path for the file.

 -  **context**: Additional dictionary of variables that will be made available in the template.

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.template()`

```python
def files.template(src: str, dest: str, context: Optional[dict] = None, 
                   mode: Optional[str] = None, owner: Optional[str] = None, 
                   group: Optional[str] = None, name: Optional[str] = None, 
                   check: bool = True, 
                   op: Operation = Operation.internal_use_only
                   ) -> OperationResult:
```

Templates the given file and uploads the result to the remote host.
All host variables, including inherited variables will be available
as-is in the template. Any variable provided via the context will
also be made available, overshadowing existing variables.

The current host and inventory will always be added under the keys
`"host"` and `"inventory"` respectively, shadowing other variables,
to ensure these objects are always accessible.

### Parameters

 -  **src**: The local file to template.

 -  **dest**: The remote destination path. If this ends in a slash, the basename of the source file is automatically appended.

 -  **context**: Additional dictionary of variables that will be made available in the template.

 -  **mode**: The file mode. Uses the remote execution defaults if None.

 -  **owner**: The file owner. Uses the remote execution defaults if None.

 -  **group**: The file group. Uses the remote execution defaults if None.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.

## <mark style="color:yellow;">`def`</mark> `files.line()`

```python
def files.line(path: str, line: str, present: bool = True, 
               regex: Optional[str] = None, ignore_whitespace: bool = True, 
               backup: Union[bool, str] = False, name: Optional[str] = None, 
               check: bool = True, 
               op: Operation = Operation.internal_use_only
               ) -> OperationResult:
```

Manage a line in a file. New lines will be added to the end of the file.
If the file does not exist, it will be created with the current default file_mode, owner and group.

### Parameters

 -  **path**: The file in question.

 -  **line**: The line that should be added or removed. If present is `False` and regex is set,
    this parameter is not used.

 -  **present**: Whether the line should exist in the file.

 -  **regex**: A regex to search for in the existing file to determine whether the line exists.
    Partial matches in a line (when for example not using `^...$`) count as matching the entire line.
    Uses standard python regular expression. If `None`, the given line will be searched for literally.

 -  **ignore_whitespace**: Whether whitespace before and after the line should be ignored when searching for the line.
    This affects both the provided line and lines in an existing file.
    This does not affect the actual line that will be put into the file if it isn't found.
    This parameter is ignored if `regex` was specified.

 -  **backup**: Whether to backup the old file if any changes will be made. If this is `True`,
    the old file will be copied to a new file with `.{date}.bak`, appended to the end of the filename.
    `date` will be replaced with the current date and time in ISO 8601 UTC format.
    You pass a string instead of a boolean to enable this option while specifying the new filename manually,
    relative to the original file.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.