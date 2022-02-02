# Operations

## operations.apt

 -  [`apt.package()`](fora/operations/apt.md#def-apt.package) ‒ Adds or removes system packages with apt-get.

## operations.files

 -  [`files.directory()`](fora/operations/files.md#def-files.directory) ‒ Manages the state of an directory on the remote host.

 -  [`files.file()`](fora/operations/files.md#def-files.file) ‒ Creates, deletes or updates the given file.

 -  [`files.link()`](fora/operations/files.md#def-files.link) ‒ Creates, deletes or updates the given symbolic link.

 -  [`files.upload_content()`](fora/operations/files.md#def-files.upload\_content) ‒ Uploads the given content as a file to the remote host.

 -  [`files.upload()`](fora/operations/files.md#def-files.upload) ‒ Uploads the given file or to the remote host.

 -  [`files.upload_dir()`](fora/operations/files.md#def-files.upload\_dir) ‒ Uploads the given directory to the remote host.

 -  [`files.template_content()`](fora/operations/files.md#def-files.template\_content) ‒ Templates the given content and uploads the result to the remote host.

 -  [`files.template()`](fora/operations/files.md#def-files.template) ‒ Templates the given file and uploads the result to the remote host.

 -  [`files.line()`](fora/operations/files.md#def-files.line) ‒ Manage a line in a file.

## operations.git

 -  [`git.repo()`](fora/operations/git.md#def-git.repo) ‒ Clones or updates a git repository and its submodules.

## operations.local

 -  [`local.script()`](fora/operations/local.md#def-local.script) ‒ Executes the given script for the current host.

## operations.pacman

 -  [`pacman.package()`](fora/operations/pacman.md#def-pacman.package) ‒ Adds or removes system packages with pacman.

## operations.pip

## operations.portage

 -  [`portage.package()`](fora/operations/portage.md#def-portage.package) ‒ Adds or removes system packages with portage.

## operations.postgres

## operations.system

 -  [`system.user()`](fora/operations/system.md#def-system.user) ‒ Creates, modifies or deletes a unix user.

 -  [`system.group()`](fora/operations/system.md#def-system.group) ‒ Creates, modifies or deletes a unix group.

 -  [`system.package()`](fora/operations/system.md#def-system.package) ‒ Adds or removes system packages by detecting a supported init system to execute the operation.

 -  [`system.service()`](fora/operations/system.md#def-system.service) ‒ Manages a system service by detecting a supported init system to execute the operation.

## operations.systemd

 -  [`systemd.daemon_reload()`](fora/operations/systemd.md#def-systemd.daemon\_reload) ‒ Manages a systemd unit.

 -  [`systemd.service()`](fora/operations/systemd.md#def-systemd.service) ‒ Manages a systemd unit.
