# resource_agents_distro

Install resource agents from distribution repositories.

## How it works

- This role installs the distribution `resource-agents` package with weak dependencies disabled, so it does not pull in the wider Pacemaker stack.
- On Red Hat family rebuilds it enables the distribution High Availability repository (`ha` on EL8, `highavailability` on EL9 and later) or the Oracle Linux `addons` repository.
- On Red Hat Enterprise Linux and SLES it enables the subscription High Availability repository, falling back to a clear error when the system is not entitled.
- When a LINBIT pacemaker repository is already present (`pacemaker-2` on EL8/9, `pacemaker-3` on EL10), the role installs from it and leaves the distribution repositories untouched.
- openSUSE Leap, Debian, Ubuntu, and Proxmox VE install directly from their default repositories.

This is the distribution-package counterpart to [`resource_agents_upstream`](../resource_agents_upstream), which compiles agents from upstream source instead.

## Requirements

The host must already be registered when running on Red Hat Enterprise Linux or SLES.
On SLES, set `resource_agents_distro_sle_ha_regcode` when the system has no existing High Availability entitlement.

## Role variables

| Variable | Default | Description |
|---|---|---|
| `resource_agents_distro_packages` | `[resource-agents]` | Resource agent package names to install |
| `resource_agents_distro_sle_ha_regcode` | `""` | SLES High Availability extension registration code; empty relies on an existing entitlement |

## Dependencies

None.

## Example playbook

```yaml
- name: Install resource agents from distribution repositories
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - name: Install distribution resource agents
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_distro
```

## Distribution availability of resource-agents

| Distribution | Package | Repository |
|---|---|---|
| Debian, Proxmox VE, Ubuntu 22.04 and earlier | `resource-agents` | Default repositories |
| Ubuntu 24.04 and later | `resource-agents-base` | Default repositories; [`IPaddr2`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/IPaddr2) moved to a separate package |
| Red Hat 8/9 family with LINBIT customer repos | `resource-agents` | Included in the LINBIT `drbd-9` or `pacemaker-2` customer repository |
| Red Hat 10 family with LINBIT customer repos | `resource-agents` | Included in the LINBIT `pacemaker-3` customer repository |
| Red Hat (without LINBIT repos) | `resource-agents` | Requires the HA Add-On subscription (`highavailability-rpms`) |
| AlmaLinux, Rocky Linux | `resource-agents` | Requires enabling the `ha` (EL8) or `highavailability` (EL9+) repository |
| Oracle Linux | `resource-agents` | Requires enabling the `addons` repository |
| openSUSE Leap | `resource-agents` | Default repositories |
| SLES | `resource-agents` | Requires the High Availability extension subscription |

## License

MIT

## Author information

[LINBIT](https://linbit.com)
