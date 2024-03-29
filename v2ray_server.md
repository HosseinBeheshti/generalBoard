
--------------------------------------------------------------------------
Referense Link: https://telegra.ph/reverse-proxy-on-nginx-for-x-ui-11-08
--------------------------------------------------------------------------

# 0 update server
```console
apt update && apt upgrade -y
```	

# 1 Firewall Settings
```console
apt install ufw -y
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

