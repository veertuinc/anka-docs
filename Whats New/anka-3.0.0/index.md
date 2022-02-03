---
date: 2021-10-25T01:00:00-00:00
title: "Anka Virtualization 3.0.0"
---

### Previous tag deletion method has been replaced

Previously you had to issue `anka delete {template}:{tag}` in order to remove a tag locally (for example if you needed to re-push it to the registry). This has been been replaced with `anka delete {template} --tag {tag}`.

### Multi-stage `anka create` for MDM profile application

Some users find that they need to set the `hw.serial` for a VM before they start the installation of macOS within a VM. We now allow users to issue `anka create {VMNAME}` first, then `anka modify` to set the `hw.serial`, and then continue with the creation using `anka create -a {PATH TO INSTALLER} {VMNAME}. . .`.

### Ability to control VM display frame rate

```bash
Veertu:~ root# anka modify 11.5.2 set display --help
Usage: anka modify set display [OPTIONS]

  Configure displays

Options:
 . . .
  -f, --fps INTEGER           Set refresh rate (30fps by default)
 . . .
```

### Additional `anka show` commands

1. Ability to display information about specific Template Tag:

    ```bash
    ❯ sudo anka show 11.5.2 tags
    +----------------------------------+----------------------+----------------------+
    | tag                              | creation_date        | last_access          |
    +----------------------------------+----------------------+----------------------+
    | vanilla+port-forward-22+brew-git | Aug 18 16:40:28 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+
    | vanilla                          | Aug 18 16:21:15 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+
    | vanilla+port-forward-22          | Aug 18 16:24:35 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+


    ❯ sudo anka show 11.5.2 --tag vanilla+port-forward-22
    +---------+--------------------------------------+
    | uuid    | c12ccfa5-8757-411e-9505-128190e9854e |
    +---------+--------------------------------------+
    | name    | 11.5.2                               |
    +---------+--------------------------------------+
    | created | Aug 18 16:24:35 2021                 |
    +---------+--------------------------------------+
    | vcpu    | 4                                    |
    +---------+--------------------------------------+
    | memory  | 8G                                   |
    +---------+--------------------------------------+
    | display | 1024x768                             |
    +---------+--------------------------------------+
    | disk    | 100GiB (17.31GiB on disk)            |
    +---------+--------------------------------------+
    | addons  | 2.5.0.131 10                         |
    +---------+--------------------------------------+
    | network | shared                               |
    +---------+--------------------------------------+
    | status  | stopped Aug 18 16:24:35 2021         |
    +---------+--------------------------------------+
    ```

1. Display verbose disk information for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 disk
    +--------------+--------------------------------------+
    | controller   | virtio-blk                           |
    +--------------+--------------------------------------+
    | file         | 635c27511ed4453798ab40dcccebe0f0.ank |
    +--------------+--------------------------------------+
    | pci_slot     | 4                                    |
    +--------------+--------------------------------------+
    | image_format | anka                                 |
    +--------------+--------------------------------------+
    | image_size   | 100GiB                               |
    +--------------+--------------------------------------+
    | block_size   | 1MiB                                 |
    +--------------+--------------------------------------+
    | data_size    | 26.84GiB                             |
    +--------------+--------------------------------------+
    | snapshot     | No                                   |
    +--------------+--------------------------------------+
    | encrypted    | No                                   |
    +--------------+--------------------------------------+
    ```

2. Display verbose network information for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 network
    +----------+-------------------+
    | mode     | shared            |
    +----------+-------------------+
    | pci_slot | 28                |
    +----------+-------------------+
    | type     | virtio-net        |
    +----------+-------------------+
    | ip       | 192.168.64.59     |
    +----------+-------------------+
    | mac      | ce:de:e3:e8:44:75 |
    +----------+-------------------+
    | hostif   | en7               |
    +----------+-------------------+

    port_forwarding_rules:
    ```

    > You may notice that `hostif` is set to `en7`. This is the interface on the host the VM is using and it's useful when you want to scan and watch packets with tools like `tcpdump` on the host.

3. Display local tags for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 tags
    +-------------------------+----------------------+----------------------+
    | tag                     | creation_date        | last_access          |
    +-------------------------+----------------------+----------------------+
    | vanilla                 | Aug 18 16:21:15 2021 | Aug 18 16:24:34 2021 |
    +-------------------------+----------------------+----------------------+
    | vanilla+port-forward-22 | Aug 18 16:24:35 2021 | Aug 18 16:24:34 2021 |
    +-------------------------+----------------------+----------------------+
    ```

