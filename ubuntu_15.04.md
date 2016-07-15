#Installing Ubuntu 15.04 and the AMD GPU drivers

I have some difficulties with this installation to configure the GPUs with aticonfig. So overclocking is a bit of a hazzle. That's why i switched to [running 14.04](ubuntu_14.04.md).

### Install [Ubuntu 15.04](http://releases.ubuntu.com/15.04/ubuntu-15.04-server-amd64.iso)

```bash
sudo apt-get update
sudo apt-get -y upgrade
```

You probably want to [add a user with key auth](http://dev.baeck.at/devops/2013/07/24/Add-an-ssh-user-with-key-to-an-EC2-instance/) and [secure your ssh server](http://dev.baeck.at/devops/2013/07/24/Secure-your-ssh-server/).

### Install [AMD drivers](https://forum.ethereum.org/discussion/2695/frontier-mining-setup-notes-ubuntu-15-04-geth-v1-0-amd-ethminer)

```bash
sudo apt-get -y install gunzip
#wget https://www.dropbox.com/s/mujihzvohefq0bb/ADL_SDK8.zip
#unzip ./ADL_SDK8.zip
wget -nc -O - https://www.dropbox.com/s/25ro8fzra3o3c6t/ADL_SDK8.zip | gunzip -
wget https://www.dropbox.com/s/yw24y75e40il007/AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh
sudo sh ./AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh
sudo ln -s /opt/AMDAPPSDK-2.9-1 /opt/AMDAPP
sudo ln -s /opt/AMDAPP/include/CL /usr/include
sudo ln -s /opt/AMDAPP/lib/x86_64/* /usr/lib/
sudo ldconfig
rm ADL_SDK8.zip
rm AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh
sudo reboot
```
TODO:
- try sh ./AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh without sudo next time (use /opt dir)

```bash
sudo apt-get install fglrx-updates
sudo aticonfig --adapter=all --initial
sudo aticonfig --list-adapters
```
