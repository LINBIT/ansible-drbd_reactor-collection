# Ansible Collection - linbit.drbd_reactor

Installs and configures DRBD Reactor.
DRBD Reactor is the daemon that monitors DRBD resources and drives HA services via promoter plugins.

## Roles

| Role | Description |
|---|---|
| `reactor_install` | Install and enable DRBD Reactor with automatic config reloading. Dynamically includes `linbit.drbd.drbd_install` if DRBD is not already present on the node. |
| `resource_agents_upstream` | Download missing OCF resource agents from the ClusterLabs GitHub repository. Only agents not already present on the system are downloaded. |

## Dependencies

| Collection | Purpose |
|---|---|
| `linbit.drbd` | DRBD kernel module installation (required by `reactor_install`) |

## Licensing

This collection is licensed under the MIT License.

See [LICENSE](LICENSE) for the full text.
