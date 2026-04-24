resource_agents_upstream
=====================

Download missing OCF resource agents from the ClusterLabs GitHub repository.

This role is useful on distributions where the `resource-agents` package is missing, incomplete, or requires enabling additional repositories.
It bypasses distribution-specific packaging differences by downloading individual agents directly from source.

### Distribution availability of resource-agents

The `resource-agents` package availability varies across distributions:

| Distribution | Package | Repository |
|---|---|---|
| Debian, Proxmox VE, Ubuntu 22.04 and earlier | `resource-agents` | Default repositories. |
| Ubuntu 24.04 and later | `resource-agents-base` | Default repositories. `IPaddr2` moved to a separate package. |
| RHEL family with LINBIT customer repos | `resource-agents` | Included in the LINBIT `drbd-9` customer repository. |
| RHEL (without LINBIT repos) | `resource-agents` | Requires the HA Add-On subscription (`highavailability-rpms`). |
| AlmaLinux, Rocky Linux | `resource-agents` | Requires enabling the `ha` repository. |
| Oracle Linux | `resource-agents` | Requires enabling the `addons` repository. |
| SLES, openSUSE Leap | `resource-agents` | Default repositories. |
| RHEL 10 family with LINBIT customer repos | — | Not yet available in the LINBIT `drbd-9` repository. |

This role avoids the complexity of enabling distribution-specific repositories by downloading agents directly from [ClusterLabs/resource-agents](https://github.com/ClusterLabs/resource-agents) on GitHub.
It also avoids pulling in extraneous Pacemaker packages that are not needed for DRBD Reactor managed services.
It works consistently across all supported distributions without additional repository configuration.

Downloaded agents are stamped with a version marker comment after the shebang line.
On subsequent runs, the role uses this marker to distinguish its own downloads from system-installed agents:

- **File missing**: download and stamp with version marker.
- **File exists with marker, same version**: skip (already current).
- **File exists with marker, different version**: re-download and update marker.
- **File exists without marker**: system-installed RA, leave untouched.

The role also compiles C helper binaries from source when they are not already present.
Which helpers are compiled depends on the contents of `resource_agents_upstream_list`:

| Resource agent | C helper | Purpose |
|---|---|---|
| `IPaddr2` | `send_arp` | Gratuitous ARP announcement after IP takeover. |
| `portblock` | `tickle_tcp` | TCP connection tickling to drop stale sessions after failover. |

If none of the listed resource agents need a helper, no binaries are compiled and `gcc` is not installed.
The same version-stamp pattern applies: binaries installed by a system package are left untouched, and recompilation only occurs when the version changes.
Compilation requires `gcc`, which the role installs automatically when needed.

Requirements
------------

None.

Role Variables
--------------

| Variable | Default | Description |
|---|---|---|
| `resource_agents_upstream_version` | `v4.16.0` | GitHub release tag to download (for example `v4.17.0`). Empty string fetches the latest release. |
| `resource_agents_upstream_list` | See defaults | List of OCF resource agent names to ensure are present. |

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
        name: linbit.drbd_reactor.resource_agents_upstream
```

To install a specific version:

```yaml
    - name: Install OCF resource agents v4.17.0
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_upstream
      vars:
        resource_agents_upstream_version: v4.17.0
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
