# Outlining the differences

## How is Fora different from existing tools?

You are probably here, asking yourself why I wrote this when there already are tools doing the same things. The answer is mainly that I liked writing it while I could address some issues I had with existing tools. Is Fora perfect? No. Certainly far from it. But it is my shot at creating something I personally enjoy using.

I took a lot of inspiration from amazing existing tools like [Ansible](https://www.ansible.com) and [pyinfa](https://pyinfra.com). I urge you to check them out if you haven't already. If you require large-scale deploys with many hosts, they are most likely a better choice and definitely more mature tools.

## Outlining the differences

To better illustrate the actual differences, I have compiled a list of short examples showing how Fora solves different problems I encountered with other tools. Basic knowledge about general infrastructure tools will be beneficial to fully understand this section.

I wanted to create a tool that...

### ...is more developer friendly.

test

**is more developer friendly.** I can already code python and I want access to the full capabilities of python at my fingertips when writing scripts or defining variables.

**minimizes hidden state and complexity.** Often introduced by the tool itself (as what feels like weird magic). I especially want clean, easy and self-contained deploy scripts.

**is more like a dumb API.** Except for initial setup like inventory loading, the tool should behave like a library without strange side-effects. Smart tools are always great, but sometimes dumb tools are better.

**focuses on being a tool for remote scripting and not a whole environment.** Keeping the tool lightweight increases its maintainability and reduces the amount of details one needs to learn about it. If you for example need to store a secret, you can already choose from a lot of good python libraries.

**properly deals with the dict merging vs.** dict overwriting issue. Scripts should have the option to do both while maintaining intuitive syntax.

**can detect variable definition conflicts.** Imagine a host is in two group, which both define a variable `var`. Which definition is chosen? Instead of choosing arbitrarily, this should be an error.

**reduces boilerplate operation arguments.** When configuring services, you often need to create files for a specific user with certain permissions. Twenty times. Therefore, I want to be able to temporarily change the defaults in a `with` block to reduce boilerplate and to keep my scripts readable.

**doesn't interface with the remote using shell commands.** I want the equivalent of a remote `subprocess.run`, both internally and in scripts. This way I don't have to worry about escaping arguments but can still spawn a shell when I need to.

**reuses a single ssh connection for all commands.** This is the only sane choice, and it is infinitely faster than the alternative.

**provides useful output.** It doesn't need to be super fancy. I just want to see the important information highlighted, instead of being presented with a terminal-filling block of text in a yellowish tint seemingly taken from `/dev/urandom`.

**generally reduces friction where possible.** When I just want to write a deploy for my dotfiles, a single (and short) `deploy.py` script should be all that is needed.

## TODO DELETE BELOW OR RESTRUCTURE FEATURES ALSO TODO show limitiations arising from simplicity (no become, no parallel execution)

## Features

🪶 **Lightweight.** Fora is a framework for infrastrucutre automation. Not more, not less. It aims to provide It has no domain-specific-language and no complex logic. If you need more, you can very easily extend it yourself or use one of the aforementioned tools. With just `~2.6k` lines of code, Fora is really tiny. As a reference: pyinfra is around `~11.1k`, and ansible beyond `>62.0k` (measured with [cloc](https://github.com/AlDanial/cloc)).

🌱 **Simple.** Writing deploys in Fora is designed to be simple. Inventories, hosts, groups and scripts are just regular python scripts defining global variables. This allows you to anything you like in these files without adding complexity to fora.

🎉⃠ **No surprises.** Fora focuses just on solving the core problem of infrastructure management, and will not get in your way. Smart tools are great, but sometimes dumb tools are better. Fora does exactly what you expect it to do and not more.

🐞 **Debuggable.** Fora shows state changes to remote hosts in a diff-like manner, so you can easily spot what has changed. With `--diff` you can even get a diff of files that were changed. Also in case an error occurs, stdout and stderr of the failed remote command will be shown to ease debugging.

## Features

🕵️ agentless and fast for small inventories. All commands are run through a single ssh session. But, fora doesn't parallelize actions on multiple host. This means while individual command chains are extremely fast, it can take a while to provision more than a few hosts.

**Extendable.** Variables are inherited automatically. No setup necessary.

* cannot become root.

TODO migrating (i.e. introduction if you know similar tooling)
