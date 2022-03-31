# usb
all usb relative

## Linux

### Multiboot
Just install it and copy ISOs and wait that finish with **sync** : https://www.ventoy.net/en/doc_start.html

Nota : I had a trouble when I tried to mount from thunar "Unable to mount XX GB Filesystem
Error mounting: mount: unknown filesystem type 'exfat'" => https://www.skyminds.net/ubuntu-resoudre-lerreur-mount-unknown-filesystem-type-exfat/

TIPS : certain ISO need Memdisk mode (see : https://lecrabeinfo.net/creer-une-cle-usb-multi-boot-avec-ventoy.html#mode-memdisk)

### Windows (7/10) installer from USB
Avec woeusb (GUI) ou [cette piste plus automatisable](https://thornelabs.net/posts/create-a-bootable-windows-7-or-10-usb-drive-in-linux.html) (CLI ; non testée)

Nota : sous woeusb l'indicateur de progression est sommaire et l'opération peut prendre du temps entre autre quand est affiché : "Installing GRUB bootloader for legacy PC booting" => laisser se terminer jusqu'au message "Installation réussie!"


### Santé d'une clé
https://www.cyberciti.biz/faq/linux-check-the-physical-health-of-a-usb-stick-flash-drive/
