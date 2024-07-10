# Installation
## Setup validator name
```bash
MONIKER="YOUR_MONIKER_GOES_HERE"
```
## Install dependencies
```bash
sudo apt update
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y
```
## Istall GO
```bash
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl <https://dl.google.com/go/go1.20.5.linux-amd64.tar.gz> | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

## Install Node
```bash
cd $HOME
rm -rf planq
git clone <https://github.com/planq-network/planq.git>
cd planq
git checkout v1.0.7
make build
```

## Initialize Node
```bash
planqd init "$MONIKER" --chain-id=planq_7070-2
```
## Create Service
```bash
sudo tee /etc/systemd/system/planqd.service > /dev/null <<EOF
[Unit]
Description=planqd Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which planqd) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload

sudo systemctl enable planqd
```
## Start the node
```bash
sudo systemctl restart planqd
journalctl -u planqd -f
```
