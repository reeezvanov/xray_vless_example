## Overview
This is an example from official [Project X setup guide](https://xtls.github.io/ru/document/level-0/) wrapped into the Docker Compose file.

## Requirements
You need to have working and configured VPS with domain connected to it

## Instalation
#### Replace vars in configs 
Replace "variables" in __./volume/nginx_fallback/nginx_fallback.conf__, __./volume/nginx_fallback/nginx_cert.conf__ and __./volume/xray/config.json__
- __yourdomaincom__ is your domain name connected to VPS
- __client_uuid__ is generated UUID for client (you can generate it [here](https://www.uuidgenerator.net/))

#### Create needed folders
```bash
sudo mkdir -p /var/log/nginx_fallback/
sudo mkdir -p $(whoami) -R /var/log/xray/

sudo chown $(whoami) -R /var/log/nginx_fallback/
sudo chown $(whoami) -R /var/log/xray/
```

#### Generate TLS certificates
```bash
wget -O -  https://get.acme.sh | sh
~/. ~/.bashrc
acme.sh --upgrade --auto-upgrade

mkdir -p ./volume/xray/cert

sudo docker-compose up -d nginx_cert

acme.sh --issue --server letsencrypt --test -d <yourdomaincom> -w ./volume/nginx_fallback/www --keylength ec-256
acme.sh --set-default-ca --server letsencrypt
acme.sh --issue -d <yourdomaincom> -w ./volume/nginx_fallback/www --keylength ec-256 --force
acme.sh --installcert -d <yourdomaincom> --cert-file ./volume/xray/cert/cert.crt --key-file ./volume/xray/cert/private.key --fullchain-file ./volume/xray/cert/fullchain.crt --ecc

sudo docker-compose down
```

#### Set permissions
```bash
sudo chown $(whoami) ./volume/nginx_fallback/
sudo chown $(whoami) ./volume/xray/
sudo chown $(whoami) -R /var/log/nginx_fallback/
sudo chown $(whoami) -R /var/log/xray/
```

#### Run
```bash
sudo docker-compose up -d --force-recreate xray nginx_fallback
```
## XRAY Clients
XRAY GUI client for your platform can be found [here](https://github.com/XTLS/Xray-core?tab=readme-ov-file#gui-clients)
