# resource_agents_upstream

Install OCF resource agents from [`ClusterLabs/resource-agents`](https://github.com/ClusterLabs/resource-agents) on GitHub.

## How it works

- This role trades the cost of [compiling helper binaries](#c-helper-binaries) to avoid downsides with OS-packaged resource agents (missing select resource agents, older versions, etc).
- It works consistently across all supported distributions without the need for extra distribution-specific HA repositories (Red Hat family distributions).
- It also avoids pulling in extraneous Pacemaker packages (not needed for DRBD Reactor managed services).
- The control host is the only host that fetches content from GitHub.
- Resource agents, OCF shell library files, and C helper sources are saved to `/tmp/linbit-resource-agents/<version>/`, then distributed to target nodes.
- This avoids GitHub rate limiting in larger clusters.
- C helper binaries are compiled on each node to match its architecture and C library.

Downloaded agents are stamped with a version marker comment after the shebang line:

```bash
#!/bin/sh
# Managed by linbit.drbd_reactor.resource_agents_upstream v4.16.0
```

On subsequent runs, the role uses this marker to distinguish its own downloads from system-installed agents:

| State | Action |
|---|---|
| Resource agent (RA) missing | Download and stamp with version marker |
| RA exists with marker, same version | Skip (already current) |
| RA exists with marker, different version | Re-download and update marker |
| RA exists without marker | System-installed RA, leave untouched |

## C helper binaries

The role also compiles C helper binaries from source when they are not already present.
Which helpers are compiled depends on the contents of `resource_agents_upstream_list`:

| Resource agent | C helper | Purpose |
|---|---|---|
| `IPaddr2` | `send_arp` | Gratuitous ARP announcement after IP takeover |
| `portblock` | `tickle_tcp` | TCP connection tickling to drop stale sessions after failover |

The role installs `gcc` and compiles helpers only when an agent in `resource_agents_upstream_list` needs one (currently only `IPaddr2` or `portblock`).
When an agent that uses a helper is updated, the binary is also checked and recompiled if needed.

## Requirements

None.

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `resource_agents_upstream_version` | `""` | Empty (default) fetches the latest upstream release; pin to a GitHub release tag (for example `v4.18.0`) for reproducibility |
| `resource_agents_upstream_list` | See defaults | List of OCF resource agent names to ensure are present |

### Default resource agents list

- [`portblock`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/portblock)
- [`Filesystem`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/Filesystem)
- [`IPaddr2`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/IPaddr2)
- [`iSCSITarget`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/iSCSITarget.in)
- [`iSCSILogicalUnit`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/iSCSILogicalUnit.in)
- [`nfsserver`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/nfsserver)
- [`exportfs`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/exportfs)
- [`nvmet-subsystem`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/nvmet-subsystem)
- [`nvmet-namespace`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/nvmet-namespace)
- [`nvmet-port`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/nvmet-port)

## Dependencies

None.

## Example Playbook

```yaml
- name: Install OCF resource agents
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - name: Install OCF resource agents from GitHub (if absent)
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_upstream
```

To pin to a specific version:

```yaml
    - name: Install OCF resource agents v4.18.0
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.resource_agents_upstream
      vars:
        resource_agents_upstream_version: v4.18.0
```

To install via `reactor_install` (enabled by default):

```yaml
    - name: Install DRBD Reactor with OCF resource agents
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
      vars:
        reactor_install_resource_agents_upstream: true  # true by default
```

## Distribution availability of resource-agents

For reference, the `resource-agents` package availability varies across distributions:

| Distribution | Package | Repository |
|---|---|---|
| Debian, Proxmox VE, Ubuntu 22.04 and earlier | `resource-agents` | Default repositories |
| Ubuntu 24.04 and later | `resource-agents-base` | Default repositories; [`IPaddr2`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/IPaddr2) moved to a separate package |
| Red Hat 8/9 family with LINBIT customer repos | `resource-agents` | Included in the LINBIT `drbd-9` or `pacemaker-2` customer repository |
| Red Hat 10 family with LINBIT customer repos | `resource-agents` | Included in the LINBIT `pacemaker-3` customer repository (not yet in `drbd-9`) |
| Red Hat (without LINBIT repos) | `resource-agents` | Requires the HA Add-On subscription (`highavailability-rpms`) |
| AlmaLinux, Rocky Linux | `resource-agents` | Requires enabling the `ha` repository |
| Oracle Linux | `resource-agents` | Requires enabling the `addons` repository |
| SLES, openSUSE Leap | `resource-agents` | Default repositories |

## License

MIT

## Author Information

[LINBIT](https://linbit.com)
