# Getting Started

## What is Fora?

Fora is an infrastructure and configuration management tool inspired by [Ansible](https://www.ansible.com) and [pyinfa](https://pyinfra.com). It can be used for machine provisioning and to configuration management. See [how it differs](usage/features.md) from existing tools.

## Installation & Quickstart

You can install Fora with pip:

```
pip install fora
```

With Fora, you create deploys, which are scripts that define in which state the remote host should be. These so-called operations are just functions that you call in a python script:

```python
from fora.operations import files, system

files.directory(
    name="Create a temporary directory",
    path="/tmp/hello")

system.package(
    name="Install neovim",
    package="neovim")
```

These can then be executed against an inventory, or a specific remote host via SSH:

```bash
fora root@example.com deploy.py
```

Fora can do a lot more than this, which you can learn in detail in the [Usage](usage/) section.
