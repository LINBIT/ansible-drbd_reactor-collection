# reactor_install

Install and configure DRBD Reactor.

## Requirements

None.

## Role variables

| Variable | Default | Description |
|---|---|---|
| `reactor_install_package_state` | `present` | Package state for the DRBD Reactor package; set `latest` to check for upgrades |
| `reactor_install_drbd` | `true` | Include `linbit.drbd.drbd_install` to install DRBD; set `false` when DRBD is already managed by a calling role (for example `satellite_install`) |
| `reactor_install_resource_agents_upstream` | `true` | Include `resource_agents_upstream` role to install missing OCF resource agents from GitHub |
| `reactor_install_scst` | `false` | Include `scst_install` role to compile and install the SCST iSCSI target from source |

## Dependencies

`linbit.drbd.drbd_install`: included by default, disable with `reactor_install_drbd: false`.

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
