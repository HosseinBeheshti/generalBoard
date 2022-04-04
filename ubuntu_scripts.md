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
Based on [digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-continuous-deployment-pipeline-with-gitlab-ci-cd-on-ubuntu-18-04).
## Registering a GitLab Runner
Start by logging in to your server:
```console
ssh sammy@your_server_IP
```
In order to install the gitlab-runner service, you’ll add the official GitLab repository. Download and inspect the install script:
```console
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh > script.deb.sh
less script.deb.sh
```
Once you are satisfied with the safety of the script, run the installer:
```console
sudo bash script.deb.sh
```
Next install the gitlab-runner service:
```console
sudo apt install gitlab-runner
```
Verify the installation by checking the service status:
```console
systemctl status gitlab-runner
```
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

Add the user to the sudo group:
```console
sudo usermod -aG sudo gitlab-runner
```

## Setting Up an SSH Key
You are going to create an SSH key for the deployment user. GitLab CI/CD will later use the key to log in to the server and perform the deployment routine.
Let’s start by switching to the newly created deployer user for whom you’ll generate the SSH key:
```console
su deployer
```
To summarize, run the following command and confirm both questions with ENTER to create a 4096-bit SSH key and store it in the default location with an empty passphrase:
```console
ssh-keygen -b 4096
```
To authorize the SSH key for the deployer user, you need to append the public key to the authorized_keys file:
```console
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
## Storing the Private Key in a GitLab CI/CD Variable
Start by showing the SSH private key:
```console
cat ~/.ssh/id_rsa
```
Copy the output to your clipboard. Make sure to add a linebreak after -----END RSA PRIVATE KEY-----:
```console
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```
Now navigate to Settings > CI / CD > Variables in your GitLab project and click Add Variable. Fill out the form as follows:
- Key: `ID_RSA`
- Value: Paste your SSH private key from your clipboard (including a line break at the end).
- Type: File
- Environment Scope: All (default)
- Protect variable: Checked
- Mask variable: Unchecked


A file containing the private key will be created on the runner for each CI/CD job and its path will be stored in the `$ID_RSA` environment variable.

Create another variable with your server IP. Click Add Variable and fill out the form as follows:

- Key: `SERVER_IP`
- Value: `your_server_IP`
- Type: Variable
- Environment scope: All (default)
- Protect variable: Checked
- Mask variable: Checked

Finally, create a variable with the login user. Click Add Variable and fill out the form as follows:
- Key: `SERVER_USER`
- Value: `deployer`
- Type: Variable
- Environment scope: All (default)
- Protect variable: Checked
- Mask variable: Checked

# General
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
