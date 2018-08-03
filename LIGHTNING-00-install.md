[ [index](/README.md), [next](/LIGHTNING-01-peers.md) ]

# Lightning
This guide can be used to perform an instant BTC<->LTC atomic swap between two exchanges A & B on the lightning network. The exchanges are running the lightning network daemon (`lnd`) and are connected to the Bitcoin and Litecoin networks using `btcd` and `ltcd`. The guide assumes that the reader has some lightning knowledge and is capable of setting up a simple network, fund wallets and open payment channels.

# Describing the setup

## Single Exchange setup
Each exchange (A, B) is running several components which together provide full support for atomic swap between BTC and LTC:
1. BTCD - full node, connected to the Bitcoin chain
2. LTCD - full node, connected to the Litecoin chain
3. LND-BTC - lightning network daemon for the BTC network
4. LND-LTC - lightning network daemon for the LTC network
5. XUD simulator 

## Multiple Exchanges setup
We are going to setup two exchanges on a single machine. For that we would need to run 2x5 processes. Luckily, we can share the BTCD and the LTCD between the two exchanges so we can set everything up using 8 processes. 

Since everything is running on a single machine, we need to assign different working director and ports for each process. More aboit this later.

### Terminal setting
It is recommended to open 9 terminals to be used with this PoC, allowing each component to run in its own terminal with an additional terminal being used for commands (CLI) 

# Installation 
## Install Lightning Dependencies
Download latest `Go` package from [offical Go repository](https://golang.org/dl/) and uncompress to `/usr/local` .
Alternativly you can use apt-get
```shell
$ sudo apt-get install golang-1.10-go
```

Add the following lines to the end of `$HOME/.bashrc` and source the file 
```shell
# add Go paths
export GOPATH=$HOME/go
export PATH=/usr/local/go/bin:$GOPATH/bin:$PATH
```

Install `Glide`
```shell
$ go get -u github.com/Masterminds/glide
```

## Installing the swap-resolver
You will need the swap-resolver to allow atomic swap across chains.  

To install the swap-resolver 
```shell
$ git clone https://github.com/offerm/swap-resolver.git $GOPATH/src/github.com/offerm/swap-resolver
```

## Build Lightning Daemon

Since cross chain swaps support was not yet merged into the official `lnd` master branch, we will use instead an xchain-swap enabled experimental `lnd` daemon. 

#### Cross-chain swap enabled `lnd`
To build and install the xchain-swap enabled experimental `lnd` daemon 

Make sure you use the resolver branch which is critical for the swap.
```shell
$ git clone -b resolver https://github.com/offerm/lnd.git $GOPATH/src/github.com/lightningnetwork/lnd
$ cd $GOPATH/src/github.com/lightningnetwork/lnd
$ make && make install
```

#### Bitcoin full node implementation `btcd`
If you don't have `btcd` set up, you will need to build and install the `btcd` Bitcoin full node implementation
```shell
$ git clone https://github.com/roasbeef/btcd $GOPATH/src/github.com/roasbeef/btcd
$ cd $GOPATH/src/github.com/roasbeef/btcd
$ glide install
$ go install . ./cmd/...
```

#### Litecoin full node implementation `ltcd`
If you don't have `ltcd` set up, you will need to build and install the `ltcd` Litecoin full node implementation
```shell
$ git clone https://github.com/ltcsuite/ltcd.git $GOPATH/src/github.com/ltcsuite/ltcd
$ cd $GOPATH/src/github.com/ltcsuite/ltcd
$ glide install
$ go install . ./cmd/...
```

## Setup Blockchain Clients

#### for Bitcoin
Running the following command will create `rpc.cert`  
Bitcoin testnet blockchain is downloaded to `$HOME/.btcd` by default
```shell
$ btcd --testnet --txindex --rpcuser=xu --rpcpass=xu
```

Sync progress may be tracked as follows
```shell
$ btcctl --testnet getinfo --rpcuser=xu --rpcpass=xu
```

#### for Litecoin
Running the following command will create `rpc.cert`  
Litecoin testnet blockchain is downloaded to `$HOME/.ltcd` by default
```shell
$ ltcd --testnet --txindex --rpcuser=xu --rpcpass=xu
```

Sync progress may be tracked as follows
```shell
$ ltcctl --testnet  getinfo --rpcuser=xu --rpcpass=xu
```

### Coffee time
It will take some time for BTCD and LTCD to sync with the chains. Get a coffee.


## Running Lightning Daemon(s)
Once testnet sync is done for the Bitcoin & Litecoin daemons, we continue to the next section which explains how to setup two lnd processes for Exchange-A and Exchange-B

[ [index](/README.md), [next](/LIGHTNING-01-peers.md) ]
