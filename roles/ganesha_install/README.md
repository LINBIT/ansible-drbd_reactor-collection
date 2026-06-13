# ganesha_install

Install the [NFS-Ganesha](https://github.com/nfs-ganesha/nfs-ganesha) userspace NFS server.

## How it works

- The role installs the NFS-Ganesha daemon and the VFS FSAL packages (`nfs-ganesha` and `nfs-ganesha-vfs`).
- On the Debian family, the packages come from the distribution repositories.
- On Enterprise Linux, the role configures the [CentOS Storage SIG](https://sigs.centos.org/storage/projects/nfs-ganesha/) NFS-Ganesha repository and selects the newest version published for the node's major release.
- The `nfs-ganesha.service` systemd unit is stopped and disabled after installation.
  The [`ganesha-nfs`](https://github.com/ClusterLabs/resource-agents/blob/main/heartbeat/ganesha-nfs) OCF resource agent runs `ganesha.nfsd` directly and renders its own configuration, so the unit and `/etc/ganesha/ganesha.conf` stay unused.
- The kernel NFS server packages and service are left untouched.

## Requirements

NFS-Ganesha 4.0 or later is required.
Earlier versions cannot place NFSv4 recovery state on a replicated volume, so clients cannot reclaim opens and locks after failover.

The following distributions are supported, all packaging 4.x or later:

- Debian 12 or later
- Ubuntu 24.04 or later
- Enterprise Linux 9 or later

The role fails with a clear message on unsupported distributions and versions.

The SUSE family is excluded:

- openSUSE Leap 15 and SLES 15: NFS-Ganesha 3.x only
- openSUSE Leap 16 and SLES 16: no NFS-Ganesha packages

The `ganesha-nfs` OCF resource agent is not part of any `resource-agents` release yet.
Pair this role with the `resource_agents_upstream` role and set `resource_agents_upstream_version` to `main` until the agent ships in a release.

## Role variables

| Variable | Default | Description |
|---|---|---|
| `ganesha_install_package_state` | `present` | Package state for the NFS-Ganesha packages; set `latest` to check for upgrades |
| `ganesha_install_sig_version` | `""` | CentOS Storage SIG NFS-Ganesha repository version on Enterprise Linux; empty resolves the latest version published for the node's major release, or pin a version (minimum `4`) |

## Dependencies

None.

## Example playbook

```yaml
- name: Install NFS-Ganesha
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - name: Install the NFS-Ganesha userspace NFS server
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.ganesha_install
```

To pin a CentOS Storage SIG repository version on Enterprise Linux instead of resolving the latest:

```yaml
    - name: Install NFS-Ganesha 7 from the CentOS Storage SIG
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.ganesha_install
      vars:
        ganesha_install_sig_version: "7"
```

To install via `reactor_install` (disabled by default), together with the `ganesha-nfs` OCF resource agent:

```yaml
    - name: Install DRBD Reactor with NFS-Ganesha
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
      vars:
        reactor_install_ganesha: true
        resource_agents_upstream_version: main
```

## License

MIT

## Author information

[LINBIT](https://linbit.com)
