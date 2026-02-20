# Ansible Collection - linbit.drbd_reactor

Installs DRBD Reactor and manages HA gateway resource definitions for NFS and iSCSI.

## Roles

| Role | Description |
|---|---|
| `reactor_install` | Install and enable DRBD Reactor with automatic config reloading |
| `ha_gateway` | Create HA NFS/iSCSI DRBD Reactor promoter resources via LINSTOR |

## Dependencies

| Collection | Purpose |
|---|---|
| `linbit.drbd` | DRBD kernel module installation (required by `reactor_install`) |
| `linbit.linstor` | LINSTOR Gateway components (required by `ha_gateway`) |
| `community.general` | package management |
