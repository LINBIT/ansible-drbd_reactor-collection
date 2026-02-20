ha_gateway
==========

Configure DRBD Reactor HA gateway resources for NFS and iSCSI.

> **Work in progress.**
> This role is an Ansible-driven alternative to using [LINSTOR Gateway](https://github.com/LINBIT/linstor-gateway) directly.
> LINSTOR Gateway is the recommended tool for managing HA NFS and iSCSI resources — this role automates equivalent configuration via DRBD Reactor promoter resources but is not feature-complete.
> **Use at your own risk.**

Requirements
------------

None.

Role Variables
--------------

See `defaults/main.yml`.

Dependencies
------------

`linbit.drbd_reactor.reactor_install`, `linbit.linstor.linstor_gateway_install_common`

Install LINSTOR Gateway first with the `linstor_gateway_install_common` role before using this role to create HA resources.

```yaml
- name: Install Gateway
  hosts: linstor_satellites
  any_errors_fatal: true
  tags: install_gateway
  become: true
  tasks:
    - name: Install LINSTOR Gateway components
      ansible.builtin.import_role:
        name: linbit.linstor.linstor_gateway_install_common
```

Example Playbook
----------------

```yaml
- name: Deploy HA resources with DRBD Reactor
  hosts: linstor_satellites
  any_errors_fatal: true
  tags: deploy_ha_resources
  become: true
  tasks:
    # Install all HA resource dependencies for DRBD Reactor
    - name: Install LINSTOR Gateway components
      ansible.builtin.import_role:
        name: linbit.linstor.linstor_gateway_install_common

    # Currently working for 3 node LINSTOR clusters
    - name: Create HA resources for DRBD Reactor
      vars:
        # ha_gateway_rg: "{{ linstor_rg }}" # Can fall back to DfltRscGrp
        ha_gateway_nfs_exports:
          - name: example-export
            size: 1G
            vip: 192.168.222.250
            # cidr_netmask: 24 # Can fall back to default (24)
            # fstype: 'ext4' # Can fall back to default (ext4)
        ha_gateway_iscsi_targets:
          - name: example-target
            size: 2G
            vip: 192.168.222.251
            # cidr_netmask: 24 # Can fall back to default (24)
            # target_port: 3260 # Can fall back to default (3260)
            # iqn_base: 'iqn.2026-02.com.linbit' # Can fall back to default
            # fstype: 'ext4' # portblock tickledir - fall back to default (ext4)
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.ha_gateway
```

License
-------

MIT

Author Information
------------------

[LINBIT](https://linbit.com)
