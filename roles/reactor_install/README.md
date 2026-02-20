reactor_install
===============

Install and configure DRBD Reactor.

Requirements
------------

None.

Role Variables
--------------

See `defaults/main.yml`.

Dependencies
------------

`linbit.drbd.drbd_install`

Example Playbook
----------------

```yaml
- name: Install DRBD Reactor
  hosts: all
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
