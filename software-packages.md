# Pacman packages
Name | Description | Essential
--- | --- | ---
remmina | remote desktop client
pavucontrol | control gui for pulse audio
jupyterlab | interactive programming environment for python. To run it, type jupyter lab
firefox | webbrowser | :heavy_check_mark:
ntfs-3g | to read and write NTFS HD's (Windows)  | :heavy_check_mark:
libreoffice-still | opensource office | :heavy_check_mark:
pinta  | similar to Microsoft paint | :heavy_check_mark:
exfat-utils
gnome-disk-utility
texmaker
texlive-most

# AUR packages
* balena-etcher
  * install AUR dependency https://aur.archlinux.org/electron3-bin.git
  * install https://aur.archlinux.org/balena-etcher.git

# Install steam
* Enabling multilib (used to run wine and steam)
  > sudo nano /etc/pacman.conf
	* uncomment the [multilib] section
* Update the system
  > pacman -Syu
* Install steam
  > sudo pacman -S steam

# Fonts
* Install the new font
  > pacman -S ttf-roboto
* Reload the cach (as user, not root)
  > fc-cache
