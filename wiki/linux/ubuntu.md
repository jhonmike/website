# Alterar Usuário do arquivo

```bash
sudo chown root:root [file]
```

# Atalhos Desktop

Pasta adicionar os .desktop na pasta `/usr/share/applications/`

Example de arquivo

```
[Desktop Entry]
Name=Steam
Comment=Application for managing and playing games on Steam
Exec=/usr/bin/steam %U
Icon=steam
Terminal=false
Type=Application
Categories=Network;FileTransfer;Game;
MimeType=x-scheme-handler/steam;
Actions=Store;Community;Library;Servers;Screenshots;News;Settings;BigPicture;Friends;
```

# Pesquisar por processos existentes

    ps -ef | grep [pesquisa]

# Matar processos

    kill [idProcesso^]

# Para gravar ISO em pendrive:

```bash
dd if=/local/da/imagem.iso of=/dev/sd*
```

# Preferencia IPV4

```bash
sudo sh -c "echo 'precedence ::ffff:0:0/96 100' >> /etc/gai.conf"
```
