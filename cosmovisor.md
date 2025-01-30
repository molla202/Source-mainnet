![image](https://github.com/molla202/Source-sourcetest-1/assets/91562185/873a99e6-aff2-4da5-a02a-16f9513b6b6c)

🌟 [Source Protocol Twitter](https://twitter.com/SourceProtocol_)

🌟 [Source Protocol Discord](https://discord.gg/MuPN6kJbCK)

🌟 [Source Protocol Explorer](https://mainnet.itrocket.net/source/staking/sourcevaloper12xtalgwjakzdz4q8s05zkm0a3nkr5wlua77q2k)

🔥 [CoreNode Telegram](https://t.me/corenode)

🔥 [CoreNode Twitter](https://twitter.com/corenodehq)

💬 [Gökhan Molla Twitter](https://twitter.com/gokhan_molla)

💬 [Gökhan Molla Telegram](https://t.me/gokhan_molla)

💬 Sorularınız için yukarıdaki adreslerden ulaşabilirsiniz.


 ## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| ✔️ CPU |	8+ |
| ✔️ RAM	| 16+ GB |
| ✔️ Storage	| 500GB+ SSD |


### 🚧Update
```
sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential
sudo apt -qy upgrade
```
### 🚧Go
```
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.19.12.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)
```

### 🚧Dosyaları çekıyoruz
```
cd $HOME
rm -rf source
git clone https://github.com/Source-Protocol-Cosmos/source.git
cd source
git checkout v3.0.2
```
```
make build
```
### 🚧Cosmovisor ayarlıyoruz
```
mkdir -p $HOME/.source/cosmovisor/genesis/bin
mv bin/sourced $HOME/.source/cosmovisor/genesis/bin/
rm -rf build
```
### 🚧Sistem kısayolları
```
sudo ln -s $HOME/.source/cosmovisor/genesis $HOME/.source/cosmovisor/current -f
sudo ln -s $HOME/.source/cosmovisor/current/bin/sourced /usr/local/bin/sourced -f
```

### 🚧Cosmovisor kuruyoruz
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```
### 🚧Servis olusturuyoruz
```
sudo tee /etc/systemd/system/sourced.service > /dev/null << EOF
[Unit]
Description=source node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.source"
Environment="DAEMON_NAME=sourced"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.source/cosmovisor/current/bin"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable sourced.service
```

### 🚧Node ayarları
```
sourced config chain-id source-1
sourced config keyring-backend file
sourced config node tcp://localhost:12857
```
### 🚧Node kuruyoruz
```
sourced init node-adınız --chain-id source-1
```
### 🚧indiriyoruz genesis ved addrbook
```
curl -Ls http://37.120.189.81/source_mainnet/genesis.json > $HOME/.source/config/genesis.json
curl -Ls http://37.120.189.81/source_mainnet/addrbook.json > $HOME/.source/config/addrbook.json
```
### 🚧Seed gas puring ayarları
```
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@source.rpc.kjnodes.com:12859\"|" $HOME/.source/config/config.toml


sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025usource\"|" $HOME/.source/config/app.toml


sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.source/config/app.toml
```
### 🚧Port değiştiriyoruz
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:12858\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:12857\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:12860\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:12856\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":12866\"%" $HOME/.source/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:12817\"%; s%^address = \":8080\"%address = \":12880\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:12890\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:12891\"%; s%:8545%:12845%; s%:8546%:12846%; s%:6065%:12865%" $HOME/.source/config/app.toml
```
### 🚧Snap
```
curl -L http://37.120.189.81/source_mainnet/source_snap.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.source
[[ -f $HOME/.source/data/upgrade-info.json ]] && cp $HOME/.source/data/upgrade-info.json $HOME/.source/cosmovisor/genesis/upgrade-info.json
```
### 🚧Başlatıyoruz...
```
sudo systemctl restart sourced.service && sudo journalctl -u sourced.service -f --no-hostname -o cat
```
