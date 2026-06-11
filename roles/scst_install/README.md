# scst_install

Compile and install the [SCST](https://github.com/SCST-project/scst) iSCSI target from source.

## How it works

- SCST is skipped entirely on nodes where the `scst` kernel module is already loaded.
- One node that needs SCST is elected as the fetch host; it clones the SCST repository once.
- The source archive is cached on the Ansible control host at `/tmp/linbit-scst/<version>.tar.gz`, then distributed to the remaining nodes, avoiding repeated GitHub clones in larger clusters.
- Each node builds and installs SCST packages with DKMS, so the module survives kernel updates.
- The `scst`, `iscsi-scst`, and `scst_vdisk` kernel modules are loaded persistently, and an `iscsi-scst.service` unit is created, started, and enabled.

## Requirements

The Red Hat and Debian OS families are supported.
EPEL provides `dkms` on Enterprise Linux, and the role enables it during the build.

## Role variables

| Variable | Default | Description |
|---|---|---|
| `scst_install_version` | `""` | Empty resolves the latest SCST release tag (see [releases](https://github.com/SCST-project/scst/releases)); or pin a tag, branch, or commit SHA |

By default the latest SCST release is built (`v3.10` today).
Its RPM build fails on newer Enterprise Linux kernels (EL9.8+, EL10), so on those nodes set `scst_install_version: master` until SCST 3.11 is released.
The Debian packaging of the same release carries kernel-compatibility patches, so it builds on newer Debian and Ubuntu kernels (for example Debian 13 on 6.12 and Ubuntu 24.04 on 6.8).

## Dependencies

None.

## Example playbook

```yaml
- name: Install SCST
  hosts: all
  any_errors_fatal: true
  become: true
  tasks:
    - name: Install SCST iSCSI target from source
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.scst_install
```

To pin a git reference:

```yaml
    - name: Install SCST from master
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.scst_install
      vars:
        scst_install_version: master
```

To install via `reactor_install` (disabled by default):

```yaml
    - name: Install DRBD Reactor with SCST
      ansible.builtin.import_role:
        name: linbit.drbd_reactor.reactor_install
      vars:
        reactor_install_scst: true
```

## License

MIT

## Author information

[LINBIT](https://linbit.com)
