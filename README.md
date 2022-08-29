# servers-baremetal-qnap

## Purpose

This Ansible playbook installs Ubuntu 20.04 LTS on [QNAP TS-1277XU-RP](https://www.qnap.com/en/product/ts-1277xu-rp/specs/hardware) booted from Ubuntu Focal server flash drive install media. Root file system will be placed on ZFS, but the kernels and bootloader have to be placed on the internal USB flash drive, because the firmware doesn't expose the SATA drives in the bootloader phase and it is unable to load a bootloader from them as well.

## Prerequisities

The target machine has to be booted from the Ubuntu LTS server installation medium, the network needs to be set up and user *root* has to have your SSH public key present in the authorized_keys file. It is advisable to upgrade your NAS to latest QTS beforehand, also perform NIC and HBA firmware updates if offered to do so. When booted into install media you can backup your internal flash (e.g. via mbuffer over the network).

## Usage

Since the playbook erases all data on the server, the hosts variable needs to be initialized by an external variable called *target* as a security precaution.

e.g. ansible-playbook site.yml -e "target=ts-1277xu-rp"

Otherwise, the playbook will fail.

## Warning

As stated above - this playbook **destroys all ZFS pools** on the target machine. Use it only on servers which contain no usable data.
**You have been warned**

## Notes
- You need to define your SSH public key in the host variables file, as well as drive paths (both USB and SATA)
- This playbook is basically an attempt to implement [Ubuntu 20.04 Root on ZFS](https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/Ubuntu%2020.04%20Root%20on%20ZFS.html) HOWTO