# Arch install VM (VMware)

1. Download and flash the ISO file on an USB stick with balena etcher
1. Connect the PC to the internet over ethernet (easiest)
1. Change the keyboard layout
   > loadkeys de_CH-latin1
1. Update the system clock
   > timedatectl set-ntp true\
   > timedatectl status
1. Partition the disk
   1. find the name of the harddrive
      > fdisk -l
   1. cfdisk
      1. Delete all unwanted partitions
      1. Example partition table (DOS)
         > Sda1 16g linux bootable\
         > Sda2 4g linux (2xRAM)
1. Format the partitions
   > mkswap /dev/sda2\
   > swapon /dev/sda2\
   > mkfs.ext4 /dev/sda1
1. Mount the partitions
   > mount /dev/sda1 /mnt
1. Check the partitions
   > lsblk
1. [OPTIONAL] Edit the mirror list (to download software from)
   > nano /etc/pacman.d/mirrorlist
   * move the closest mirror to the top
1. Install essential packages
   > pacstrap /mnt base linux linux-firmware
1. Generate fstab file
   > genfstab -U /mnt >> /mnt/etc/fstab
1. Check fstab with
   > nano /mnt/etc/fstab
1. Chroot into the new system
   > arch-chroot /mnt
1. Set the timezone
   > ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime
1. Set the system time to hardware clock
   > hwclock --systohc
1. Install nano text editor
   > pacman -S nano
1. Set the localization
   > nano /etc/locale.gen
   * Uncomment en_US.UTF-8 UTF-8
   * Uncomment de_CH.UTF-8 UTF-8
   > locale-gen
1. Create the local.conf file to set the localization
   > nano /etc/locale.conf
   * LANG=de_CH.UTF-8
   * [OR] LANG=en_US.UTF-8
1. Create the vconsole.conf file to set the keyboard layout
   > nano /etc/vconsole.conf
   * KEYMAP=de_CH-latin1
1. Create the hostname file
   > nano /etc/hostname
   * *pc-name*
1. Set the network connection
   > nano /etc/hosts
   * 127.0.0.1 localhost
   * ::1 localhost
   * 127.0.1.1 *pc-name*.localdomain *pc-name*
1. Enable dhcpcd
   > pacman -S dhcpcd\
   > systemctl enable dhcpcd
1. Set the root password
   > passwd
1. Install the bootloader
   > pacman –S grub os-prober\
   > grub-install /dev/sda\
   > grub-mkconfig -o /boot/grub/grub.cfg
1. Reboot the system
   > exit\
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
### Set the localization in Gnome
   The localization in Gnome is not the same as set through vconsole.
### Install Gnome extensions if wished
   * Install dash to panel gnome extension
   * Install applications menu
   * Install desktop-icons
### Install the Gnome tweak tool to modify appearance
   > pacman -S gnome-tweak-tool\
   > gnome-tweak-tool\
   > gnome-tweaks (to start as a user other than root)

## Install VMware tools
   * start the installation through vmware
   > for x in {0..6}; do mkdir -p /etc/init.d/rc${x}.d; done\
   > mount /dev/cdrom /mnt\
   > tar xf /mnt/VMwareTools*.tar.gz -C /root\
   > perl /root/vmware-tools-distrib/vmware-install.pl\
   > nano /etc/systemd/system/vmwaretools.service\
       > [Unit]\
       > Description=VMWare Tools daemon\

       > [Service]\
       > ExecStart=/etc/init.d/vmware-tools start\
       > ExecStop=/etc/init.d/vmware-tools stop\
       > PIDFile=/var/lock/subsys/vmware\
       > TimeoutSec=0\
       > RemainAfterExit=yes\
 
       > [Install]\
       > WantedBy=multi-user.target
   > systemctl enable vmwaretools.service
   > reboot

## [OPTIONAL] Start the bluetooth service
> systemctl start bluetooth.service\
> systemctl enable bluetooth.service

BUGFIX [NOT REQUIRED ANYMORE] two pulseaudio instances started with Gnome
> mkdir -p  /var/lib/gdm/.config/systemd/user\
> ln -s /dev/null  /var/lib/gdm/.config/systemd/user/pulseaudio.socket

## Update the mirror list
If the system did not update, make sure the mirrorlist is valid.

   > pacman -S reflector\
   > reflector --verbose -l 50 -p http -p https --country Switzerland --sort rate --save /etc/pacman.d/mirrorlist
