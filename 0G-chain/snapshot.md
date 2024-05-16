# Snapshot 0G chain
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.

My snapshot: [https://snap0g.aznope.com/downloads/0gchain.tar.lz4](https://snap0g.aznope.com/downloads/0gchain.tar.lz4)

# Instructions
1. Stop the service and reset the data
 ```
sudo systemctl stop 0gd
cp $HOME/.0gchain/data/priv_validator_state.json $HOME/.0gchain/priv_validator_state.json.backup
0gchaind tendermint unsafe-reset-all --home $HOME/.0gchain --keep-addr-book
rm -rf $HOME/.0gchain/data
```
2. Download latest snapshot
```bash
curl -L https://snap.aznope.com/downloads/0gchain.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.0gchain
mv $HOME/.0gchain/priv_validator_state.json.backup $HOME/.0gchain/data/priv_validator_state.json
```
3. Restart the service and check the log
```bash
sudo systemctl start 0gd && sudo journalctl -u 0gd -f --no-hostname -o cat
```
