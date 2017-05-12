### Virtual Host Example

```
<VirtualHost *:80>
    ServerName jhonmike.com.br
    ServerAlias www.jhonmike.com.br

    ServerAdmin developer@jhonmike.com.br
    DocumentRoot /var/www/jhonmike/public
    <Directory /var/www/jhonmike/public>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

### Ativando Virtual Host

```bash
a2ensite /etc/apache2/sites-available/jhonmike.com.br.conf
```

### ModRewrite

Para habilitar o módulo no Apache basta esta linha:

```bash
a2enmod rewrite
```

Agora abra o arquivo de configuração:

```bash
vim  /etc/apache2/sites-available/default
```

Procure no seu arquivo a entrada "AllowOverride None"
Altere esse valor para "AllowOverride All" .
Salve o arquivo e reinicie o Apache.

```bash
/etc/init.d/apache2 restart
or
service apache2 restart
```
