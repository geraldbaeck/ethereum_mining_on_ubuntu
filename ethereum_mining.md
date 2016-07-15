#Mining Etherum with Ubuntu

This has been tested on [Ubuntu 14.04](ubuntu_14.04.md) and [15.04](ubuntu_15.04.md) as well.

### Install the [genoil miner](https://github.com/Genoil/cpp-ethereum#building-on-ubuntu)

```bash
sudo apt-get -y install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install -y git cmake libcryptopp-dev libleveldb-dev libjsoncpp-dev libjson-rpc-cpp-dev libboost-all-dev libgmp-dev libreadline-dev libcurl4-gnutls-dev ocl-icd-libopencl1 opencl-headers mesa-common-dev libmicrohttpd-dev build-essential
git clone https://github.com/Genoil/cpp-ethereum/
mkdir cpp-ethereum/build
cd cpp-ethereum/build
cmake -DBUNDLE=miner ..
make -j8
```
You can then find the executable at ~/cpp-ethereum/build/ethminer/ethminer.


### Run the miner

create a shell script to run the miner
```bash
cat > ~/mine_ether.sh << EOF
export GPU_FORCE_64BIT_PTR=0
export GPU_MAX_HEAP_SIZE=100
export GPU_USE_SYNC_OBJECTS=1
export GPU_MAX_ALLOC_PERCENT=100
export GPU_SINGLE_ALLOC_PERCENT=100
export ETH_WALLET="0xd9d9aBe3e89123c171fF137bA50e81EC85dA8a2e"
export WORKER="XFXR9390Test"
~/cpp-ethereum/build/ethminer/ethminer -G -S pool.alpereum.ch:3001 -O \$ETH_WALLET.\$WORKER
EOF
sudo chmod +x ~/mine_ether.sh
```

actually run the miner
```bash
sudo ~/mine_ether.sh
```

Look up your stats:
https://www.alpereum.ch/worker/?address=0xd9d9aBe3e89123c171fF137bA50e81EC85dA8a2e

TODO:
- generate a worker name
- get wallet address from remote server


#### Other Miners
- [Claymore](https://bitcointalk.org/index.php?topic=1433925.0)  # did only work with a very low hashrate, did not investigate further


#### Overclocking

TODO:
- look into aticonfig
- look into atitweak

#### Links:
- http://www.overclock.net/t/517861/how-to-overclocking-ati-cards-in-linux
- https://forum.ethereum.org/discussion/5302/mining-with-ubuntu-server-do-i-really-need-x-gnome-the-graphical-environment


#### Further readings:
[Stratum Proxy](https://github.com/slush0/stratum-mining-proxy#installation-on-linux-using-git) - Not needed, I guess
