resource_agents_fetch
=====================

Download missing OCF resource agents from the ClusterLabs GitHub repository.

This role is useful on distributions where the `resource-agents` package is missing or incomplete.
For example, AlmaLinux 10 has no `resource-agents` package, and Ubuntu 22.04 is missing the `nvmet-*` agents.

Downloaded agents are stamped with a version marker comment after the shebang line.
On subsequent runs, the role uses this marker to distinguish its own downloads from system-installed agents:

- **File missing**: download and stamp with version marker.
- **File exists with marker, same version**: skip (already current).
- **File exists with marker, different version**: re-download and update marker.
- **File exists without marker**: system-installed RA, leave untouched.

Requirements
------------

None.

Role Variables
--------------

| Variable | Default | Description |
|---|---|---|
| `resource_agents_version` | `v4.16.0` | GitHub release tag to download (for example `v4.17.0`). Empty string fetches the latest release. |
| `resource_agents_list` | See defaults | List of OCF resource agent names to ensure are present. |

### Default resource agents list

- `portblock`
- `Filesystem`
- `IPaddr2`
- `iSCSITarget`
- `iSCSILogicalUnit`
- `nfsserver`
- `exportfs`
- `nvmet-subsystem`
- `nvmet-namespace`
- `nvmet-port`

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Install OCF resource agents
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - name: Install missing OCF resource agents from GitHub
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_fetch
```

To install a specific version:

```yaml
    - name: Install OCF resource agents v4.17.0
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_fetch
      vars:
        resource_agents_version: v4.17.0
```

To install via `reactor_install` (enabled by default):

```yaml
    - name: Install DRBD Reactor with OCF resource agents
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
```

License
-------

MIT

Author Information
------------------

[LINBIT](https://linbit.com)
