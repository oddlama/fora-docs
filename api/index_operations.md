# Operations

## [`operations.apt`](api/fora/operations/apt.md)

 -  [`apt.package()`](api/fora/operations/apt.md#def-apt.package) ‒ Adds or removes system packages with apt-get.

## [`operations.files`](api/fora/operations/files.md)

 -  [`files.directory()`](api/fora/operations/files.md#def-files.directory) ‒ Manages the state of an directory on the remote host.

 -  [`files.file()`](api/fora/operations/files.md#def-files.file) ‒ Creates, deletes or updates the given file.

 -  [`files.link()`](api/fora/operations/files.md#def-files.link) ‒ Creates, deletes or updates the given symbolic link.

 -  [`files.upload_content()`](api/fora/operations/files.md#def-files.upload\_content) ‒ Uploads the given content as a file to the remote host.

 -  [`files.upload()`](api/fora/operations/files.md#def-files.upload) ‒ Uploads the given file or to the remote host.

 -  [`files.upload_dir()`](api/fora/operations/files.md#def-files.upload\_dir) ‒ Uploads the given directory to the remote host.

 -  [`files.template_content()`](api/fora/operations/files.md#def-files.template\_content) ‒ Templates the given content and uploads the result to the remote host.

 -  [`files.template()`](api/fora/operations/files.md#def-files.template) ‒ Templates the given file and uploads the result to the remote host.

 -  [`files.line()`](api/fora/operations/files.md#def-files.line) ‒ Manage a line in a file.

## [`operations.git`](api/fora/operations/git.md)

 -  [`git.repo()`](api/fora/operations/git.md#def-git.repo) ‒ Clones or updates a git repository and its submodules.

## [`operations.local`](api/fora/operations/local.md)

 -  [`local.script()`](api/fora/operations/local.md#def-local.script) ‒ Executes the given script for the current host.

## [`operations.pacman`](api/fora/operations/pacman.md)

 -  [`pacman.package()`](api/fora/operations/pacman.md#def-pacman.package) ‒ Adds or removes system packages with pacman.

## [`operations.pip`](api/fora/operations/pip.md)

## [`operations.portage`](api/fora/operations/portage.md)

 -  [`portage.package()`](api/fora/operations/portage.md#def-portage.package) ‒ Adds or removes system packages with portage.

## [`operations.postgres`](api/fora/operations/postgres.md)

## [`operations.system`](api/fora/operations/system.md)

 -  [`system.user()`](api/fora/operations/system.md#def-system.user) ‒ Creates, modifies or deletes a unix user.

 -  [`system.group()`](api/fora/operations/system.md#def-system.group) ‒ Creates, modifies or deletes a unix group.

 -  [`system.package()`](api/fora/operations/system.md#def-system.package) ‒ Adds or removes system packages by detecting a supported init system to execute the operation.

 -  [`system.service()`](api/fora/operations/system.md#def-system.service) ‒ Manages a system service by detecting a supported init system to execute the operation.

## [`operations.systemd`](api/fora/operations/systemd.md)

 -  [`systemd.daemon_reload()`](api/fora/operations/systemd.md#def-systemd.daemon\_reload) ‒ Manages a systemd unit.

 -  [`systemd.service()`](api/fora/operations/systemd.md#def-systemd.service) ‒ Manages a systemd unit.
