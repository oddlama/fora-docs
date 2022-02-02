# fora.connectors

Contains all standard conectors to register them by default.

## Submodules

 -  **connectors.local**: Contains a connector which handles connections to hosts via SSH.

 -  **connectors.tunnel_connector**: Contains a connector base which handles communication via any spawned subprocess command that can run a tunnel dispatcher on the remote host.

 -  **connectors.ssh**: Contains a connector which handles connections to hosts via SSH.

 -  **connectors.connector**: Defines the connector interface.

 -  **connectors.tunnel_dispatcher**: Provides a stdin/stdout based protocol to safely dispatch commands and return their
    results over any connection that forwards both stdin/stdout, as well as some other
    needed remote system related utilities.