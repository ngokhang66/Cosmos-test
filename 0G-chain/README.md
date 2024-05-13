## 0G Chain
|     Hardward    |   CPU   |  RAM  |  Storage  |  NETWORK | Port |
|-----------------|---------|-------|-----------|----------|------|
|    Requirement  | 8 cores | 64 GB | 1 TB NVME | 100 MBps |  321 |


**Explorer:** https://explorer.aznope.com/0G-Chain

**API:** https://api0g.aznope.com/

**RPC:** https://rpc0g.aznope.com/

**Snapshot:** 

**Faucet 0G:** [https://faucet.0g.ai/](https://faucet.0g.ai/)

# Node Setup
1. Install packages 
```
sudo apt update
sudo apt install curl tar wget clang pkg-config protobuf-compiler libssl-dev jq build-essential protobuf-compiler bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
2. Install GO via CLI
- Remove GO if you have
```
sudo rm -rf /usr/local/go
sudo rm /etc/paths.d/go
sudo apt-get remove golang-go
sudo apt-get remove --auto-remove golang-go
```
- Install
```
ver="1.21.10"
sudo rm -rf /usr/local/go
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" 
rm -rf /usr/local/go
tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm -rf "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile 
source $HOME/.bash_profile
go version
```
3. Install 0gchaind via CLI
```
git clone -b v0.1.0 https://github.com/0glabs/0g-chain.git
./0g-chain/networks/testnet/install.sh
source .profile
```
4. Install cosmovisor
```
mkdir -p $HOME/.0gchain/cosmovisor/genesis/bin
cp $HOME/go/bin/0gchaind $HOME/.0gchain/cosmovisor/genesis/bin/
sudo ln -s $HOME/.0gchain/cosmovisor/genesis $HOME/.0gchain/cosmovisor/current -f
sudo ln -s $HOME/.0gchain/cosmovisor/current/bin/0gchaind /usr/local/bin/0gchaind -f
cd $HOME
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```
5. Initialize Node
- Input NODE_NAME with your node name you want
  ```
  echo "export NODE_NAME=INPUT YOUR NODE NAME HERE" >> ~/.bash_profile
  ```
```
echo "export OG_WALLET=wallet" >> $HOME/.bash_profile
source $HOME/.bash_profile
0gchaind config chain-id zgtendermint_16600-1
0gchaind config keyring-backend os
0gchaind init $NODE_NAME --chain-id zgtendermint_16600-1

```
6. Configure Genesis & Seeds
- Copy the Genesis File
```
sudo apt install -y unzip wget
rm ~/.0gchain/config/genesis.json
wget -P ~/.0gchain/config https://github.com/0glabs/0g-chain/releases/download/v0.1.0/genesis.json
```
- Add Seed && Peers Node
```
PEERS="fbc3b6d41cd39a62ef5e3fc596435adfaf428a34@37.120.189.81:16656,645531eb02b275a59cc3b1af99e541852849f695@84.247.139.25:16656,d00273ac6a2470cd4e48008d9af4d2521b134394@62.169.29.136:26656,f5a7d34355f6d89b7ece583131c6b1f79ac5485e@218.102.97.67:25856,a3e6c6214805c1c068882f1981855c7a9f5926ea@213.168.249.202:26656,da1f4985ce3df05fd085460485adefa93592a54c@172.232.33.25:26656,91f079ccd2e0edf42e0fa57183ac92c22c525658@14.245.25.144:14256,9d09d391b2cf706a597d03fe8bb6700fe5cac53d@65.108.198.183:18456,5a202fb905f20f96d8ff0726f0c0756d17cf23d8@43.248.98.100:26656,74775d65b6ab427c685efcaa8190912d3a60e562@123.19.45.21:12656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,9d7564df34efa146a94c073e5bf3f5e11f947b75@155.133.22.230:26656,e179d05dc792d9b902be3baa7a31a07a92afbcf0@118.142.83.5:26656,c4b9c3a7f3651af729d73b150e714ee91e7585c1@14.176.200.133:26656,f64f0fb500c62bffa33d60450d30792ee4b5fbd0@167.86.119.168:26656,d4085fd93ab77576f2acdb25d2d817061db5afe6@62.169.19.156:26656,2b8ee12f4f94ebc337af94dbec07de6f029a24e6@94.16.31.161:26656,0f5022e4265184052a5468379687625a81fd255e@154.12.253.116:26656,3859828e1099214de14dae91d1f7decf2374eeb4@47.236.170.254:26656,23b0a0624699f85062ddebf910583f70a5b9e86b@14.167.152.116:14256,b8f8ed478f2794629fdb5cf0c01edaed80f00f84@168.119.64.172:26656,5d81d59e81356a33e6ccccaa3d419ff73244697e@107.173.18.103:26656,c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,a83f5d07a8a64827851c9f1d0c21c900b9309608@188.166.181.110:26656,19943cbe46cdb9eb37cb06c0067ce63154eee6ea@213.199.52.155:26656,a6ff8a651dd0a0e66dbfb2174ccadcbbcf567b29@66.94.122.224:26656,f3c912cf5653e51ee94aaad0589a3d176d31a19d@157.90.0.102:31656,141dbd90d5c3411c9ba72ba03704ccdb70875b01@65.109.147.58:36656,cd529839591e13f5ed69e9a029c5d7d96de170fe@46.4.55.46:34656,a8d7c5a051c4649ba7e267c94e48a7c64a00f0eb@65.108.127.146:26656" && \
SEEDS="c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,44d11d4ba92a01b520923f51632d2450984d5886@54.176.175.48:26656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,f878d40c538c8c23653a5b70f615f8dccec6fb9f@54.215.187.94:26656" && \
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.0gchain/config/config.toml
```
- Configure setting
```
# Setting Pruning 
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.0gchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.0gchain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.0gchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.0gchain/config/app.toml
```
```
# Enabling Prometheus
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.0gchain/config/config.toml
# Set up the minimum gas price
sed -i "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ua0gi\"/" $HOME/.0gchain/config/app.toml
```
7. Custom port (Optional)
- You can ignore it
```
echo 'export 0g="321"' >> ~/.bash_profile
source $HOME/.bash_profile
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://0.0.0.0:${0g}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://0.0.0.0:${0g}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${0g}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${0g}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${0g}60\"%" $HOME/.0gchain/config/config.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://0.0.0.0:${0g}17\"%; s%^address = \":8080\"%address = \":${0g}80\"%; s%^address = \"localhost:9090\"%address = \"0.0.0.0:${0g}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${0g}91\"%; s%:8545%:${0g}45%; s%:8546%:${0g}46%; s%:6065%:${0g}65%" $HOME/.0gchain/config/app.toml
0gchaind config node tcp://localhost:${0g}57
```
8. Create Service File
```
sudo tee /etc/systemd/system/0gd.service > /dev/null << EOF
[Unit]
Description=0G-chain node service
After=network-online.target
 
[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.0gchain"
Environment="DAEMON_NAME=0gchaind"
 
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable 0gd
```
- Start node service
```
sudo systemctl daemon-reload
sudo systemctl restart 0gd
```
- Check logs node
```
# ctrl-c to turn off follow log
sudo journalctl -u 0gd -f --no-hostname -o cat
```
9. Create Wallet
- Create new wallet
```
0gchaind keys add $OG_WALLET --eth
```
- Restore wallet
```
0gchaind keys add $OG_WALLET --eth --recover
```
10. Create Validator
```
0gchaind tx staking create-validator \
--amount=1000000ua0gi \
--pubkey=$(0gchaind tendermint show-validator) \
--moniker=$NODE_NAME \
--chain-id=zgtendermint_16600-1\
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.05 \
--min-self-delegation="1" \
--gas-prices=0.25ua0gi  \
--gas-adjustment=1.5 \
--gas=auto \
--from=$OG_WALLET \
--details="write everything you want" \
--security-contact="xxxxxxx@xxx.com" \
--website="https://your website.com" \
--identity="your avatar" \
--yes
```
## Delete node
```
cd $HOME
sudo systemctl stop 0gd
sudo systemctl disable 0gd
sudo rm /etc/systemd/system/0gd.service
sudo systemctl daemon-reload
sudo rm $HOME/go/bin/0gchaind
sudo rm -f $(which 0gchaind)
sudo rm -rf $HOME/0g-chain
sudo rm -rf $HOME/.0gchain
```
## User Command
- Delegate token
```
0gchaind tx staking delegate $(0gchaind keys show OG_WALLET --bech val -a) 1000000ua0gi --from $OG_WALLET -y
```
- Unjail Node (if your node is jailed)
```
0gchaind tx slashing unjail --from $OG_WALLET --gas=500000 --gas-prices=0.25ua0gi -y
```
- Balance wallet
```
0gchaind q bank balances $(0gchaind keys show OG_WALLET -a)
```
