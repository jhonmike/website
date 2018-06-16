# Syslinux

Lembrando que o syslinux deve ser montado direto na partição `/boot`,
porque quando o syslinux for ativar o linux ele precisa ler os arquivos `/boot/vmlinuz-linux` e
`initramfs-linux.img` que tem que estar disponivel antes de subir a partição principal do linux
que carrega simultanio quando usamos UEFI

```shell
pacman -S syslinux efibootmgr
mkdir -p /boot/EFI/boot
cp -r /usr/lib/syslinux/efi64/* /boot/EFI/boot/
efibootmgr -c -g -d /dev/sda -p 1 -w -L boot -l '\\EFI\\boot\\bootx64.efi'
```

e crio o arquivo `/boot/EFI/boot/syslinux.cfg` com o seguinte conteudo:

```
PROMPT 0
DEFAULT arch

LABEL arch
  LINUX ../../vmlinuz-linux
  APPEND root=/dev/sda2 rw
  INITRD ../../initramfs-linux.img
```
Caso tenha a partição cryptografada use o seguinte conteudo no APPEND:

`APPEND root=/dev/mapper/cryptroot cryptdevice=/dev/sda2:cryptroot rw`
