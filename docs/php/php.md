## PHP Arch

```bash
sudo pacman -S php php-gd php-intl php-pgsql php-mcrypt php-imap php-sqlite php-memcache
```

## DateTime

```php
new \DateTime();
```

### Add dias no objeto

```php
$date->add(new \DateInterval('P1D'));
```
