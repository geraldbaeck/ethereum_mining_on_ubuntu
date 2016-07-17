# GPU Mining for SIACoins


### Download and install Sia binaries
```bash
wget -qO- https://github.com/NebulousLabs/Sia/releases/download/v1.0.0/Sia-v1.0.0-linux-amd64.tar.gz | tar xvz
mv ~/Sia-v1.0.0-linux-amd64 ~/Sia
exit
```

### Configure supervisord so siad is always running
```bash
sudo apt-get -y install supervisor
sudo nano /etc/supervisor/supervisord.conf
```

#### Copy this at the bottom of supervisord.conf
```bash
[program:siad]
command=/home/geraldbaeck/Sia/siad
autostart=true
autorestart=true
user=geraldbaeck
directory=/home/geraldbaeck/Sia
priority=10
stopwaitsecs=60
stdout_logfile=/home/geraldbaeck/Sia/siad-stdout.log
stderr_logfile=/home/geraldbaeck/Sia/siad-stderr.log
```

#### Configure firewall and restart supervisor
```bash
sudo ufw allow 9982
sudo /etc/init.d/supervisor restart
```

#### Confirm siad is running
```bash
sudo tail -f /var/log/supervisor/supervisord.log
```

You should see something like this:
```bash
siad entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
```

You can also confirm siad is running by querying its API:
```bash
curl -s -X GET http://localhost:9980/consensus -A "Sia-Agent"
```

You should see something like this:
```bash
{"synced":false,"height":29731,"currentblock":"00000000000045c26833a3468024ab02bb8a1c9ebfe7e6214802d4e219bc5f0d","target":[0,0,0,0,0,1,137,117,99,158,187,183,52,35,143,246,125,232,33,67,117,64,219,147,103,161,85,248,122,236,204,243]}
```

### Configure siad as user `siad`
```bash
su siad
cd ~/Sia
```

#### Create new wallet
*Copy down the wallet seed/password!*
```bash
~/Sia/siac wallet init
```

### Or use an existing wallet
*prepare your seed*
```bash
~/Sia/siac wallet load
```

#### Unlock wallet (might take upwards to a minute, but probably less)
```bash
~/Sia/siac wallet unlock
```

#### Create a new wallet address
*Copy down the new wallet address!*
```bash
~/Sia/siac wallet address
```

#### configure the host for now do not take any contracts
```bash
~/Sia/siac host config acceptingcontracts false
exit
```

### Install the genoil Sia Miner
```bash
git clone https://github.com/Genoil/Sia-GPU-Miner
cd Sia-GPU-Miner/
make
```

#### run the miner
```bash
sudo ~/Sia-GPU-Miner/sia-gpu-miner -I 8
```
[configuration options](https://github.com/Genoil/Sia-GPU-Miner#configuration)
I had to restart the get the miner running before it stuck on "Initializing..."
you probably have to unlock your wallet before

### Use the [SiaMining](http://siamining.com/) pool (still testing)
```bash
export DISPLAY=:0
export GPU_FORCE_64BIT_PTR=0
export GPU_MAX_HEAP_SIZE=100  # 100 means that 100% of GPU memory are free to use for OpenCL see http://developer.amd.com/knowledge-base/?ID=123
export GPU_USE_SYNC_OBJECTS=1  # seems to be useful in a multi GPU environment
export GPU_MAX_ALLOC_PERCENT=100
export GPU_SINGLE_ALLOC_PERCENT=100
WALLET="0xd9d9aBe3e89123c171fF137bA50e81EC85dA8a2e"
WORKER="XFXR9390Test"
INTENSITY=25
sudo ~/Sia-GPU-Miner/sia-gpu-miner -I $INTENSITY -H siamining.com -P 9980 -Q "address=$WALLET&worker=$WORKER"
```
User [bitcanuk](https://www.reddit.com/user/bitcanuck) [commented on reddit](https://www.reddit.com/r/siacoin/comments/4smcqn/another_walktrough_for_sia_mining_with_ubuntu/d5bo96j) that setting those GPU variables is not really necessary for mining SIACoins. Will be interesting to see, if it makes any difference.

##### Look at your stats at:
- http://siamining.com/addresses/a051efc7151d90db02761078da0b49328b3b15296bd9e22dd8daf40b0da65c47615e38a74a15


##### see also:
- https://nerdralph.blogspot.co.at/2016/07/mining-sia-coin-on-ubuntu.html

##### other pools:
- [nanopool](http://sia.nanopool.org/) has EU Server und 1% less fee, not tested by me
