# arch-zfs-esp-sync

For UEFI Arch Linux systems that don't mount the ESP on /boot and wanting to
boot the kernel directly using EFISTUB without a bootloader.

This package includes a systemd unit to monitor the specified kernel
images, copy them to the ESP, and generate the required UEFI scripts.

This is built for the author's own usage requirements, no backward
compatibility will be maintained, if you find it useful, you should fork it
and maintain your own copy for your needs.

## Build

```bash
makepkg
```

## Install

```bash
pacman -U *.pkg.tar.xz
```

## Configuration

To monitor kernel image files for specific kernel packages, enable and
start the corresponding `zfs-esp-sync@.service`, for example, to monitor
both the `linux` and `linux-lts` packages:

```bash
systemctl enable zfs-esp-sync@linux.service
systemctl start zfs-esp-sync@linux.service

systemctl enable zfs-esp-sync@linux-lts.service
systemctl start zfs-esp-sync@linux-lts.service
```

This assume each package creates the following files:

```bash
/boot/vmlinuz-${package-name}
/boot/initramfs-${package-name}.img
/boot/initramfs-${package-name}-fallback.img
```

Create `/etc/zfs-esp-sync` (a bash file) with the suitable options.

Example config:

```bash
#
# Default boot target to be started by the <esp>/startup.nsh file,
# usually the name of the package, such as linux, linux-lts.
#
# The files will be searched are:
#   - /boot/vmlinuz-${target}
#   - /boot/initramfs-${target}.img
#   - /boot/initramfs-${target}-fallback.img
#
default=linux

#
# Where is the ESP? One of the 'FS' entries returned by the 'map'
# command ran inside an UEFI shell. Usually fs0.
#
esp_fs=fs0

#
# Where the ESP is mounted.
#
esp_mount=/boot/efi
```

Example files generated using above config:

```
<esp>
- startup.nsh
- EFI
  - arch
  
    - vmlinuz-linux.efi
    - initramfs-linux.img
    - initramfs-linux-fallback.img
    - start-linux.nsh
    - start-linux-fallback.nsh

    - vmlinuz-linux-lts.efi
    - initramfs-linux-lts.img
    - initramfs-linux-lts-fallback.img
    - start-linux-lts.nsh
    - start-linux-lts-fallback.nsh

```

The startup file `startup.nsh` will invoke `start-linux.nsh` by default during
boot, other start scripts can be invoked manually if required while in the UEFI
shell.

## Manual Execution

```bash
zfs-esp-sync <package, e.g. linux, linux-lts>
```
