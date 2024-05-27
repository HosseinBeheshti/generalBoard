
# OpenVPN Installation on Ubuntu 22
https://docs.onion.io/omega2-docs/boot-from-external-storage.html

## 1 
format USB stick to ext4

## 2
```console
mount /dev/sda1 /mnt/ ; tar -C /overlay -cvf - . | tar -C /mnt/ -xf - ; umount /mnt/
```
## 3 install block-mount

```console
opkg update
```

```console
opkg install block-mount
```

## 4 automatically mount
```console
block detect > /etc/config/fstab
```


```console
vi /etc/config/fstab
```

Look for the line

option  target  '/mnt/<device name>'
and change it to:

option target '/overlay'
Then, look for the line:

option  enabled '0'
and change it to

option  enabled '1'


```console
reboot
```

# OpenVPN Installation on Ubuntu 22

https://as-portal.openvpn.com/

## 1 
```console
apt update && apt -y install ca-certificates wget net-tools gnupg
```

## 2 
```console
wget https://as-repository.openvpn.net/as-repo-public.asc -qO /etc/apt/trusted.gpg.d/as-repository.asc
```

## 3 
```console
echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/as-repository.asc] http://as-repository.openvpn.net/as/debian jammy main">/etc/apt/sources.list.d/openvpn-as-repo.list
```

## 4 
```console
apt update && apt -y install openvpn-as
```

After these steps, your Access Server should be installed and awaiting further configuration.

## 1 
```console

```





```console
ufw allow 22
ufw allow 80
ufw allow 443
ufw allow <port of xui>
```


```console
ufw enable
ufw status
```

```console
ufw disable
```

# 2 install NGINX & Certbot
```console
sudo apt install nginx certbot python3-certbot-nginx -y
```

# 3 copy default NGINX config to your website
```console
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/hbv2ray.tk
```

# 4 enable your website 
```console
ln -s /etc/nginx/sites-available/hbv2ray.tk /etc/nginx/sites-enabled/
```


```console
cd /etc/nginx/sites-enabled && ls -la
```

# 5 edit your website config file and edit as below
```console
vi /etc/nginx/sites-available/hbv2ray.tk
```

# 5.1 remove default values  
```
server_name hbv2ray.tk;
```

# add another location
```console
location /downloader {

        if ($http_upgrade != "websocket") {

            return 404;

        }

        location ~ /downloader/\d\d\d\d\d$ {

            if ($request_uri ~* "([^/]*$)" ) {

                set $port $1;

            }

            proxy_redirect off;

            proxy_pass http://127.0.0.1:$port/;

            proxy_http_version 1.1;

            proxy_set_header Upgrade $http_upgrade;

            proxy_set_header Connection "upgrade";

            proxy_set_header Host $host;

            proxy_set_header X-Real-IP $remote_addr;

            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        }

        return 404;

    }
```

# 5.2 restart NGINX service	
```console
systemctl restart nginx
```

# 6 get a free SSL 
```console
certbot --nginx -d hbv2ray.tk --register-unsafely-without-email
```

# 7 install xRay panel FranzKafkaYu "https://github.com/FranzKafkaYu/x-ui/blob/main/README_EN.md"
```console
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install_en.sh)
```

# edit inbound
```console
listening ip: 127.0.0.1
network: ws
```

# edite in v2rayng
```console
address: hbv2ray.tk
port: 443
Network: ws
request host: hbv2ray.tk
path: /downloader/<inpund_port>
tls: tls
```


# 8 Note
if you want to use cdn, then use 127.0.0.1 in listening ip for users

