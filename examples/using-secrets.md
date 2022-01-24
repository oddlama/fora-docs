# Using secrets

When your deploy repository is on GitHub or any other third party storage,
you should properly encrypt your secrets. Fora doesn't provide any proprietary
methods to do this, as there are plenty of libraries that solve this problem already.

As host and group modules are regular python modules, loading secrets with an external library is quite easy.
While there are different ways to achieve the same things, I recommend decrypting the secret
storage in your inventory. This is beneficial because groups might be executed multiple times
(once for each host that uses it), and this way you only have to decrypt once.

The general structure you can use for any type of encryption is to
load a global dictionary containing the secret values when
the inventory is loaded. This variable can then be acccessed
anywhere in your deploy.

{% tabs %}
{% tab title="inventory.py" %}
```python
import toml

_decrypted_toml = # decrypt a file using your method of choice
secrets = toml.loads(_decrypted_toml)

# ...
```
{% endtab %}
{% tab title="groups/desktops.py" %}
```python
root_password_hash = secrets["desktops"]["root_password_hash"]
```
{% endtab %}
{% endtabs %}

### Example: Using `age` or `gpg`

A great option is to store secrets in an [`age`](https://age-encryption.org) encrypted `toml` file.
While age doesn't support Yubikeys out-of-the-box, using `gpg` may also be a good option.
Using `cryptography`'s fernet protocol might also be a viable option.

{% tabs %}
{% tab title="age" %}
{% code title="inventory.py" %}
Encrypt a file using `cat secrets.toml | age --encrypt --armor --passphrase > secrets.toml.age` and delete the original.
```python
import toml, subprocess

with open("secrets.toml.age", "rb") as f:
    _decrypted_toml = subprocess.run(["age", "--decrypt"], input=f.read(), stdout=subprocess.PIPE, check=True).stdout.decode()
secrets = toml.loads(_decrypted_toml)
```
{% endcode %}
{% endtab %}
{% tab title="gpg" %}
Encrypt a file using `gpg --quiet --encrypt --recipient your-identity@example.com --output secrets.toml.gpg secrets.toml` and delete the original.
{% code title="inventory.py" %}
```python
import toml, subprocess

with open("secrets.toml.gpg", "rb") as f:
    _decrypted_toml = subprocess.run(["gpg", "--quiet", "--decrypt"], input=f.read(), stdout=subprocess.PIPE, check=True).stdout.decode()
secrets = toml.loads(_decrypted_toml)

# ...
```
{% endcode %}
{% endtab %}
{% tab title="secrets.toml" %}
```python
[desktops]
root_password_hash = "$6$3gtzs4..."
```
{% endtab %}
{% endtabs %}
