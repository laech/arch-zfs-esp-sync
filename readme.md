
# arch-zfs-esp-sync

For UEFI Arch Linux systems that don't mount the ESP on /boot and wanting to
boot the kernel directly using EFISTUB without a bootloader.

This package includes a script to copy the kernel images to the ESP, generate
the required UEFI scripts, and a pacman hook to keep them updated when the
kernal and related packages are updated.

This is built for the author's own usage requirements, no backward
compatibility will be maintained, if you find it useful, you should fork it
and maintain your own copy for your needs.

## Configuration

Create `/etc/zfs-esp-sync` (a bash file) with the suitable options.

Example config:

```bash
#
# List of targets, usually the name of the package.
#
# The files will be searched are:
#   - /boot/vmlinuz-${target}
#   - /boot/initramfs-${target}.img
#   - /boot/initramfs-${target}-fallback.img
#
# The last entries will be the default to be started by the root
# <esp>/startup.nsh file.
#
targets=(linux-lts linux)

#
# Where is the ESP? One of the 'FS' entries return by the 'map'
# command ran inside an UEFI shell. Usuall fs0.
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
- EFI
  - startup.nsh
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

## Build

```bash
makepkg
```

## Install

```bash
pacman -U *.pkg.tar.xz
```

## Manual Execution

```bash
/usr/share/libalpm/scripts/zfs-esp-sync
```
