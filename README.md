# vagrant-docker-wls2
[![GitHub version](https://badge.fury.io/gh/thanatos-vf-2000%2Fvagrant-docker-wls2.svg)](https://badge.fury.io/gh/thanatos-vf-2000%2Fvagrant-docker-wls2)

[![Latest Stable Version](http://poser.pugx.org/thanatos-vf-2000/vagrant-docker-wls2/v)](https://packagist.org/packages/thanatos-vf-2000/vagrant-docker-wls2) [![Total Downloads](http://poser.pugx.org/thanatos-vf-2000/vagrant-docker-wls2/downloads)](https://packagist.org/packages/thanatos-vf-2000/vagrant-docker-wls2) [![Latest Unstable Version](http://poser.pugx.org/thanatos-vf-2000/vagrant-docker-wls2/v/unstable)](https://packagist.org/packages/thanatos-vf-2000/vagrant-docker-wls2) [![License](http://poser.pugx.org/thanatos-vf-2000/vagrant-docker-wls2/license)](https://packagist.org/packages/thanatos-vf-2000/vagrant-docker-wls2) [![PHP Version Require](http://poser.pugx.org/thanatos-vf-2000/vagrant-docker-wls2/require/php)](https://packagist.org/packages/thanatos-vf-2000/vagrant-docker-wls2)

Exemple of Vagrant and Docker in WSL2


Docker installation on WSL
------------

Installing Docker in WSL
```shell
# Installation requirements
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Adding the Docker repo
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Adding the Docker repo key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Docker installation
sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io

# Add the current user to the Docker group
sudo usermod -aG docker $USER
```

Update IPTables
```shell
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```

Create /etc/profile.d/vagrant.sh
```shell
sudo vi /etc/profile.d/vagrant.sh
```

Add this line to file
```shell
#https://developer.hashicorp.com/vagrant/docs/other/wsl
export PATH="$PATH:/mnt/c/Windows/System32/WindowsPowerShell/v1.0"
export PATH="$PATH:/mnt/c/Windows/System32"
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
```

change right to file
Add this line to file
```shell
sudo chmod a+x /etc/profile.d/vagrant.sh
```


* Under windows 11, you need to add these lines to /etc/wsl.conf

Edit file /etc/wsl.conf
```shell
sudo vi /etc/wsl.conf
```
Add this line
```shell
[boot]
command = "service docker start"
```

Run this command on cmd or powershell
```shell
wsl.exe --shutdown
```

Once the WSL machine is restarted, check that Docker is operational:

```shell
docker ps
```

* Under Windows 10, you must add these lines to your .profile 

Edit file .profile
```shell
vi ~/.profile

```

Add this lines and replace WSL_DISTRO_NAME[^1] by the image name
```shell
if service docker status 2>&1 | grep -q "is not running"; then
    wsl.exe -d "${WSL_DISTRO_NAME}" -u root -e /usr/sbin/service docker start >/dev/null 2>&1
fi
```

Run this command on cmd or powershell
```shell
wsl.exe --shutdown
```

Once the WSL machine is restarted, check that Docker is operational:

```shell
docker ps
```


Installation of Vagrant
------------

Add repository and install
```shell
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
```

Restart WSL, run this command on cmd or powershell
```shell
wsl.exe --shutdown
```

[^1]: run in powershell commande wsl -l -v

Images
------------
 - [x] [Ubuntu](https://hub.docker.com/_/ubuntu)
   - [ ] 14.04 - trusty
   - [x] 16.04 - xenial
   - [x] 18.04 - bionic
   - [x] 20.04 - focal
   - [ ] 22.04 - jammy
   - [ ] 22.10 - kinetic
 - [ ] Windows