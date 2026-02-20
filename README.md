# Ansible Collection - linbit.drbd_reactor

Installs and configures DRBD Reactor.
DRBD Reactor is the daemon that monitors DRBD resources and drives HA services via promoter plugins.

## Roles

| Role | Description |
|---|---|
| `reactor_install` | Install and enable DRBD Reactor with automatic config reloading |

## Dependencies

| Collection | Purpose |
|---|---|
| `linbit.drbd` | DRBD kernel module installation (required by `reactor_install`) |
