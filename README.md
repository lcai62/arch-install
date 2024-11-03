# arch-install


## Pre-Installation

## First Boot

### Connect to a network (if no ethernet)

```bash

root@archiso ~ # iwctl
root@archiso ~ # device list
root@archiso ~ # adapter phy0 set-property Powered on
root@archiso ~ # station wlan0 get-networks
root@archiso ~ # station wlan0 connect **SSID**
root@archiso ~ # station wlan0 show

root@archiso ~ # ping archlinux.org
```


### Partition disk
```bash
root@archiso ~ # lsblk
root@archiso ~ # cfdisk /dev/nvme0n1
```

create partitions:
/dev/nvme0n1p1 - 1G
/dev/nvme0n1p2 - 24G
/dev/nvme0n1p3 - Remaining

### Format partition
```bash
root@archiso ~ # mkfs.ext4 /dev/nvme0n1p3
root@archiso ~ # mkfs.fat -F 32 /dev/nvme0n1p1
root@archiso ~ # mkswap /dev/nvme0n1p2
```

### Mount partitions
```bash
root@archiso ~ # mount /dev/nvme0n1p3 /mnt
root@archiso ~ # mount /dev/nvme0n1p1 /mnt/boot/efi --mkdir
root@archiso ~ # swapon /dev/nvme0n1p2
```


### Installation
```bash
root@archiso ~ # pacstrap /mnt base linux linux-firmware sof-firmware base-devel grub efibootmgr nano networkmanager
```
### System configuration
```bash
root@archiso ~ # genfstab /mnt > /mnt/etc/fstab

root@archiso ~ # arch-chroot /mnt

[root@archiso /]# ln -sf /usr/share/zoneinfo/Canada/Eastern /etc/localtime
[root@archiso /]# hwclock --systohc

[root@archiso /]# nano /etc/locale.gen
```
Uncomment en_US.UTF-8 UTF-8
```bash
[root@archiso /]# locale-gen
[root@archiso /]# nano /etc/locale.conf
```
Write LANG=en_US.UTF-8
```bash
[root@archiso /]# nano /etc/vconsole.conf
```
Write KEYMAP=us
```bash
[root@archiso /]# nano /etc/hostname
```
Write **hostname**

```bash
[root@archiso /]# passwd
[root@archiso /]# useradd -m -G wheel -s /bin/bash chazzybear
[root@archiso /]# passwd chazzybear

[root@archiso /]# EDITOR=nano visudo
```
Uncomment %wheel ALL=(ALL) ALL

```bash
[root@archiso /]# systemctl enable NetworkManager
```

### Bootloader
```bash
[root@archiso /]# grub-install /dev/nvme0n1
[root@archiso /]# grub-mkconfig -o /boot/grub/grub.cfg
[root@archiso /]# exit
[root@archiso /]# umount -a
[root@archiso /]# reboot
```

## Setting up GUI
```bash
[chazzybear@arch ~]$ nmcli device wifi list
[chazzybear@arch ~]$ nmcli device wifi connect "SSID" password "PASSWORD"
[chazzybear@arch ~]$ ping archlinux.org

[chazzybear@arch ~]$ sudo pacman -S cinnamon lightdm lightdm-gtk-greeter
[chazzybear@arch ~]$ sudo pacman -S konsole firefox kate

[chazzybear@arch ~]$ sudo systemctl enable lightdm
```






