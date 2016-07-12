# GPU Mining for SIACoins


### Download and install Sia binaries
```bash
wget https://github.com/NebulousLabs/Sia/releases/download/v1.0.0/Sia-v1.0.0-linux-amd64.tar.gz
tar xvzf ./Sia-v1.0.0-linux-amd64.tar.gz
mv Sia-v1.0.0-linux-amd64/ Sia/
exit
```

### Configure supervisord so siad is always running
```bash
sudo apt-get -y install supervisor
sudo nano /etc/supervisor/supervisord.conf
```

#### Copy this at the bottom of supervisord.conf
``shell
[program:siad]
command=/home/siad/Sia/siad
autostart=true
autorestart=true
user=siad
directory=/home/siad/Sia
priority=10
stopwaitsecs=60
stdout_logfile=/home/siad/Sia/siad-stdout.log
stderr_logfile=/home/siad/Sia/siad-stderr.log
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
./siac wallet init
```

#### Unlock wallet (might take upwards to a minute, but probably less)
```bash
./siac wallet unlock
```

#### Create a new wallet address
*Copy down the new wallet address!*
```bash
./siac wallet address
```

#### configure the host for now do not take any contracts
```bash
./siac host config acceptingcontracts false
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
