---
title:          "Building Kernel for Manjaro"
date:           2020-10-09
category:       Tech
tag:            Tech[mantra, class]
---


# New laptop

I bought a new laptop and it is Lenovo Legion-5 15ARH05. It is such a cool machine. I've been wanting to buy myself a new laptop, I promised myself I will get an AMD this time.



So this is what I'm scoring...

```
██████████████████  ████████     fazreil@Legion5
██████████████████  ████████     OS: Manjaro 20.1 Mikah
██████████████████  ████████     Kernel: x86_64 Linux 5.8.13-3-MANJARO
██████████████████  ████████     Uptime: 1h 25m
████████            ████████     Packages: 1433
████████  ████████  ████████     Shell: zsh 5.8
████████  ████████  ████████     Resolution: 1920x1080
████████  ████████  ████████     DE: GNOME 3.36.4
████████  ████████  ████████     WM: Mutter
████████  ████████  ████████     WM Theme: Matcha-dark-sea
████████  ████████  ████████     GTK Theme: Matcha-sea [GTK2/3]
████████  ████████  ████████     Icon Theme: Papirus-Dark-Maia
████████  ████████  ████████     Font: Noto Sans 11
████████  ████████  ████████     Disk: 73G / 697G (11%)
                                 CPU: AMD Ryzen 5 4600H with Radeon Graphics @ 12x 3GHz
                                 GPU: GeForce GTX 1650
                                 RAM: 4166MiB / 15434MiB
```

...which brings us to the subject. As you can see there I wiped out Windows 10 it came with and installed Manjaro 20.1 Minkah. Beautiful, sleek and very comfortable to use. Little did I know there was an issue with the touchpad. The cursor just don't move. It just don't work.

# Building new kernel with patched driver


# Installing the newly built Kernel

Install new kernel :thumbsup:
Install newly built own kernel :question:

```
~/.../linux58/linux58-master >>> sudo pacman -U ./linux58-5.8.14-1-x86_64.pkg.tar.xz ./linux58-headers-5.8.14-1-x86_64.pkg.tar.xz
[sudo] password for fazreil:
loading packages...
resolving dependencies...
looking for conflicting packages...

Packages (2) linux58-5.8.14-1  linux58-headers-5.8.14-1

Total Installed Size:  197.55 MiB
Net Upgrade Size:        0.03 MiB

:: Proceed with installation? [Y/n] Y
(2/2) checking keys in keyring                                  [##################################] 100%
(2/2) checking package integrity                                [##################################] 100%
(2/2) loading package files                                     [##################################] 100%
(2/2) checking for file conflicts                               [##################################] 100%
(2/2) checking available disk space                             [##################################] 100%
:: Running pre-transaction hooks...
(1/2) Removing linux initcpios...
(2/2) Save Linux kernel modules
:: Processing package changes...
(1/2) upgrading linux58                                         [##################################] 100%
(2/2) upgrading linux58-headers                                 [##################################] 100%
:: Running post-transaction hooks...
(1/5) Arming ConditionNeedsUpdate...
(2/5) Updating module dependencies...
(3/5) Updating linux initcpios...
==> Building image from preset: /etc/mkinitcpio.d/linux58.preset: 'default'
  -> -k /boot/vmlinuz-5.8-x86_64 -c /etc/mkinitcpio.conf -g /boot/initramfs-5.8-x86_64.img
==> Starting build: 5.8.14-1-MANJARO
  -> Running build hook: [base]
  -> Running build hook: [udev]
  -> Running build hook: [autodetect]
  -> Running build hook: [modconf]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [resume]
  -> Running build hook: [keyboard]
  -> Running build hook: [fsck]
==> Generating module dependencies
==> Creating gzip-compressed initcpio image: /boot/initramfs-5.8-x86_64.img
==> Image generation successful
==> Building image from preset: /etc/mkinitcpio.d/linux58.preset: 'fallback'
  -> -k /boot/vmlinuz-5.8-x86_64 -c /etc/mkinitcpio.conf -g /boot/initramfs-5.8-x86_64-fallback.img -S autodetect
==> Starting build: 5.8.14-1-MANJARO
  -> Running build hook: [base]
  -> Running build hook: [udev]
  -> Running build hook: [modconf]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [resume]
  -> Running build hook: [keyboard]
  -> Running build hook: [fsck]
==> Generating module dependencies
==> Creating gzip-compressed initcpio image: /boot/initramfs-5.8-x86_64-fallback.img
==> Image generation successful
(4/5) Updating Grub-Bootmenu
Generating grub configuration file ...
Found theme: /usr/share/grub/themes/manjaro/theme.txt
Found linux image: /boot/vmlinuz-5.8-x86_64
Found initrd image: /boot/intel-ucode.img /boot/amd-ucode.img /boot/initramfs-5.8-x86_64.img
Found initrd fallback image: /boot/initramfs-5.8-x86_64-fallback.img
Found linux image: /boot/vmlinuz-5.4-x86_64
Found initrd image: /boot/intel-ucode.img /boot/amd-ucode.img /boot/initramfs-5.4-x86_64.img
Found initrd fallback image: /boot/initramfs-5.4-x86_64-fallback.img
Found linux image: /boot/vmlinuz-5.4-rt-x86_64
Found initrd image: /boot/intel-ucode.img /boot/amd-ucode.img /boot/initramfs-5.4-rt-x86_64.img
Found initrd fallback image: /boot/initramfs-5.4-rt-x86_64-fallback.img
Adding boot menu entry for UEFI Firmware Settings ...
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe: warning: unknown device type nvme0n1.
done
(5/5) Restore Linux kernel modules

==> Warning:
	 -> Kernel has been updated. Modules of the current kernel
	 -> have been backed up so you can continue to use your
	 -> computer. However, the new kernel will only work
	 -> at next boot.


~/.../linux58/linux58-master >>>            
```
