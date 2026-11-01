# reactor_install

Install and configure DRBD Reactor.

## Requirements

None.

## Role variables

| Variable | Default | Description |
|---|---|---|
| `reactor_install_package_state` | `present` | Package state for the DRBD Reactor package; set `latest` to check for upgrades |
| `reactor_install_drbd` | `true` | Include `linbit.drbd.drbd_install` to install DRBD; set `false` when DRBD is already managed by a calling role (for example `satellite_install`) |
| `reactor_install_resource_agents` | `upstream` | Where the OCF resource agents come from: `upstream` includes `resource_agents_upstream`, which adds only the agents missing on the node from GitHub and leaves agents from a distribution package in place, `distro` includes `resource_agents_distro` to install the distribution `resource-agents` package, `none` installs no agents |
| `reactor_install_scst` | `false` | Include `scst_install` role to compile and install the SCST iSCSI target from source |
| `reactor_install_ganesha` | `false` | Include `ganesha_install` role to install the NFS-Ganesha userspace NFS server |
| `reactor_install_prometheus` | `false` | Open the firewall for the Prometheus exporter; the exporter only listens once a `[[prometheus]]` section exists in the DRBD Reactor configuration, so the port stays closed until this is set |
| `reactor_install_firewall_rules` | `true` | Manage firewall rules for the Prometheus exporter port; no effect unless `reactor_install_prometheus` is set |
| `reactor_install_firewalld_zone` | `""` | firewalld zone to open the Prometheus exporter port in; empty uses the default zone, set it when a zone is bound to a source; ignored by UFW |
| `reactor_install_firewall_ports` | `9942/tcp` | Ports to open in firewalld or UFW for the Prometheus exporter |

## Dependencies

`linbit.drbd.drbd_install`: included by default, disable with `reactor_install_drbd: false`.

`linbit.drbd_reactor.resource_agents_upstream`: included by default, with `reactor_install_resource_agents: upstream`.

`linbit.drbd_reactor.resource_agents_distro`: included with `reactor_install_resource_agents: distro`.

## Example playbook

```yaml
- name: Install DRBD Reactor
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
```

## License

MIT

## Author information

[LINBIT](https://linbit.com)
