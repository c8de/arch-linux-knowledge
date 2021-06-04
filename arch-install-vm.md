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
   > pacman -S open-vm-tools\
   > systemctl start vmtoolsd\
   > systemctl enable vmtoolsd
### If fullscreen does not work
   > cp /etc/vmware-tools/tools.conf.example /etc/vmware-tools/tools.conf\
   > nano /etc/vmware-tools/tools.conf\
   > Remove the # from this block:
   
    [resolutionKMS]

    # Default is true if tools finds an xf86-video-vmware driver with
    # version >= 13.2.0. If you don't have X installed, set this to true manually.
    # This only affects tools for Linux.
    enable=true
   > systemctl restart vmtoolsd
