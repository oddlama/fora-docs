[//]: # (TODO logo image)
[//]: # (TODO FAQ)

# Getting Started

## What is Fora?

Fora is an infrastructure and configuration management tool inspired by [Ansible](https://www.ansible.com/) and [pyinfa](https://pyinfra.com/).

## Why? How is it different from existing tools?

Both Ansible and pyinfra are amazing tools. Check them out if you haven't already.
If you require large-scale deploys (>100 hosts), they are most likely a better choice than fora.
That being said, here is how fora is different.

🪶 **Lightweight.** With just `~2.6k` lines of code, Fora is tiny.
As a reference: pyinfra is around `~11.1k`, and ansible beyond `>62.0k` (measured with [cloc](https://github.com/AlDanial/cloc)).
Fora focuses on being a tool to execute common operations on remote hosts. Not more, not less.
It has no domain-specific-language and no complex logic.
If you need more, you can very easily extend it yourself or use one of the aforementioned tools.

🌱 **Simple.** Writing deploys in Fora is designed to be simple.
Inventories are strings like `root@example.com`, or a module with an array of hostnames.
Hosts and groups are simple python files defining global variables.
Scripts are just pure python scripts.

Install a package, upload a templated configuration and start a service all in three lines of code plus one line of imports.

🎉⃠ **No surprises.** Fora focuses on solving the core problem of infrastructure management only,
and will not get in your way. Smart tools are great, but sometimes dumb tools are better.
Fora does exactly what you would expect it to do.

🐞 **Debuggable.** Fora shows state changes to remote hosts in a diff-like manner,
so you can easily spot what has changed. With `--diff` you can even get a diff of
files that were changed. Also in case an error occurs, stdout and stderr of the failed
remote command will be shown to ease debugging.

## Features

- (shelock emoji and 20e8 unicode) agentless and fast for small inventories.
All commands are run through a single ssh session.
But, fora doesn't parallelize actions on multiple host. This means while
individual command chains are extremely fast, it can take a while to provision
more than a few hosts.

**Extendable.** Variables are inherited automatically. No setup necessary.

- cannot become root.

## Installation & Quickstart

You can install fora with pip:

```
pip install fora
```

With fora, you create deploys, which are scripts
that define what should be done on a remote host.
You write those operations to a file like `deploy.py`:

```python
from fora.operations import files, system

files.directory(
    name="Create a temporary directory",
    path="/tmp/hello")

system.package(
	name="Install neovim",
	package="neovim")
```

which can then be executed on a remote host via SSH:

```bash
fora root@example.com deploy.py
```
