---
layout: single
title: Install ArchLinux from Kubuntu
tags:
  - arch
  - kubuntu
  - bootstrap
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Arch Linux
---

# Install tools

```bash
sudo apt install arch-install-scripts pacman
```

# Partition

Don't shoot yourself, use KDE Partition Manager to create a new XFS with LUKS partition.

# Download bootstrap

```bash
mkdir ~/archlinux && cd ~/archlinux
wget https://fastly.mirror.pkgbuild.com/iso/2026.01.01/archlinux-bootstrap-x86_64.tar.zst
tar --zstd -xvf archlinux-bootstrap-x86_64.tar.zst --strip-components 1
```

# Mount

```bash
sudo cryptsetup luksOpen /dev/nvme0n1p3 cryptroot
sudo mount --mkdir /dev/mapper/cryptroot ./mnt/
sudo mount --mkdir /dev/nvme0n1p1 ./mnt/boot/
```

# Chrooting to the Arch bootstrap

```bash
# Copy your host DNS so the chroot has internet
sudo cp /etc/resolv.conf etc/resolv.conf

# Enter the temporary Arch environment
sudo bin/arch-chroot .
```

# Setup package manager (pacman)

Workaround before installing:

```bash
sed -i 's/^CheckSpace/#CheckSpace/' /etc/pacman.conf
```

Setup pacman.

```bash
echo "Server = https://geo.mirror.pkgbuild.com/\$repo/os/\$arch" > /etc/pacman.d/mirrorlist
pacman-key --init
pacman-key --populate archlinux

# Updating
pacman -Sy archlinux-keyring --noconfirm
```

```bash
$ cat /etc/mkinitcpio.conf | grep -v "^#"
MODULES=(i915)
BINARIES=()
FILES=()
HOOKS=(base systemd autodetect microcode modconf kms plymouth keyboard keymap sd-vconsole sd-encrypt block filesystems fsck)
``

```bash
$ cat /etc/kernel/cmdline 
rw quiet splash loglevel=3 rd.systemd.show_status=auto rd.udev.log_level=3 rd.luks.name=3bd3e154-c741-4863-942a-c93bfa43ed38=cryptroot root=/dev/mapper/cryptroot
```

```bash
$ cat /etc/mkinitcpio.d/linux.preset | grep -v "#"

ALL_kver="/boot/vmlinuz-linux"

PRESETS=('default')

default_uki="/boot/EFI/Linux/arch-linux.efi"
```