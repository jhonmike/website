# Instalação

```bash
sudo pacman -S nvidia nvidia-libgl xorg-xrandr
```

# Config

```bash
lspci | grep -E "VGA|3D"
```

/etc/X11/xorg.conf

```
Section "Module"
    Load "modesetting"
EndSection

Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "PCI:8.0.0"
    Option "AllowEmptyInitialConfiguration"
EndSection
```

~/.xinitrc
```
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
