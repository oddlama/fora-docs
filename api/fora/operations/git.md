# fora.operations.git

Provides operations related to git.

## <mark style="color:yellow;">`def`</mark> `git.repo()`

```python
def git.repo(url: str, path: str, branch_or_tag: Optional[str] = None, 
             update: bool = True, depth: Optional[int] = None, 
             rebase: bool = True, ff_only: bool = False, 
             update_submodules: bool = False, 
             recursive_submodules: bool = False, 
             shallow_submodules: bool = False, name: Optional[str] = None, 
             check: bool = True, op: Operation = Operation.internal_use_only
             ) -> OperationResult:
```

Clones or updates a git repository and its submodules.

### Parameters

 -  **url**: The url to the git repository.

 -  **path**: The path where the repository should be cloned.

 -  **branch_or_tag**: Either a branch name or a tag to clone. Follows the default branch of the remote if not given.

 -  **update**: Whether to keep the repository up to date if it has already been cloned.

 -  **depth**: Keep the repository as a shallow clone with the specified number of commits.
    Also applies when pulling updates.

 -  **rebase**: Use `--rebase` when pulling updates.

 -  **ff_only**: Use `--ff-only` when pulling updates.

 -  **update_submodules**: Also initialize and update submodules after cloning or pulling.

 -  **recursive_submodules**: Recursively update submodules after cloning or pulling.

 -  **shallow_submodules**: Also apply the given `depth` to submodule updates.

 -  **name**: The name for the operation.

 -  **check**: If True, returning `op.failure()` will raise an OperationError. All manually raised
    OperationErrors will be propagated. When False, any manually raised OperationError will
    be caught and `op.failure()` will be returned with the given message while continuing execution.

 -  **op**: The operation wrapper. Must not be supplied by the user.