# Install Vitis
```console
sudo apt install libtinfo5
sudo ./xsetup
sudo chmod 777 -R ./Xilinx
sudo chmod 777 -R ./.Xilinx
sudo  <installation_dir>/Vitis/20xx.x/scripts/installLibs.sh
```	
## petalinux
```console
sudo apt-get install -y gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison libselinux1 gnupg wget diffstat chrpath socat xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib build-essential libsdl1.2-dev libglib2.0-dev zlib1g:i386 screen pax gzip gawk
```
Note: install on ext4 partition
```console
mkdir -p /tools/Xilinx
cd /tools/
chmod 777 -R ./Xilinx
cd ./Xilinx
mkdir ./PetaLinux
chmod 755 -R ./PetaLinux/20xx.x/
./petalinux-v20xx.x-final-installer.run --dir /tools/Xilinx/PetaLinux/20xx.x/
source /tools/Xilinx/PetaLinux/20xx.x/settings.sh
```
change dash to bash
```console
sudo dpkg-reconfigure bash
```
# MATLAB
```console
sudo ./install
sudo chmod 777 -R ./R2016b
export PATH=$PATH:/usr/local/Polyspace/R20xxx/bin
```
## Model composer:
```console
sudo apt-get install libstdc++6
```
*******************************
# LaTex
```console
sudo ./inatall-tl
export PATH=$PATH:/usr/local/texlive/20xx/bin/x86_64-linux
export MANPATH=$MANPATH:/usr/local/texlive/20xx/texmf-dist/doc/man
export INFOPATH=$INFOPATH:/usr/local/texlive/20xx/texmf-dist/doc/info
```
## docker
```console
sudo docker run texlive/texlive
```
*******************************
# docker
```console
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker 
sudo chmod 666 /var/run/docker.sock
```
test: sudo docker run hello-world
Configure Docker to start on boot:
```console
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
# General
## vlc
```console
sudo snap install vlc
```
##	bing wallpaper
from software: BingWall - Bing wallpaper of the day
## Git configuration
```console
git autocrlf
git config --global core.autocrlf true
git config --global http.proxy 'socks5://127.0.0.1:9150'
git config --global --unset http.proxy
```
##	x2go server
```console
sudo apt update
sudo apt install xfce4 #(lightdm)
sudo apt install x2goserver x2goserver-xsession
```
## touchEgg for multi-touch
```console
sudo add-apt-repository ppa:touchegg/stable
sudo apt install touchegg
```
