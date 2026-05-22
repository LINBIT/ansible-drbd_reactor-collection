# LINBIT DRBD Reactor Collection

The `linbit.drbd_reactor` Ansible collection for installing and configuring [DRBD Reactor](https://github.com/LINBIT/drbd-reactor).

## Requirements

- ansible-core 2.16 or newer

## Installation

Install directly from GitHub using `requirements.yml` until LINBIT publishes to Ansible Galaxy:

```yaml
# requirements.yml
collections:
  - name: linbit.common
    source: https://github.com/LINBIT/ansible-common-collection.git
    type: git
  - name: linbit.drbd
    source: https://github.com/LINBIT/ansible-drbd-collection.git
    type: git
  - name: linbit.drbd_reactor
    source: https://github.com/LINBIT/ansible-drbd_reactor-collection.git
    type: git
```

```bash
ansible-galaxy collection install -r requirements.yml
```

To upgrade to the latest commits on the default branch:

```bash
ansible-galaxy collection install -r requirements.yml --upgrade
```

See [using Ansible collections](https://docs.ansible.com/ansible/latest/collections_guide/) for more details.

## Roles

| Role | Description |
|---|---|
| [`reactor_install`](roles/reactor_install/README.md) | Install and enable DRBD Reactor with automatic config reloading. Dynamically includes `linbit.drbd.drbd_install` if DRBD is not already present on the node. |
| [`resource_agents_upstream`](roles/resource_agents_upstream/README.md) | Download missing OCF resource agents from the ClusterLabs GitHub repository. Only agents not already present on the system are downloaded. |

## Licensing

This collection is licensed under the MIT License.

See [LICENSE](LICENSE) for the full text.

## Authors

Created in 2026 by [Ryan Ronnander](https://github.com/ryan-ronnander) on behalf of [LINBIT](https://linbit.com).

Inspired by pre-collection Ansible contributions from [Matt Kereczman](https://github.com/kermat), [Ryan Ronnander](https://github.com/ryan-ronnander), [Michael Troutman](https://github.com/emteelb), and [Devin Vance](https://github.com/dvance).
