# install core
Update
```console
apt update && apt upgrade -y
```	

```console
wget https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh

chmod +x install.sh

bash install.sh
```	
insert number 1

3.1 Please confirm that the time is accurate; the error range is ±3 minutes (Y/N):
y

3.2 Please enter your domain information (e.g. www.wulabing.com):
domain FQDN (TODO)

3.3 Please enter the connection port (default 443):
443

3.4 Please enter alterID (default 2, must be numeric):
2

***** The script downloads the latest source for Nginx, OpenSSL, and V2Ray. It then compiles everything directly from the source. This takes 10–20 minutes. *****

3.5 Please select the generated link type:
1: V2RayNG/V2RayN

3.6 Please select a supported TLS version (default 3):
***** Please note that if you use Quantaumlt X / Router / Legacy Shadowrocket / V2ray core lower than 4.18.1 please select compatibility mode *****
2: TLS1.2 and TLS1.3

3.7 load user configuration:
cat ~/v2ray_info.inf

# X-UI (https://github.com/hossinasaadi/x-ui)

```console
bash <(curl -Ls https://raw.githubusercontent.com/hossinasaadi/x-ui/master/install.sh)
```	



