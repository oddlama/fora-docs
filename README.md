# Getting Started

## What is Fora?

Fora is an infrastructure and configuration management tool inspired by [Ansible](https://www.ansible.com) and [pyinfa](https://pyinfra.com). It can be used for machine provisioning and configuration management. See [how it differs](outlining-the-differences.md#how-is-fora-different-from-existing-tools) from existing tools.

## Installation & Quickstart

You can install Fora with pip:

```bash
pip install fora
```

Afterwards, you can use it to write scripts that can run operation or commands on a remote host.

{% tabs %}
{% tab title="deploy.py" %}
```python
from fora.operations import files, system

files.directory(
    name="Create a temporary directory",
    path="/tmp/hello")

system.package(
    name="Install neovim",
    package="neovim")
```
{% endtab %}
{% endtabs %}

These scripts are executed against an inventory, or a specific remote host (usually via SSH).

```bash
fora root@example.com deploy.py
```

To start with your own (more complex) deploy, you can have Fora create a scaffolding in an empty directory. There are [different scaffoldings](usage/introduction/#deploy-structure) available for different use-cases.

```bash
fora --init minimal
```

Fora can do a lot more than this, which is explained in the [introduction](usage/introduction/ "mention") section. If you are interested in how Fora is different from existing tools, have a look at [outlining-the-differences.md](outlining-the-differences.md "mention").
