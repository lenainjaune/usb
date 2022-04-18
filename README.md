# usb
all usb relative

## Linux

### Multiboot
Just install it and copy ISOs and wait that finish with **sync** : https://www.ventoy.net/en/doc_start.html

Nota : I had a trouble when I tried to mount from thunar "Unable to mount XX GB Filesystem
Error mounting: mount: unknown filesystem type 'exfat'" => https://www.skyminds.net/ubuntu-resoudre-lerreur-mount-unknown-filesystem-type-exfat/

TIPS : certain ISO need Memdisk mode (see : https://lecrabeinfo.net/creer-une-cle-usb-multi-boot-avec-ventoy.html#mode-memdisk)

### Windows (7/10) installer from USB
#### woeusb
Avec woeusb (GUI)

Nota : sous woeusb l'indicateur de progression est sommaire et l'opération peut prendre du temps entre autre quand est affiché : "Installing GRUB bootloader for legacy PC booting" => laisser se terminer jusqu'au message "Installation réussie!"
#### Bash
TODO : brouillon bien vérifier que tout fonctionne comme attendu !
[Source](https://thornelabs.net/posts/create-a-bootable-windows-7-or-10-usb-drive-in-linux.html) (CLI)
```sh
# Créer une clé USB d'installation de Windows 7 ou 10 depuis Linux Debian Bullseye 11
# https://thornelabs.net/posts/create-a-bootable-windows-7-or-10-usb-drive-in-linux.html

root@vm-bullseye-xfce:~# apt update && apt install ntfs-3g gcc make gettext

# Install ms-sys (http://ms-sys.sourceforge.net/#Download prendre la version la plus à jour ; ici c'est la version ms-sys-2.7.0)
#  Nota : j'ai d'abord téléchargé avec Firefox pour avoir le lien de téléchargement car sourceforge empêche les liens directs
#
# Attention : adapter la version dans les commandes suivantes ...
root@vm-bullseye-xfce:~# wget https://freefr.dl.sourceforge.net/project/ms-sys/ms-sys%20development/2.7.0/ms-sys-2.7.0.tar.gz
...
2022-04-18 19:57:43 (829 KB/s) — « ms-sys-2.7.0.tar.gz » sauvegardé [85195/85195]
root@vm-bullseye-xfce:~# tar xvzf ms-sys-2.7.0.tar.gz
root@vm-bullseye-xfce:~# cd ms-sys-2.7.0/
root@vm-bullseye-xfce:~/ms-sys-2.7.0# make
#
# On peut l'installer dans le système dans /usr/local/bin (optionnel) ...
#
root@vm-bullseye-xfce:~/ms-sys-2.7.0# #make# install
root@vm-bullseye-xfce:~/ms-sys-2.7.0# #./build/bin/ms-sys# -v
ms-sys version 2.7.0
Écrit par Henrik Carlqvist

Copyright (C) 2009-2020 Free Software Foundation, Inc.
Ceci est un Logiciel Libre ; veuillez consulter le code source pour les conditions 
générales de copie. Ce logiciel est fourni SANS AUCUNE GARANTIE ; incluant
la COMMERCIALISABILITÉ ou L’ADÉQUATION À UN OBJECTIF PARTICULIER.
root@vm-bullseye-xfce:~/ms-sys-2.7.0# #ms-sys# -v
# affiche la version
# Nota : si ms-sys -v ne fonctionne pas, il faut s'assurer que PATH contient bien /usr/local/bin
#
# ... ou PAS !
#
/data/noinstall/ms-sys/ms-sys-2.7.0/build/bin/ms-sys -v
# affiche la version

root@vm-bullseye-xfce:~# dmesg -w
...
/dev/sdb détecté une fois branché
root@vm-bullseye-xfce:~# lsblk   
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk 
├─sda1   8:1    0   19G  0 part /
├─sda2   8:2    0    1K  0 part 
└─sda5   8:5    0  975M  0 part [SWAP]
sdb      8:16   0   16G  0 disk 
root@vm-bullseye-xfce:~# DEV=/dev/sdb
root@vm-bullseye-xfce:~# parted -a cylinder $DEV -s "mklabel msdos" -s "mkpart primary ntfs 1 -1" -s "set 1 boot on"
root@vm-bullseye-xfce:~# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
...
sdb      8:16   0   16G  0 disk 
└─sdb1   8:17   0   16G  0 part 
root@vm-bullseye-xfce:~# mkfs.ntfs -f ${DEV}1
Cluster size has been automatically set to 4096 bytes.
Creating NTFS volume structures.
mkntfs completed successfully. Have a nice day.
root@vm-bullseye-xfce:~# /root/ms-sys-2.7.0/build/bin/ms-sys -7 ${DEV}
Master Boot Record de type Windows 7 écrit avec succès sur /dev/sdb
# Certaines machines auront besoin de la ligne suivante (EBR) :
root@vm-bullseye-xfce:~# /root/ms-sys-2.7.0/build/bin/ms-sys -n ${DEV}1
Secteur de démarrage NTFS Windows 7 écrit avec succès sur /dev/sdb1
root@vm-bullseye-xfce:~# mkdir -p /mnt/usb /mnt/iso
root@vm-bullseye-xfce:~# mount ${DEV}1 /mnt/usb/
root@vm-bullseye-xfce:~# mount -o loop fr_windows_7_aio_with_sp1_x64.iso /mnt/iso/
root@vm-bullseye-xfce:~# ls /mnt/iso/
autorun.inf  boot  bootmgr  bootmgr.efi  efi  setup.exe  sources  support  upgrade
root@vm-bullseye-xfce:~# rsync -aP /mnt/iso/ /mnt/usb/
root@vm-bullseye-xfce:~# umount -l /mnt/iso /mnt/usb
root@vm-bullseye-xfce:~# rmdir /mnt/iso /mnt/usb


=> installation Windows 7 Intégral (ultimate) SP1 depuis fr_windows_7_aio_with_sp1_x64.iso testée avec succès en vUSB !
```

### Santé d'une clé
https://www.cyberciti.biz/faq/linux-check-the-physical-health-of-a-usb-stick-flash-drive/
