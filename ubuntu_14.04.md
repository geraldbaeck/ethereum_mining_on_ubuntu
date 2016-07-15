#Installing Ubuntu 14.04 and configuring your AMD GPUs

### Install [Ubuntu 14.04](http://releases.ubuntu.com/14.04/ubuntu-14.04.4-server-amd64.iso)

```bash
sudo apt-get update
sudo apt-get -y upgrade
```

You probably want to [add a user with key auth](http://dev.baeck.at/devops/2013/07/24/Add-an-ssh-user-with-key-to-an-EC2-instance/) and [secure your ssh server](http://dev.baeck.at/devops/2013/07/24/Secure-your-ssh-server/).

### Install AMD drivers

```bash
sudo apt-get -y install fglrx fglrx-core fglrx-dev fglrx-amdcccle
```
#### Configure bash
```bash
nano .bashrc
export DISPLAY=:0 # add to the end of the script
```

#### Configure profile
```bash
sudo nano /etc/profile
export GPU_MAX_ALLOC_PERCENT=100
export GPU_USE_SYNC_OBJECTS=1
export XAUTHORITY=~/.Xauthority
sudo reboot
```
these exports could probably go into .bashrc instead -- I believe either location works

#### Install minimal x server & display manager
```bash
sudo apt-get -y install xorg
sudo dpkg-reconfigure x11-common # reconfigure and choose "Anybody"
```

#### install nodm
nodm and xorg are needed to be able to configure the GPUs via ssh without graphical environment.
```bash
sudo apt-get install nodm
sudo nano /etc/default/nodm
  set "NODM_ENABLED=true"
sudo reboot
```

To test if you are able to overclock/configure your GPU try this
```bash
sudo aticonfig --odgt --odgc
```
