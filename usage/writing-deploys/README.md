# Writing deploys

A deploy is a directory containing your inventories, hosts, groups and scripts.
Here is a short overview over these components:

[**Inventory.**](TODO) A collection of hosts, against which your scripts are run. Single hostnames like `root@example.com` are valid inventories.

[**Host.**](TODO) A remote host. Optionally associated with a module file, where you can assign variables and groups to this host.

[**Group.**](TODO) A collection of hosts. All hosts in a group inherit variables defined by the group. All hosts implicitly belong to the `all` group for global variables.

[**Script.**](TODO) A python script using predefined [operations](TODO) to define what should be run on the remote hosts.

Fora always needs an inventory and a main script to run on each invocation:

```
fora <inventory>... <script>`
```

## Deploy structure

While the specific deploy structure is up to you, we recommend the following layout:

```
inventories/
	hosts/
		- example.py # host specific variables
	groups/
		- all.py         # global variables
		- webservers.py  # variables for webservers
		- production.py  # variables for production only
    - production.py  # production inventory hosts
    - staging.py     # staging inventory hosts
tasks/  # reusable scripts performing specific tasks
    - ssh.py
	nginx/ # Could be an external repository
		files/
		templates/
		- nginx.py
files/     # Static files to upload via `files.upload()` not belonging to any task
templates/ # Templates that are rendered and uploaded via `files.template()`
- setup_server.py  # Initializer script containing one-off setup instructions
- deploy.py        # Main deploy script that can be re-run to update configuration
```

Host and group module definitions are always searched relative to the inventory.
Scripts are always executed in their respective folder, so files and templates are
relative to the script. Access to the "main" directory is always possible, and is defined
as the directory of the main script that is executed.


You can have a single `deploy.py` script that does everything, or you can organize different service deploys into different scripts. split deploy script different scripts to deploy different services, or one big script that does it all.

See the included [Examples](../TODO/) for more details on different layouts for different use-cases.

### Simple deploys

If your deploy is very limited in scope, the following layout might be a better fit:

```
hosts/
	- example.py # host specific variables
groups/ # if needed
- inventory.py  # all hosts
tasks/  # reusable scripts performing specific tasks
    - ssh.py
    - nginx.py
files/     # Static files to upload via `files.upload()` not belonging to any task
	- sshd.conf
templates/ # Templates that are rendered and uploaded via `files.template()`
	- nginx.conf.j2
- deploy.py        # Main deploy script that can be re-run to update configuration
```
