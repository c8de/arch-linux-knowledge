# Arch install VM (VMware)

1. Download the ISO file
1. Setup a new VM in VMware
1. Add the following line to the vm config file (.vmx)
   * firmware = “efi”
1. Install as described in arch-install.md without installing the network manager. Instead install dhcpcd:
   > pacman -S dhcpcd\
   > systemctl enable dhcpcd

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
