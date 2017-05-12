## Definiremos o layout do nosso teclado:

```shell
loadkeys br-abnt2
```

## Vamos alterar o idioma da instalação:

```shell
nano /etc/locale.gen
```

Aperte “Ctrl + W” para abrir um campo de pesquisa, digite “pt_BR” e dê Enter. Estará na linha exata para alteração: #pt_BR.UTF-8 UTF-8.

Agora remova somente o # da respectiva linha, dê um “Ctrl + O”  e Enter para salvar e depois um “Ctrl + X” para sair do arquivo.

Após o procedimento de alteração:

# locale-gen && export LANG=pt_BR.UTF-8


Passo 5: Lembra quando reservamos o espaço no HD com o Windows? Então, vamos organizá-lo agora!

# cfdisk


Listará todas as partições do seu HD. Ficará em destaque o Espaço livre que reservamos.

Nota: Use as setas cima e baixo para navegar nas partições do cfdisk. Direita e esquerda para setar as opções.

Selecione Espaço Livre  e dê  Enter na opção Nova. Pedirá a confirmação do tamanho da partição, confirme sem alterá-la.  Surgirá outra opção, entre primária e estendida, selecione a segunda.

Irá transferir o Espaço Livre, virando uma subpartição.  Como já citado, reservamos os 47GB e partindo disso, criaremos apenas nossa raiz  e a memória swap.  Dê um Enter novamente em Nova, agora altere o tamanho da partição. Nesse exemplo, utilizaremos 43G para raiz e 4G para swap.

 

Raiz:

 

Tamanho da partição: 43G

Ainda com a partição selecionada, vá na opção  “Iniciali.” e dê um Enter. Assim, essa partição se tornará  inicializável.

Por padrão do cfdisk, todas as partições criadas, tornam-se do tipo 83 Linux. Prosseguiremos sem alterações.

Swap:

Tamanho da partição: 4G

Nessa partição, teremos que alterar a tipagem.  Selecione a opção Tipo  e escolha a opção  82 Linux swap / Solaris.

Após isso, vá na opção Gravar e confirme. Faça esse procedimento nas novas partições criadas. É recomendável a repetição da ação “Gravar” também nas particoes do Windows.

 

Passo 6: Configuraremos a nossa conexão com a internet para seguir com a instalação. Exemplificaremos a configuração do Wireless. Para conexões cabeadas, recomendo a leitura  desse tópico. Geralmente esse reconhecimento  é automático.

Prosseguindo, precisamos saber o nome da nossa interface Wireless:

# iwconfig

No meu caso, a interfase é wlp3s0.

Após, digite:

# wifi-menu wlp3s0

Agora vamos fazer o teste de conexão:

# ping -c3 google.com

Pronto! Linguagem, particionamento e conexão configurados!

 

Passo 7: Formatando as partições do Arch Linux:

Procure o Device da partição do Linux e Swap, pois será a referência para formatação, com o comando:

# fdisk -l

 

Após, seguiremos com a formatação:

 

## Montando as partições

### /
Endereço que consta no seu fdisk -l.

```shell
mkfs.ext4 /dev/sda3
mount /dev/sda3 /mnt
```

### Swap

```shell
mkswap /dev/sda2
swapon /dev/sda2
```

### UEFI

```shell
mkfs.fat -F32 /dev/sda1
mkdir /mnt/boot
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

### Home

```shell
mkdir /mnt/home
mount /dev/sda7 /mnt/home
```

Agora configuraremos os repositórios do Arch Linux:

# nano /etc/pacman.d/mirrorlist

Pressione “Ctrl + W” para pesquisarmos os repositórios. Digite BRAZIL e dê um Enter.

Recorte e cole no topo da lista, as linhas com os endereços dos servidores brasileiros com o “Ctrl+K” e “Ctrl+U” respectivamente.

Dê um  “Ctrl+O” para salvar e “Ctrl+X” para fechar o arquivo.

Na figura 4, exemplifica esse processo.

 
aa
Figura 4. Meu mirrrorlist. (Pós-instalação)

Depois, instalaremos o sistema base:

# pacstrap -i /mnt base base-devel

Fstab:

# genfstab -U -p /mnt >> /mnt/etc/fstab

Passo 8: Iremos configurar o sistema base na pasta /mnt com o chroot. E ajustaremos algumas coisas básicas do sistema.

# arch-chroot /mnt /bin/bash

 

Idioma do Arch Linux:

# nano /etc/locale.gen 

 

Nota: O processo do arquivo locale.gen é mesmo do início da instalação.

Carregue a alteração do arquivo locale.gen, digite:

# locale-gen

 

Em seguida, execute:

# echo LANG=pt_BR.UTF-8 > /etc/locale.conf

# export LANG=pt_BR.UTF-8

 

Fuso Horário:

# ln -s /usr/share/zoneinfo/America/Boa_Vista /etc/localtime

 

Troque Boa_Vista por sua cidade. Caso queira listar as localidades:

# ls /usr/share/zoneinfo

 

Relógio do Hardware:

# hwclock --systohc --utc

 

Hostname:

# echo ArchLinux > /etc/hostname

 

Troque o nome ArchLinux por outro de sua preferência.

Instalação das ferramentas Wireless no sistema base do Arch Linux:

# pacman -S wireless_tools wpa_supplicant wpa_actiond dialog

 

Ambiente inicial:

mkinitcpio -p linux

Senha do root:

# passwd

 

Informe sua senha, confirme e dê um Enter.

Habilitar multilib

# nano /etc/pacman.conf 

 

No arquivo procure as linhas:

#[multilib]
#Include = /etc/pacman.d/mirrorlist

 

Retire o # de ambas.

Após essas alterações, atualize o sistema:

# pacman -Syu

 

Nesse guia, utilizaremos o Grub como o gerenciador de boot, então:

# pacman -S grub

 

Instalando o Grub.

# grub-install --target=i386-pc --recheck /dev/sda

 

Nota: Atenção! A parte /dev/sda não leva números.

Para que o Grub reconheça o seu Windows, é necessário a instalação do pacote os-prober, então digite:

# pacman -S os-prober

 

Finalizar o Grub

# grub-mkconfig -o /boot/grub/grub.cfg

 

Criar usuário padrão:

# useradd -m -g users -G wheel -s /bin/bash seu-usuario

 

Senha:

# passwd seu-usuario

 

Caso queira excluir:

# userdel -r seu-usuario

 

Adicionar permissões do sistema ao usuário:

 
 # gpasswd -a seu-usuario locate
 # gpasswd -a seu-usuario users
 # gpasswd -a seu-usuario audio
 # gpasswd -a seu-usuario video
 # gpasswd -a seu-usuario daemon
 # gpasswd -a seu-usuario dbus
 # gpasswd -a seu-usuario disk
 # gpasswd -a seu-usuario games
 # gpasswd -a seu-usuario rfkill
 # gpasswd -a seu-usuario lp
 # gpasswd -a seu-usuario network
 # gpasswd -a seu-usuario optical
 # gpasswd -a seu-usuario power
 # gpasswd -a seu-usuario scanner
 # gpasswd -a seu-usuario storage
 

Instalação das fontes para tornar o ambiente mais agradável:

# pacman -S $(pacman -Ss ttf | grep -v ^” ” | awk ‘{print $1}’) && fc-cache

 

Instalação do monitor para a bateria do notebook:

# pacman -S acpi acpid

 

Agora vamos criar uma inicialização automática do  acpid no  sistema:

# systemctl enable acpid.service

Passo 9: A partir daqui, iremos providenciar a instalação do Xorg e o Driver de vídeo Intel.

Xorg

# pacman -S xorg-xinit xorg-utils xorg-server

Intel

# pacman -S xf86-video-intel mesa mesa-demos 

NVIDIA

Mostrar os controladores compatíveis VGA:

# lspci | grep VGA

Instalação dos drivers:

# pacman -S intel-dri xf86-video-intel bumblebee nvidia

Instalação do bbswitch:

# pacman -S bbswitch

Instalação das libs 32bits (Caso seu Arch seja da arquitetura 86_x64, configurar o multilib no arquivo /etc/pacman.conf) e demais pacotes:

# pacman -S lib32-nvidia-utils
# pacman -S lib32-intel-dri
# pacman -S opencl-nvidia
# pacman -S lib32-virtualgl

Adicionar o nosso usuário ao grupo Bumblebee:

# gpasswd -a nomeDoUsuario bumblebee

Verificar e ativar o serviço do Bumblebee:

# systemctl status bumblebeed
# systemctl enable bumblebeed
# systemctl start bumblebeed

Testar elemento gráfico do pacote opencl:

# glxspheres64

Testar elemento gráfico do pacote opencl utilizando a placa dedicada Nvidia:

# optirun glxspheres64

DICA: Para testar se o bbswitch está ativo:

$ cat /proc/acpi/bbswitch

DICA DE EXECUÇÃO: Para executar alguma aplicação com o uso da placa gráfica NVIDIA:

# optirun nomeAplicacao
 

Passo 10: Nesse ponto, iremos providenciar os gerenciadores: touchpad, mouse e teclado, respectivamente.

# pacman -S xf86-input-synaptics xf86-input-mouse xf86-input-keyboard

Configuração do sudo:

Aqui iremos acrescentar privilégios para o usuário padrão que criamos anteriormente. Demostraremos duas sugestões:

Entre com o nano para editar o arquivo /etc/sudoers:

# nano /etc/sudoers

 

Pressione “Ctrl+W” digite: root ALL=(ALL) ALL e dê um Enter. Sem alterá-la, acrescente na linha de abaixo a regra de privilégios que lhe convir:

    Usuário padrão com todos os privilégios do root:

seu-usuario ALL=(ALL) ALL

    Usuário padrão com privilégio apenas para execução do pacman:

seu-usuario ALL=(ALL) NOPASSWD:/usr/bin/pacman

Dê um “Ctrl+O” para salvar e “Ctrl+X” para sair do arquivo.

Passo 11: Agora iremos habilitar o yaourt para usarmos os pacotes do AUR. Iremos novamente ao diretório, usando o nano:

# nano /etc/pacman.conf

No final de todo arquivo, acrescente as três linhas abaixo:

[archlinuxfr]
SigLevel =  Never
Server = http://repo.archlinux.fr/$arch

Salve com o “Ctrl+O” e feche com “Ctrl+X”.

Após acrescentarmos as linhas no arquivo pacman.conf, finalize com a instalação do Yaourt:

# pacman -Sy yaourt

Pronto! Finalizamos a instalação e configuração básica do sistema.

Saia do chroot:

# exit 

 

Desmonte tudo relacionado a pasta /mnt e reinicie o sistema:

# umount -R /mnt && reboot

Passo 12: A partir desse ponto, iremos trabalhar a instalação do ambiente gráfico do sistema.

Primeiramente, faça o login com seu-usuario e em seguida como root, com o comando:

# su

Em seguida, conecte a sua rede para que possamos baixar os pacotes das interfaces pretendidas:

Troque o wlp3s0 por sua interface

# wifi-menu wlp3s0 

Install the i3 package group. It includes the window manager i3-wm, i3status which writes a status line to i3bar through stdout, and i3lock, a screen locker.

Additional packages are available in the Arch User Repository. See the section #Patches for examples.
Display manager

i3-wm includes i3.desktop as Xsession which starts the window manager. i3-with-shmlog.desktop enables logs (useful for debugging).
xinitrc

Edit Xinitrc, and add:

exec i3

To log the output from i3, add this line instead:

exec i3 -V >> ~/i3log-$(date +'%F-%k-%M-%S') 2>&1

usaremos o gdm:

# pacman -S gdm

Ativar a inicialização junto com o sistema:

# systemctl enable gdm.service
# systemctl start gdm.service

