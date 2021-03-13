# Command list

## Keyboard Shortcuts
Command | Description
---|---
CTRL + h | show hidden files

## General
Command | Description
---|---
rm -f *file* | remove file
rm -rf *folder* | remove folder

## Pacman
Command | Description
---|---
pacman -Q | list all installed packages
pacman -Syu | system update, sometimes needs restart, always before installing other packages
pactree *package* | list the dependency tree of a package
pacman -Rscn *package* | remove a package with all its dependencies
pacman -Qdtq | list all unnecessary packages
pacman -R $(pacman -Qdtq) | remove all unnecessary packages

## Install AUR packages
The user needs to be added to the sudoer file in order for makepkg to use pacman.
* install AUR package (as user, not root)
  > git clone *package-link.git*\
  > cd *package-name*\
  > makepkg -si
* then delete the git-package
* if the key is not trusted yet
  > gpg --recv-key *key_number*

## Add user to sudoer file
> pacman -S sudo\
> EDITOR=nano visudo
* add the user under root in the section "User privilege specification"

## Python
Command | Description
---|---
python -m venv *name* | create a viritual python environment in the home directory of the logged in user
source /home/*user*/*name*/bin/activate | activate the environment
deactivate | deactivate the environment from inside, then the folders could be deleted

## Graphic cards
Command | Description
---|---
lspci \| grep -E "VGA\|3D" | list graphic cards
lspci -vnn \| grep VGA -A 12 | show graphic card details
