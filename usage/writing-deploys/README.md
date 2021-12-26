# Writing deploys

fora is designed to work with arbitrary filenames. arguments are properly passed and never split like in a shell. robustutututu

A deploy is a directory containing your inventories, hosts, groups and scripts. Only a script is mandatory, the rest are optional created when needed.

**Inventory.** A inventory is a collection of hosts against which the scripts should be run. The simplest inventory is just a hostname like `root@example.com` passed to the `fora` command, but usually will be a file that defines an array of hosts. Let's call it `inventory.py`.

**Host.** A host definition is. list of vars, has a url which is by default the name. It is not necessary to have a matching module, if the host has no special. and.

**Group.** Similar to a host, a group is a python module file defining variables, which a host inherits (at lookup time!) if it is in the group. Groups can depend on one another to establish a definite variable precedence. Implicit all group f,or global variables.

**Script.** A script is basically just a regular python script that will be instanciated each time it is run on a specific host, with access to the initialized fora library. A script calls the operations that should be excuted on the currently processed host.

Global variables in a script are also inherited by a host (with least precedence), which allows the script to define global variables that have fallback values in case a host doesn't override it.

A script can define parameters, which allows you to separate and reuse functionality. Scripts can be called from other scripts with a special operation.

## Deploy structure

While the specific deploy structure is up to you, we recommend the following layout:

```
example_deploy/
  - inventory.py
```

You can have a single `deploy.py` script that does everything, or you can organize different service deploys into different scripts. split deploy script different scripts to deploy different services, or one big script that does it all.

See the included [Examples](../TODO/) for more details on different layouts for different use-cases.
