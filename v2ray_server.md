# install core
Update
```console
apt update && apt upgrade -y
```	

```console
curl https://raw.githubusercontent.com/SonyaCore/V2RayGen/main/V2RayGen.py | sudo python3 - --vmess
```	

Or:

# 1 firewall settings
```console
sudo ufw status
```

# 2
```console
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw allow ????/tcp
```

# enable firewall
```console
sudo ufw enable
```
# firewall status
```console
sudo ufw status
```

# 3
```console
sudo apt-get update -y 
sudo apt-get upgrade -y
sudo apt install curl -y
```

# 4 X-UI (https://github.com/hossinasaadi/x-ui)
```console
bash <(curl -Ls https://raw.githubusercontent.com/hossinasaadi/x-ui/master/install.sh)
```	

# 5 install certbot
```console
sudo apt install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get install certbot
```


# 6  Use certbot to get SSL Certificate.
```console
sudo certbot certonly --standalone --preferred-challenges http --agree-tos --email <your-email-address> -d <test.example.com>
```

# Note both paths
/etc/letsencrypt/live/test.example.com/fullchain.pem
/etc/letsencrypt/live/test.example.com/privkey.pem

# 7 renew ssl certificate
```console
certbot renew --force-renewal
```

# 8 Install Google BBR using BBR script
```console
wget -N --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && bash bbr.sh
```
