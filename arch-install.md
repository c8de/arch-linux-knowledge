# Arch install
https://arcolinuxd.com/5-the-actual-installation-of-arch-linux-phase-1-uefi/

1. Change the keyboard layout
   > loadkeys de_CH-latin1
1. Verify if the installation supports EFI by checking if the following directory exists:
   > ls /sys/firmware/efi/efivars
1. Connect to the internet
   * install supports automatic connection over ethernet with DHCP
   * can be verfied with ping
     > ping archlinux.org
1. Update the system clock
   > timedatectl set-ntp true\
   > timedatectl status
1. [OPTIONAL] using existing bootloader
   > fdisk -l\
   > mount /dev/sda1 /boot
1. Partition the disk
   1. find the name of the harddrive
      > fdisk -l
   1. cfdisk /dev/sda
      1. Delete all unwanted partitions
      1. Example partition table (GPT)
         > Sda1 550mb...1g efi system\
         > Sda2 100...200g linux root (x86-64)\
         > Sda3 20g linux swap (2xRAM)\
         > Sda4 rest linux home
1. Format the partitions
   > mkfs.fat -F32 /dev/sda1\
   > mkswap /dev/sda3\
   > swapon /dev/sda3\
   > mkfs.ext4 /dev/sda2\
   > mkfs.ext4 /dev/sda4
1. Mount the partitions
   > mount /dev/sda2 /mnt\
   > mkdir /mnt/boot\
   > mount /dev/sda1 /mnt/boot\
   > mkdir /mnt/home\
   > mount /dev/sda4 /mnt/home
1. Check the partitions
   > lsblk
1. Edit the mirror list (to download software from)
   > nano /etc/pacman.d/mirrorlist
   * move the closest mirror to the top
1. Install base and base-devel
   > pacstrap /mnt base base-devel
1. Generate fstab
   > genfstab -U /mnt >> /mnt/etc/fstab
1. Check fstab with
   > nano /mnt/etc/fstab
1. Chroot into the new system
   > arch-chroot /mnt
1. Set the timezone
   > ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime\
1. Set the system time to hardware clock
   > hwclock --systohc
1. Set the localization
   > nano /etc/locale.gen
   * Uncomment en_US.UTF-8 UTF-8
   * Uncomment de_CH.UTF-8 UTF-8
   > locale-gen
1. Set the language
   > nano /etc/locale.conf\
   * LANG=de_CH.UTF-8\
   * [OR] LANG=en_US.UTF-8
1. Set the keyboard layout
   > nano /etc/vconsole.conf
   * KEYMAP=de_CH-latin1
1. Set the hostname
   > nano /etc/hostname
   * *pc-name*
1. Set the network connection
   > nano /etc/hosts
   * 127.0.0.1 localhost
   * ::1 localhost
   * 127.0.1.1 *pc-name*.localdomain *pc-name*
1. Install network manager
   > pacman -S networkmanager\
   > systemctl enable NetworkManager
1. Configure network connections
   > nm-connection-editor
1. Set the root password
   > passwd
1. Install the bootloader
   > pacman -S grub efibootmgr\
   > grub-install --target=x86_64-efi --efi-directory=/boot\
   > grub-mkconfig -o /boot/grub/grub.cfg
1. Reboot the system
   > exit\
   > umount -R /mnt\
   > reboot

# Post arch install

## Login as root
   > Root\
   > Password

## Create a new user
   > useradd -m *user-name*\
   > passwd *user-name*

## Update the system
   > pacman -Syu

## Install the desktop environment Gnome
   > pacman -S gnome\
   > systemctl enable gdm\
   > reboot
### Install Gnome extensions if wished
    * Install dash to panel gnome extension
    * Install applications menu
    * Install desktop-icons
### Install the Gnome tweak tool to modify appearance
    > pacman -S gnome-tweak-tool\
    > gnome-tweak-tool\
    > gnome-tweaks (to start as a user other than root)

## [OPTIONAL] Install Nvidia driver
1. Check GPU
   > lspci -k | grep -A 2 -E "(VGA|3D)"
1. Install driver
   > pacman -S nvidia nvidia-settings\
   > reboot (nouveau is blacklisted automatically)
