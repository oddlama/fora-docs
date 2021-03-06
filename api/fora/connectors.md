# fora.connectors

Contains all standard conectors to register them by default.

## Submodules

 -  [`connectors.local`](connectors/local.md) ‒ Contains a connector which handles connections to hosts via SSH.

 -  [`connectors.tunnel_connector`](connectors/tunnel\_connector.md) ‒ Contains a connector base which handles communication via any spawned subprocess command that can run a tunnel dispatcher on the remote host.

 -  [`connectors.ssh`](connectors/ssh.md) ‒ Contains a connector which handles connections to hosts via SSH.

 -  [`connectors.connector`](connectors/connector.md) ‒ Defines the connector interface.

 -  [`connectors.tunnel_dispatcher`](connectors/tunnel\_dispatcher.md) ‒ Provides a stdin/stdout based protocol to safely dispatch commands and return their
    results over any connection that forwards both stdin/stdout, as well as some other
    needed remote system related utilities.
