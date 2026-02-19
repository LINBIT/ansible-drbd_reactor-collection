Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Installing LINSTOR Gateway with the `linstor_gateway_install_common` role is an easy to to install all required software for creating HA resources with DRBD Reactor.

```yaml
- name: Install Gateway
  hosts: linstor_satellites
  any_errors_fatal: true
  tags: install_gateway
  become: true
  tasks:
    - name: Install LINSTOR Gateway components
      ansible.builtin.include_role:
        name: linbit.linstor.linstor_gateway_install_common
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- name: Deploy HA resources with DRBD Reactor
  hosts: linstor_satellites
  any_errors_fatal: true
  tags: deploy_ha_resources
  become: true
  tasks:
    # Install all HA resource dependencies for DRBD Reactor
    - name: Install LINSTOR Gateway components
      ansible.builtin.include_role:
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
      ansible.builtin.include_role:
        name: linbit.drbd_reactor.ha_gateway
```

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
