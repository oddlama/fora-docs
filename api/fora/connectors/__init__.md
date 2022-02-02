# fora.connectors

Contains all standard conectors to register them by default.

## Submodules

 -  [`connectors.local`](api/fora/connectors/local.md) ‒ Contains a connector which handles connections to hosts via SSH.

 -  [`connectors.tunnel_connector`](api/fora/connectors/tunnel\_connector.md) ‒ Contains a connector base which handles communication via any spawned subprocess command that can run a tunnel dispatcher on the remote host.

 -  [`connectors.ssh`](api/fora/connectors/ssh.md) ‒ Contains a connector which handles connections to hosts via SSH.

 -  [`connectors.connector`](api/fora/connectors/connector.md) ‒ Defines the connector interface.

 -  [`connectors.tunnel_dispatcher`](api/fora/connectors/tunnel\_dispatcher.md) ‒ Provides a stdin/stdout based protocol to safely dispatch commands and return their
    results over any connection that forwards both stdin/stdout, as well as some other
    needed remote system related utilities.
