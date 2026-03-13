reactor_install
===============

Install and configure DRBD Reactor.

Requirements
------------

None.

Role Variables
--------------

| Variable | Default | Description |
|---|---|---|
| `reactor_install_drbd` | `true` | Include `linbit.drbd.drbd_install` to install DRBD. Set to `false` when DRBD is already managed by a calling role (for example `satellite_install`). |
| `reactor_install_package_state` | `latest` | Package state for DRBD Reactor package; set `present` to skip upgrades |

Dependencies
------------

`linbit.drbd.drbd_install`: included by default, disable with `reactor_install_drbd: false`.

Example Playbook
----------------

```yaml
- name: Install DRBD Reactor
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
```

License
-------

MIT

Author Information
------------------

[LINBIT](https://linbit.com)
