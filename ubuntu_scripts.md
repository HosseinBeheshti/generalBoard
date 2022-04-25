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
# GitLab runner
Start by logging in to your server:
```console
ssh sammy@your_server_ip
```
```console
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```

```console
sudo chmod +x /usr/local/bin/gitlab-runner
```

```console
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

```console
sudo rm /etc/systemd/system/gitlab-runner.service
```

```console
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-r
```

```console
sudo usermod -aG sudo gitlab-runner
```

## Registering a GitLab Runner
In your GitLab project, navigate to Settings > CI/CD > Runners.
In the Set up a specific Runner manually section, you’ll find the registration token and the GitLab URL. Copy both to a text editor; you’ll need them for the next command. They will be referred to as `https://your_gitlab.com` and `project_token`.
Back to your terminal, register the runner for your project:
```console
sudo gitlab-runner register \
-n --url https://your_gitlab.com \
--registration-token project_token \
--executor "shell" \
--description "Deployment Runner" \
--tag-list deployment
```
The command options can be interpreted as follows:

- `-n` executes the register command non-interactively (we specify all parameters as command options).
- `--url` is the GitLab URL you copied from the runners page in GitLab.
- `--registration-token` is the token you copied from the runners page in GitLab.
- `--executor` is the executor type
- `--description` is the runner’s description, which will show up in GitLab.
- `--tag-list` is a list of tags assigned to the runner. Tags can be used in a pipeline configuration to select specific runners for a CI/CD job. The deployment tag will allow you to refer to this specific runner to execute the deployment job.
- --docker-privileged executes the Docker container created for each CI/CD job in privileged mode. A privileged container has access to all devices on the host machine and has nearly the same access to the host as processes running outside containers (see Docker’s documentation about runtime privilege and Linux capabilities). The reason for running in privileged mode is so you can use Docker-in-Docker (dind) to build a Docker image in your CI/CD pipeline. It is good practice to give a container the minimum requirements it needs. For you it is a requirement to run in privileged mode in order to use Docker-in-Docker. Be aware, you registered the runner for this specific project only, where you are in control of the commands being executed in the privileged container.
After executing the gitlab-runner register command, you will receive the following output:
```
Output
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

NOTE: A common failure is when you have a .bash_logout that tries to clear the console. 
comment this lines in `/home/gitlab-runner/.bash_logout`
```
if [ "$SHLVL" = 1 ]; then
    [ -x /usr/bin/clear_console ] && /usr/bin/clear_console -q
fi
```
##Uninstall
```console
sudo gitlab-runner uninstall
sudo rm -rf /usr/local/bin/gitlab-runner
sudo userdel gitlab-runner
sudo rm -rf /home/gitlab-runner/
```
# General
# Disable Ubuntu Sleep Mode
Use the following command to disable sleep, hibernation and etc.  
```console
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
Then reboot the system and check if the changes have been effected using the command:  
```console
sudo systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target
```
## Tor
```console
sudo apt install tor
```
```console
sudo vi /etc/tor/torrc
```
Find the line containing the following:
```
#ControlPort 9051
```
…and uncomment it. Next, find the following line:

```
#CookieAuthentication 1
```
Uncomment it, and change 1 to 0.

Finally, restart the tor service:
```console
sudo /etc/init.d/tor restart
```
If you want to force Tor to generate a new circuit, and thus a new IP, use the following command:
```console
echo -e 'AUTHENTICATE ""\r\nsignal NEWNYM\r\nQUIT' | nc 127.0.0.1 9051
```
remove from auto start
```console
sudo systemctl disable tor
```
stop tor
```console
sudo systemctl stop tor
```
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
