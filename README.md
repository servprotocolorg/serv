<!--
parent:
  order: false
-->

<div align="center">
  <h1>SERV Protocol's serv codebase</h1>
</div>

<div align="center">
  Based on the work done by:
  <a href="https://github.com/EscanBE/evermint/blob/main/LICENSE">
    <img alt="License: LGPL-3.0" src="https://img.shields.io/github/license/EscanBE/evermint.svg" />
  </a>
  <a href="https://pkg.go.dev/github.com/evmos/evmos">
    <img alt="GoDoc" src="https://godoc.org/github.com/evmos/evmos?status.svg" />
  </a>
</div>

### SERV Protocol's serv is a customized EVM-enabled blockchain network

### About SERV's serv codebase

Serv is based on Evermint, a fork of open source Evmos v12.2.2+, maintained by Escan team with bug fixes, customization and enable developers to fork and transform to their chain, fully customized, in just 2 steps.

_Important Note: Evermint was born for development and research purpose so maintainers do not support migration for new upgrade/breaking changes._

### Evermint is based on Evmos

Evmos is a scalable, high-throughput Proof-of-Stake blockchain
that is fully compatible and interoperable with Ethereum.
It's built using the [Cosmos SDK](https://github.com/cosmos/cosmos-sdk/)
which runs on top of the [Tendermint Core](https://github.com/cometbft/cometbft) consensus engine.

### Different of Evermint & Evmos

- Evermint is for research and development purpose.
- Evermint is fork of open source Evmos v12.1.6 plus bug fixes.
- Evermint is [currently removing some modules](https://github.com/EscanBE/evermint/issues/41) from Evmos codebase and only keep `x/evm`, `x/erc20`, `x/feemarket`. The goal is to make it more simple for research and only focus on the skeleton of Evmos.

## Documentation

SERV's serv does not yet maintain its own documentation site, user can refer to Evmos documentation hosted at [evmos/docs](https://github.com/evmos/docs) and can be found at [docs.evmos.org](https://docs.evmos.org).
Head over there and check it out.

**Note**: Requires [Go 1.20+](https://golang.org/dl/)

## Quick Start

To learn how the Evmos works from a high-level perspective,
go to the [Protocol Overview](https://docs.evmos.org/protocol) section from the documentation.
You can also check the instructions to [Run a Node](https://docs.evmos.org/protocol/evmos-cli#run-an-evmos-node).

### Additional feature provided by Evermint:
1. Command convert between 0x address and bech32 address, or any custom bech32 HRP
```bash
servnode convert-address evm1sv9m0g7ycejwr3s369km58h5qe7xj77hxrsmsz evmos
# alias: "ca"
```
# Becoming A Validator

**How to validate on the SERV Testnet**

*(serv_43970-1)*

> Genesis file [Published](https://github.com/servprotocolorg/serv/releases/download/v12.2.3/genesis.json)
> Peers list [Published](https://github.com/servprotocolorg/serv/releases/download/v12.2.3/seeds.txt)

## Hardware Requirements

### Minimum:
* 8 GB RAM
* 100 GB SSD (Must Enable Prunning)
* 2.4+ GHz x4 CPU

### Recommended:
* 32 GB RAM
* 500 GB NVME SSD
* 4.2 GHz x6 CPU

### Operating System:
* Linux (x86_64) or Linux (amd64)
* Recommended Ubuntu or Arch Linux

## Install dependencies 

**If using Ubuntu:**

### Install all dependencies:

`sudo apt-get update`

`sudo apt-get -y upgrade`

#### Install via apt
`sudo apt install build-essential git vim jq libleveldb-dev make gcc`

#### Or install via apt-get individually:
* build-essential : `sudo apt-get install build-essential`
* libleveldb-dev : `sudo apt-get install libleveldb-dev`
* jq : `sudo apt-get install jq`
* git: `sudo apt-get install git`
* gcc: `sudo apt-get install gcc`
* make: `sudo apt-get install make`


[____
#### **If using Arch Linux:**
* jq: `pacman -S jq`
* git: `pacman -S git`
* gcc: `pacman -S gcc`
* make: `pacman -S make`
____]

## Install Go

If you have already installed go on this machine, remove it.
`sudo rm -rf /usr/local/go`

Download GO locally 
`sudo wget https://go.dev/dl/go1.20.13.linux-amd64.tar.gz`

Install GO locally
`sudo tar -C /usr/local -xzf go1.20.13.linux-amd64.tar.gz`

## Setup Go Environment

Create your validator's directories to compile, build and run from
`mkdir -p $HOME/go/src -p $HOME/go/pkg -p $HOME/go/bin`

Add usr/local Go bin and your Validators go/bin to .bashrc 

`echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bashrc`

`echo "export GOPATH=$HOME/go" >> $HOME/.bashrc`

reload the .bashrc locally

`source .bashrc`

## Install servnode binaries

### Clone git repository

```bash
git clone https://github.com/servprotocolorg/serv.git
cd serv
make install
```

### Generate and store keys

Replace `<keyname>` below with whatever you'd like to name your key.

*  `servnode keys add <key_name>`
*  `servnode keys add <key_name> --recover` to regenerate keys with your mnemonic
*  `servnode keys add <key_name> --ledger` to generate keys with ledger device

Store a backup of your keys and mnemonic securely offline.

Then save the generated public key config in the main servermint directory as `<key_name>.info`. It should look like this:

```

pubkey: {
  "@type":" ethermint.crypto.v1.ethsecp256k1.PubKey",
  "key":"############################################"
}

```

You'll use this file later when creating your validator txn.

## Set up validator

Install servnode binary from `serv` directory: 

`sudo make install`

Initialize the node. Replace `<moniker>` with whatever you'd like to name your validator.

TESTNET 
`servnode init <moniker> --chain-id serv_43970-1`

MAINNET (NOT DEPLOYED YET)
`servnode init <moniker> --chain-id serv_53970-1`


If this runs successfully, it should dump a blob of JSON to the terminal.

Download the Genesis file: 

`wget https://github.com/servprotocolorg/serv/releases/download/v12.2.3/genesis.json -P $HOME/.serv/config/` 

> _**Note:** If you later get `Error: couldn't read GenesisDoc file: open /root/.serv/config/genesis.json: no such file or directory` put the genesis.json file wherever it wants instead, such as:
> 
> `sudo wget https://github.com/servprotocolorg/serv/releases/download/v12.2.3/genesis.json -P/root/.serv/config/`

Edit the minimum-gas-prices in `${HOME}/.serv/config/app.toml`:

`sed -i 's/minimum-gas-prices = "0aservo"/minimum-gas-prices = "0.0001aservo"/g' $HOME/.serv/config/app.toml`

Add persistent peers to `$HOME/.serv/config/config.toml`:

You can find these at 
`https://github.com/servprotocolorg/serv/releases/download/v12.2.3/seeds.txt`

### Set `servnode` to run automatically

* Start `servnode` by creating a systemd service to run the node in the background: 
* Edit the file: `sudo nano /etc/systemd/system/servnode.service`
* Then copy and paste the following text into your service file. Be sure to edit as you see fit.

```bash

[Unit]
Description=SERV Node
After=network.target

[Service]
Type=simple
User=servuser
WorkingDirectory=/home/servuser/go/bin
ExecStart=/home/servuser/go/bin/servnode start --trace --log_level info --json-rpc.api eth,txpool,net,debug,web3 --api.enable
Restart=on-failure
StartLimitInterval=0
RestartSec=3
LimitNOFILE=65535
LimitMEMLOCK=209715200

[Install]
WantedBy=multi-user.target

```

## Start the node

Reload the service files: 

`sudo systemctl daemon-reload`

Create the symlinlk: 

`sudo systemctl enable servnode.service`

Start the node: 

`sudo systemctl start servnode && journalctl -u servnode -f`

You should then get several lines of log files and then see: `No addresses to dial. Falling back to seeds module=pex server=node`

This is an indicator things thus far are working and now you need to create your validator txn. `^c` out and follow the next steps.

### Create Validator Transaction

Modify the following items below, removing the `<>`

- `<KEY_NAME>` should be the same as `<key_name>` when you followed the steps above in creating or restoring your key.
- `<VALIDATOR_NAME>` is whatever you'd like to name your node
- `<DESCRIPTION>` is whatever you'd like in the description field for your node
- `<SECURITY_CONTACT_EMAIL>` is the email you want to use in the event of a security incident
- `<YOUR_WEBSITE>` the website you want associated with your node
- `<TOKEN_DELEGATION>` is the amount of tokens staked by your node (`1aservo` should work here, but you'll also need to make sure your address contains tokens.)

TESTNET 
```bash
servnode tx staking create-validator \
--from <KEY_NAME> \
--chain-id serv_43970-1 \
--moniker="<VALIDATOR_NAME>" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.05 \
--details="<DESCRIPTION>" \
--security-contact="<SECURITY_CONTACT_EMAIL>" \
--website="<YOUR_WEBSITE>" \
--pubkey $(servnode tendermint show-validator) \
--min-self-delegation="1" \
--amount <TOKEN_DELEGATION>aservo \
--gas 600000 \
--gas-prices 2000000000aservo
```

MAINNET (NOT DEPLOYED YET)
```bash
servnode tx staking create-validator \
--from <KEY_NAME> \
--chain-id serv_53970-1 \
--moniker="<VALIDATOR_NAME>" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.05 \
--details="<DESCRIPTION>" \
--security-contact="<SECURITY_CONTACT_EMAIL>" \
--website="<YOUR_WEBSITE>" \
--pubkey $(servnode tendermint show-validator) \
--min-self-delegation="1" \
--amount <TOKEN_DELEGATION>aservo \
--gas 600000 \
--gas-prices 2000000000aservo
```
