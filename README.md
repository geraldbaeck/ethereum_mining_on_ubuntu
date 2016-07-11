#Mining Etherum with Ubuntu

### Install [Ubuntu 15.04](http://releases.ubuntu.com/15.04/ubuntu-15.04-server-amd64.iso)

### Install [AMD drivers](https://forum.ethereum.org/discussion/2695/frontier-mining-setup-notes-ubuntu-15-04-geth-v1-0-amd-ethminer
)
```bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install unzip -y
wget https://www.dropbox.com/s/mujihzvohefq0bb/ADL_SDK8.zip
unzip ./ADL_SDK8.zip
wget https://www.dropbox.com/s/50wvvuky9bmhk4m/AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh
sudo sh ./AMD-APP-SDK-v2.9-1.599.381-GA-linux64.sh
sudo ln -s /opt/AMDAPPSDK-2.9-1 /opt/AMDAPP
sudo ln -s /opt/AMDAPP/include/CL /usr/include
sudo ln -s /opt/AMDAPP/lib/x86_64/* /usr/lib/
sudo ldconfig
sudo reboot
```

```bash
sudo apt-get install fglrx-updates
sudo aticonfig --adapter=all --initial
aticonfig --list-adapters
```

# Other Miners
- [Claymore](https://bitcointalk.org/index.php?topic=1433925.0)


# Overclocking info
https://forum.ethereum.org/discussion/5302/mining-with-ubuntu-server-do-i-really-need-x-gnome-the-graphical-environment


### Further readings:
[Stratum Proxy](https://github.com/slush0/stratum-mining-proxy#installation-on-linux-using-git) - Not needed, I guess
