# Docker

## Installing

```
sudo pacman -S docker
newgrp docker
sudo gpasswd -a jhon docker
systemctl start docker
```

## Pull Image

Ex.:
```
docker pull postgres:9.5.7
```

## Start Container

Ex.:
```
docker run -i -p 5432:5432 --name postgres -e POSTGRES_DB=xpto_db -d postgres:9.5.7
```
