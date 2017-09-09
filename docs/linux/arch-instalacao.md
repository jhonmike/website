### Definiremos o layout do nosso teclado:

```shell
loadkeys br-abnt2
```

### Vamos alterar o idioma da instalação:

descomente a linha com `#pt_BR.UTF-8` do arquivo `/etc/locale.gen`
Após o procedimento de alteração:
```shell
locale-gen && export LANG=pt_BR.UTF-8
```

### Particionando o HD para modo UEFI

```
/dev/sda1 /boot/efi type: EFI 512M
/dev/sda2           type: swap 4G
/dev/sda3 /         type: ext4 full
/dev/sda4 /home     type: ext4 full (opcional)
```

### Acessando a rede wifi

```shell
wifi-menu

ping -c3 google.com
```

### Formatação

```shell
fdisk -l
```

#### Montando as partições sem crypt

##### /
Endereço que consta no seu fdisk -l

```shell
mkfs.ext4 /dev/sda3
mount /dev/sda3 /mnt
```

##### Swap

```shell
mkswap /dev/sda2
swapon /dev/sda2
```

##### UEFI

```shell
mkfs.fat -F32 /dev/sda1
mkdir /mnt/boot
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

##### Home (opcional)

```shell
mkdir /mnt/home
mount /dev/sda4 /mnt/home
```
#### Montando as partições com crypt

##### /
Endereço que consta no seu fdisk -l

```shell
cryptsetup -y -v luksFormat /dev/sda3
cryptsetup open /dev/sda3 cryptroot
mkfs.ext4 /dev/mapper/cryptroot
mount /dev/mapper/cryptroot /mnt
```

##### Swap

```shell
mkswap /dev/sda2
swapon /dev/sda2
```

##### UEFI

```shell
mkfs.fat -F32 /dev/sda1
mkdir /mnt/boot
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

### Agora configuraremos os repositórios do Arch Linux:

No arquivo `/etc/pacman.d/mirrorlist` esta a ordem de prioridade dos links que seram utilizados na busca de pacotes
se desejar altere a ordem colocando os links do BRAZIL no topo da lista.

Em seguida instale a base do arch
```shell
pacstrap -i /mnt base base-devel
```

### Fstab

O Fstab ira salvar gerar as configs de montagem das partições de seu hd/ssd

```shell
genfstab -U -p /mnt >> /mnt/etc/fstab
```

### Iniciando root
```shell
arch-chroot /mnt /bin/bash
```

#### Configurações basicas do seu ArchLinux

Idioma do Arch Linux, descomente a linha com `pt_BR` do arquivo `/etc/locale.gen` e rode `locale-gen`
Em seguida, execute:
```shell
echo LANG=pt_BR.UTF-8 > /etc/locale.conf && export LANG=pt_BR.UTF-8
```
Fuso Horário:
```shell
ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
```

Relógio do Hardware:
```shell
hwclock --systohc --utc
```

Hostname:
```shell
echo ArchLinux > /etc/hostname
```

#### Instalação das ferramentas Wireless no sistema base do Arch Linux:

```shell
pacman -S wireless_tools wpa_supplicant wpa_actiond dialog
```

#### Ambiente inicial:

```shell
mkinitcpio -p linux
```

#### Senha do root:
```
passwd
```
Informe sua senha, confirme e dê um Enter.

### Habilitar multilib
```shell
vim /etc/pacman.conf
```

No arquivo procure as linhas:
```
#[multilib]
#Include = /etc/pacman.d/mirrorlist
```

Retire o # de ambas e atualize o pacman
```shell
pacman -Syu
```

### GRUB

Nesse guia, utilizaremos o Grub como o gerenciador de boot, então:
```shell
pacman -S grub grub-efi-x86_64 efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

### Criar usuário padrão:
```shell
useradd -m -g users -G wheel -s /bin/bash seu-usuario
passwd seu-usuario
```

Adicionar permissões do sistema ao usuário:

```shell
gpasswd -a seu-usuario locate
gpasswd -a seu-usuario users
gpasswd -a seu-usuario audio
gpasswd -a seu-usuario video
gpasswd -a seu-usuario daemon
gpasswd -a seu-usuario dbus
gpasswd -a seu-usuario disk
gpasswd -a seu-usuario games
gpasswd -a seu-usuario rfkill
gpasswd -a seu-usuario lp
gpasswd -a seu-usuario network
gpasswd -a seu-usuario optical
gpasswd -a seu-usuario power
gpasswd -a seu-usuario scanner
gpasswd -a seu-usuario storage
```

### Xorg
```shell
pacman -S xorg-xinit xorg-server
```

#### Intel
```shell
pacman -S xf86-video-intel mesa mesa-demos 
```

#### NVIDIA

Mostrar os controladores compatíveis VGA:
```shell
lspci | grep VGA
```

Instalação dos drivers:
```shell
pacman -S intel-dri xf86-video-intel bumblebee nvidia
```

Instalação do bbswitch:
```shell
pacman -S bbswitch
```
Instalação das libs 32bits (Caso seu Arch seja da arquitetura 86_x64, configurar o multilib no arquivo /etc/pacman.conf) e demais pacotes:
```shell
pacman -S lib32-nvidia-utils
pacman -S lib32-intel-dri
pacman -S opencl-nvidia
pacman -S lib32-virtualgl
```
Adicionar o nosso usuário ao grupo Bumblebee:
```shell
gpasswd -a nomeDoUsuario bumblebee
```
Verificar e ativar o serviço do Bumblebee:
```shell
systemctl status bumblebeed
systemctl enable bumblebeed
systemctl start bumblebeed
```
Testar elemento gráfico do pacote opencl:
```shell
glxspheres64
```
Testar elemento gráfico do pacote opencl utilizando a placa dedicada Nvidia:
```shell
optirun glxspheres64
```
DICA: Para testar se o bbswitch está ativo:
```shell
cat /proc/acpi/bbswitch
```
DICA DE EXECUÇÃO: Para executar alguma aplicação com o uso da placa gráfica NVIDIA:
```shell
optirun nomeAplicacao
```

#### ATI

```shell
pacman -S xf86-video-ati
```

### touchpad, mouse e teclado, respectivamente.
```shell
pacman -S xf86-input-synaptics xf86-input-mouse xf86-input-keyboard
```

### Configuração do sudo:

Editar o arquivo `/etc/sudoers` como abaixo:
```shell
root ALL=(ALL) ALL
seu-usuario ALL=(ALL) ALL
```

### Pacotes do AUR

No arquivo `/etc/pacman.conf` adicione as seguintes linhas no final do arquivo:
```shell
[archlinuxfr]
SigLevel =  Never
Server = http://repo.archlinux.fr/$arch
```
e execute:
```shell
pacman -Sy yaourt
```

### Pronto!
Finalizamos a instalação e configuração básica do sistema.

Saia do chroot:

```shell
exit
```

Desmonte tudo relacionado a pasta /mnt e reinicie o sistema:
```shell
umount -R /mnt && reboot
```
