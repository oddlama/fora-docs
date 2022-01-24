# Groups

A group defines variables that and can be inherited by other groups or hosts.
Hosts always inherit all variables from their groups, while groups only inherit
variables from groups they (transitively) depend on, to make sure no ambiguous or
conflicting variables are defined.

In principle, group modules function entirely analogous to host modules,
except that they are only used to define shared global variables.
By default, an inventory tries to load group modules from `groups/<name>.py`, relative to the inventory.
