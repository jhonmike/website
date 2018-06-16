# GRUB

Nesse guia, utilizaremos o Grub como o gerenciador de boot, então:
```shell
pacman -S grub grub-efi-x86_64 efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=boot --recheck
grub-mkconfig -o /boot/grub/grub.cfg
mv boot/grubx64.efi boot/bootx64.efi
efibootmgr -c -g -d /dev/sda -p 1 -w -L boot -l '\\EFI\\boot\\bootx64.efi'
efibootmgr -n IDBOOT
```

## Encryption com GRUB

```
/etc/mkinitcpio.conf
HOOKS=(base udev autodetect keyboard keymap consolefont modconf block encrypt lvm2 filesystems fsck)
```

```
/etc/default/grub
GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda2:cryptroot"
GRUB_ENABLE_CRYPTODISK=y
```

```
mkinitcpio -p linux
```
