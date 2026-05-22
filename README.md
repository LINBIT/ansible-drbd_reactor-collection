# LINBIT DRBD Reactor Collection

The `linbit.drbd_reactor` Ansible collection for installing and configuring [DRBD Reactor](https://github.com/LINBIT/drbd-reactor).

## Requirements

- ansible-core 2.16 or newer

## Roles

| Role | Description |
|---|---|
| `reactor_install` | Install and enable DRBD Reactor with automatic config reloading. Dynamically includes `linbit.drbd.drbd_install` if DRBD is not already present on the node. |
| `resource_agents_upstream` | Download missing OCF resource agents from the ClusterLabs GitHub repository. Only agents not already present on the system are downloaded. |

## Licensing

This collection is licensed under the MIT License.

See [LICENSE](LICENSE) for the full text.

## Authors

Created in 2026 by [Ryan Ronnander](https://github.com/ryan-ronnander) on behalf of [LINBIT](https://linbit.com).

Inspired by pre-collection Ansible contributions from [Matt Kereczman](https://github.com/kermat), [Ryan Ronnander](https://github.com/ryan-ronnander), [Michael Troutman](https://github.com/emteelb), and [Devin Vance](https://github.com/dvance).
