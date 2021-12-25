# Overview

## How is Fora different from existing tools?

Fora is lightweight and simple. It doesn't get in your way and allows
you to use python anywhere - like you normally would.
Be it to build your inventory, to set host or group variables, or in between operations.

Both Ansible and pyinfra are amazing tools. Check them out if you haven't already.
If you require large-scale deploys (>100 hosts), they are most likely a better choice than Fora.
That being said, here is how Fora is different:

## Features

🪶 **Lightweight.** With just `~2.6k` lines of code, Fora is tiny.
As a reference: pyinfra is around `~11.1k`, and ansible beyond `>62.0k` (measured with [cloc](https://github.com/AlDanial/cloc)).
Fora focuses on being a tool to execute common operations on remote hosts. Not more, not less.
It has no domain-specific-language and no complex logic.
If you need more, you can very easily extend it yourself or use one of the aforementioned tools.

🌱 **Simple.** Writing deploys in Fora is designed to be simple.
Inventories, hosts, groups and scripts are just regular python scripts defining global variables.
This allows you to anything you like in these files without adding complexity to fora.

🎉⃠ **No surprises.** Fora focuses just on solving the core problem of infrastructure management,
and will not get in your way. Smart tools are great, but sometimes dumb tools are better.
Fora does exactly what you expect it to do and not more.

🐞 **Debuggable.** Fora shows state changes to remote hosts in a diff-like manner,
so you can easily spot what has changed. With `--diff` you can even get a diff of
files that were changed. Also in case an error occurs, stdout and stderr of the failed
remote command will be shown to ease debugging.

## Features

🕵️ agentless and fast for small inventories.
All commands are run through a single ssh session.
But, fora doesn't parallelize actions on multiple host. This means while
individual command chains are extremely fast, it can take a while to provision
more than a few hosts.

**Extendable.** Variables are inherited automatically. No setup necessary.

- cannot become root.
