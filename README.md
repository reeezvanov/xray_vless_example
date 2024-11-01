## Overview
---
This is an example from official [Project X setup guide](https://xtls.github.io/ru/document/level-0/) wrapped into the Docker Compose file.

## Requirements
---
You need to have working and configured VPS with domain connected to it

## Instalation
---
#### Generate TLS certificates
```bash
mkdir -p ./volume/xray/cert
wget -O -  https://get.acme.sh | sh
. .bashrc
acme.sh --upgrade --auto-upgrade
acme.sh --issue --server letsencrypt --test -d <yourdomaincom> -w ./volume/nginx/www --keylength ec-256
acme.sh --set-default-ca --server letsencrypt
acme.sh --issue -d <yourdomaincom> -w /volume/nginx/www --keylength ec-256 --force
acme.sh --installcert -d <yourdomaincom> --cert-file ./volume/xray/cert/cert.crt --key-file ./volume/xray/cert/private.key --fullchain-file ./volume/xray/cert/fullchain.crt --ecc
```

#### Replace vars in configs 
Replace "variables" __nginx.conf__ and __config.json__
- __<yourdomaincom>__ is your domain name connected to VPS/VDS
- __<client_uuid>__ generated UUID for client (you can generate it [here](https://www.uuidgenerator.net/))

#### Permissions
```bash
sudo chown user ./volume/nginx/
sudo chown user ./volume/xray/
sudo chown user -R /var/log/nginx/
sudo chown user -R /var/log/xray/
```

#### Run
```bash
sudo docker-compose up --force-recreate
```
## XRAY Clients
---
XRAY GUI client for your platform can be found [here](https://github.com/XTLS/Xray-core?tab=readme-ov-file#gui-clients)
