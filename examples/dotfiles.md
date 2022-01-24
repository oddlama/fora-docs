# Managing dotfiles

Fora can easily be used to manage dotfiles.
It's common for people to have a dotfiles repository, and often they
make it available publicly. Usually those contain custom install scripts
that install the configuration files on new system.

{% tabs %}
{% tab title="install.py" %}
```python
from fora import host
from fora.operations import files

# Fetch home dir of executing user
home = host.connection.home_dir()

files.directory(f"{home}/.config/nvim")
files.upload(src="nvim/init.lua", dest=f"{home}/.config/nvim/")

# ...
```
{% endtab %}
{% endtabs %}

By executing this script against a host with url `local:`, it will automatically
be run as the user executing fora.

```bash
fora local: install.py
```

To guard against execution without fora, you can add this to
the first line of your script:

```python
if __name__ == '__main__':
	print("Please run this using `fora local: install.py`")
	sys.exit(1)

# ...
```
