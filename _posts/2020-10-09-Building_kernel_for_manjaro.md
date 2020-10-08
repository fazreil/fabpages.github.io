---
title:          "Building Kernel for Manjaro"
date:           2020-10-09
category:       Tech
tag:            Tech[manjaro, linux, laptop, legion]
---


# New laptop

I've been wanting to buy myself a new laptop, I promised myself I will get an AMD this time.

I considered a lot of laptop models. Asus Zephyrus G14, Asus TUF A15, Acer Nitro, HP Pavillion, MSI. But after seeing Lenovo Legion 5, I kept making excuses why I should not choose the others

* Asus Zephyrus G14 - No Home, Pg Up, Pg Down, End key. Otherwise this is the best machine.
* Asus TUF A15 - Specs are great, just not there. I can't make good excuse for this one, honest.
* Acer Nitro - Small keys
* HP Pavillion - Too new, high chance of unsupported drivers.
* MSI - Red keys

## Legion 5

Why I choose Lenovo Legion:

* I used Thinkpad X20 before. It brings back nostalgia
* The keys on the keyboard is just great
* The build is great
* I don't want my laptop to look too flashy.

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

# Problem with the touchpad on Legion 5 15ARH05

...which brings us to the titular subject. As you can see there, I wiped out Windows 10 it came with and installed Pop!_Os first before deciding to go for Manjaro 20.1 Minkah. I want to use Arch based linux for a change.

![manjaro-desktop]({{"/assets/manjaro-desktop.png"}})

Beautiful, sleek and very comfortable to use. Little did I know there was an issue with the touchpad. The cursor just don't move. It just don't work. :disappointed:

ref: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1887190

## Looking for fix

The webpage above actually provide a solution for the issue. But I need to build the patched kernel myself. I've never build a kernel to be honest (despite being a senior build engineer, this is embarassing :expressionless:). The patch isn't going to be merged into the official build anytime soon because it will break other touchpads.

ref: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1887190/comments/179

## Building new kernel with patched driver

I followed this step:
https://medium.com/@evintheair/building-a-custom-kernel-in-manjaro-linux-186da6a1cedf

Here is the rundown of what I did that night:

1. Download linux58 package<br/>
`wget https://gitlab.manjaro.org/packages/core/linux58/-/archive/master/linux58-master.tar.gz`
1. Extract it to my checkout directory<br/>
`tar xvzf linux58-master.tar.gz`
1. change directory to the extracted <br/>
`cd linux58-master`
1. download patch for Legion 5 touchpad <br/>
`wget https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1887190/+attachment/5418465/+files/0303-pinctrl-amd2.patch`
1. Copy the file over to the extracted linux58 <br/>
`cp ~/Downloads/0303-pinctrl-amd2.patch .`
1. Update checksums <br/>
`updpkgsums`
1. While making the package, I got this error:<br/>
`==> ERROR: Cannot find the fakeroot binary required for building as non-root user.`
1. This can be solved by installing base-devel <br/>
`sudo pacman -S base-devel`
1. The package brings fakeroot, now let's make the package <br/>
`makepkg -s`
1. Now let the system compile and make the new kernel package, together with the headers

### What was that patch about
I went to check what the patch is all about

`cat 0303-pinctrl-amd2.patch`

and this is what I found:

```
diff --git a/drivers/pinctrl/pinctrl-amd.c b/drivers/pinctrl/pinctrl-amd.c
index 9a760f5cd7ed..e786d779d6c8 100644
--- a/drivers/pinctrl/pinctrl-amd.c
+++ b/drivers/pinctrl/pinctrl-amd.c
@@ -472,7 +472,7 @@ static int amd_gpio_irq_set_type(struct
 		pin_reg &= ~(ACTIVE_LEVEL_MASK << ACTIVE_LEVEL_OFF);
 		pin_reg |= ACTIVE_LOW << ACTIVE_LEVEL_OFF;
 		pin_reg &= ~(DB_CNTRl_MASK << DB_CNTRL_OFF);
-		pin_reg |= DB_TYPE_PRESERVE_HIGH_GLITCH << DB_CNTRL_OFF;
+		/** pin_reg |= DB_TYPE_PRESERVE_HIGH_GLITCH << DB_CNTRL_OFF; */
 		irq_set_handler_locked(d, handle_level_irq);
 		break;
```
The patch actually removes one line from one of the driver to make the touchpad works, got it

![got it](https://media3.giphy.com/media/1X7lCRp8iE0yrdZvwd/giphy.gif)


## Installing the newly built Kernel

Ok, after 10 minutes or so, the build completed successfully

![this]({{ "/assets/compilation-complete.png"}})

The suspense is mounting. So far my personal achievement when dealing with kernel...

Install new kernel: :heavy_check_mark:<br/>
Build new kernel: :thumbsup: <br/>
Install newly built own kernel :question:

Time to install the newly built kernel

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

# Reboot

Next, I kept faith in all kernel developers and reboot the machine. Praise to god, the new kernel works!

```
~ >>> screenfetch                                                              

 ██████████████████  ████████     fazreil@Legion5
 ██████████████████  ████████     OS: Manjaro 20.1 Mikah
 ██████████████████  ████████     Kernel: x86_64 Linux 5.8.14-1-MANJARO
 ██████████████████  ████████     Uptime: 3m
 ████████            ████████     Packages: 1433
 ████████  ████████  ████████     Shell: zsh 5.8
 ████████  ████████  ████████     Resolution: 1920x1080
 ████████  ████████  ████████     DE: GNOME 3.36.4
 ████████  ████████  ████████     WM: Mutter
 ████████  ████████  ████████     WM Theme: Matcha-dark-sea
 ████████  ████████  ████████     GTK Theme: Matcha-sea [GTK2/3]
 ████████  ████████  ████████     Icon Theme: Papirus-Dark-Maia
 ████████  ████████  ████████     Font: Noto Sans 11
 ████████  ████████  ████████     Disk: 75G / 697G (12%)
                                  CPU: AMD Ryzen 5 4600H with Radeon Graphics @ 12x 3GHz
                                  GPU: GeForce GTX 1650
                                  RAM: 1628MiB / 15434MiB
~ >>> uname -r                                                                 
5.8.14-1-MANJARO
~ >>>    
```

More interestingly the touchpad now works!

Now I can unplug my laptop and carry it around
 without carrying mouse with me.

p.s. I could use [i3](https://en.wikipedia.org/wiki/I3_(window_manager) but when it comes to browsing on firefox, I really need the touchpad.
