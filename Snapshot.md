
![image](https://github.com/molla202/Source-sourcetest-1/assets/91562185/873a99e6-aff2-4da5-a02a-16f9513b6b6c)

🌟 [Source Protocol Twitter](https://twitter.com/SourceProtocol_)

🌟 [Source Protocol Discord](https://discord.gg/MuPN6kJbCK)

🌟 [Source Protocol Explorer](https://mainnet.itrocket.net/source/staking/sourcevaloper12xtalgwjakzdz4q8s05zkm0a3nkr5wlua77q2k)

🔥 [CoreNode Telegram](https://t.me/corenode)

🔥 [CoreNode Twitter](https://twitter.com/corenodehq)

💬 [Gökhan Molla Twitter](https://twitter.com/gokhan_molla)

💬 [Gökhan Molla Telegram](https://t.me/gokhan_molla)

💬 Sorularınız için yukarıdaki adreslerden ulaşabilirsiniz.


## Daily Snapshot

```
sudo apt install liblz4-tool

systemctl stop sourced

cp $HOME/.source/data/priv_validator_state.json $HOME/.source/priv_validator_state.json.backup

sourced tendermint unsafe-reset-all --home $HOME/.source --keep-addr-book

curl -L http://37.120.189.81/source_mainnet/source_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.source

mv $HOME/.source/priv_validator_state.json.backup $HOME/.source/data/priv_validator_state.json

sudo systemctl restart sourced && sudo journalctl -u sourced -fo cat
```
