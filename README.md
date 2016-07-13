#Mining Etherum with Ubuntu

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
Q: Why do i have to sudo aticonfig?

### Install the [genoil miner](https://github.com/Genoil/cpp-ethereum#building-on-ubuntu)

```bash
sudo apt-get -y install software-properties-common
add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install git cmake libcryptopp-dev libleveldb-dev libjsoncpp-dev libjson-rpc-cpp-dev libboost-all-dev libgmp-dev libreadline-dev libcurl4-gnutls-dev ocl-icd-libopencl1 opencl-headers mesa-common-dev libmicrohttpd-dev build-essential -y
git clone https://github.com/Genoil/cpp-ethereum/
cd cpp-ethereum/
mkdir build
cd build
cmake -DBUNDLE=miner ..
make -j8
```
You can then find the executable in the ethminer subfolder.

TODO:
- make the executeable globally accessible

### Run the miner

```bash
export GPU_FORCE_64BIT_PTR=0
export GPU_MAX_HEAP_SIZE=100
export GPU_USE_SYNC_OBJECTS=1
export GPU_MAX_ALLOC_PERCENT=100
export GPU_SINGLE_ALLOC_PERCENT=100
WALLET="0xd9d9aBe3e89123c171fF137bA50e81EC85dA8a2e"
WORKER="XFXR9390Test"
~/cpp-ethereum/build/ethminer/ethminer -G -S pool.alpereum.ch:3001 -O $WALLET.$WORKER
```
Look up your stats:
https://www.alpereum.ch/worker/?address=0xd9d9aBe3e89123c171fF137bA50e81EC85dA8a2e

TODO:
- generate a worker name
- get wallet address from remote server





#### Other Miners
- [Claymore](https://bitcointalk.org/index.php?topic=1433925.0)  # did only work with a very low hashrate, did not investigate further


#### Overclocking

If you get "ERROR - X needs to be running" do a
```bash
sudo startx &
```

#### Links:
- http://www.overclock.net/t/517861/how-to-overclocking-ati-cards-in-linux
- https://forum.ethereum.org/discussion/5302/mining-with-ubuntu-server-do-i-really-need-x-gnome-the-graphical-environment


#### Further readings:
[Stratum Proxy](https://github.com/slush0/stratum-mining-proxy#installation-on-linux-using-git) - Not needed, I guess
