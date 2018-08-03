[ [index](/README.md), [previous](/LIGHTNING-00-install.md), [next](/LIGHTNING-02-connect.md) ]

# Lightning nodes Setup

Once we installed all components and we have Litecoin and Bitcoin synced with their blockchains we can set up `lnd` processes. We are going to setup two `lnd` processes for each exchange that we mimic (one for BTC and one for LTC). It will take the `lnd` processes some time to sync with the `btcd` and `ltcd` daemons. It is OK to setup the four `lnd`s in parallel.

## aliases
To make our life easier with lncli, it is recommanded to use aliases.

```bash
alias xa-lnd-btc='lncli --rpcserver=localhost:10002 '
alias xa-lnd-ltc='lncli --rpcserver=localhost:10001 '
alias xb-lnd-btc='lncli --rpcserver=localhost:20001 '
alias xb-lnd-ltc='lncli --rpcserver=localhost:20001 '
```

`xb=lnd-btc` allows CLI command to exchange B LND for the BTC network

Add the aliases file from `$GOPATH/src/github.com/lofferm/swap-resolver` to ~/.bash_profile and source it. Now we can use these aliases to communicate with the 4 `lnd` processes without the need to type the needed CLI arguments. 

## startup scripts
To make life easier we prepared a directory structure under $GOPATH/src/github.com/lofferm/swap-resolver as follow:

$GOPATH/src/github.com/lofferm/swap-resolver
*	exchange-a
	+	lnd (resolve.conf)
		*	btc (start.bash)
		*	ltc (start.bash)
	+	xud (start.bash)
*	exchange-b
	+	lnd (resolve.conf)
		*	btc (start.bash)
		*	ltc (start.bash)
	+	xud (start.bash)

the `start.bash` script invokes the LND process using the right parameters (ports, etc).
the `resolve.conf` is needed for the swap-resolver 

## Exchange A
### Launch `lnd-btc`
Open a terminal to set Exchange A's `lnd-btc` daemon
```shell
cd $GOPATH/src/github.com/lofferm/swap-resolver/exchange-a/lnd/btc/
./start.bash
```

check progress with
```shell
xa-lnd-btc getinfo
```
### Launch `lnd-ltc`
Open a terminal to set Exchange A's `lnd-ltc` daemon
```shell
cd $GOPATH/src/github.com/lofferm/swap-resolver/exchange-a/lnd/ltc/
./start.bash
```

check progress with
```shell
xa-lnd-ltc getinfo
```

## Exchange B
### Launch `lnd-btc`
Open a terminal to set Exchange B's `lnd-btc` daemon
```shell
cd $GOPATH/src/github.com/lofferm/swap-resolver/exchange-b/lnd/btc/
./start.bash
```

check progress with
```shell
xb-lnd-btc getinfo
```
### Launch `lnd-ltc`
Open a terminal to set Exchange B's `lnd-ltc` daemon
```shell
cd $GOPATH/src/github.com/lofferm/swap-resolver/exchange-b/lnd/ltc/
./start.bash
```

check progress with
```shell
xb-lnd-ltc getinfo
```


## Wait until Exchange A and Exchange B are synced

Give the four `lnd` the time they need to sync with `btcd` and `ltcd`. You can check the status by using the `getinfo` command (use the cli terminal for this). You would want to see `"synced_to_chain": true,` for all process exchanges.

### Check status 

example - Check status of Exchange A

```shell
$ xa-lnd-btc getinfo
{
    "identity_pubkey": "035e3d3884e9a26dcd238b2b2d1ef608a31888365fed327b4af563671a2ee49bb6",
    "alias": "035e3d3884e9a26dcd23",
    "num_pending_channels": 0,
    "num_active_channels": 1,
    "num_peers": 1,
    "block_height": 1356729,
    "block_hash": "00000000007dd1ef9454e52b3986b939ed91b7f81587c95ff0921977be316114",
    "synced_to_chain": true,
    "testnet": true,
    "chains": [
        "bitcoin"
    ],
    "uris": [
    ],
    "best_header_timestamp": "1533285232",
    "version": "0.4.2-beta commit=6a8368a632d22762299686f7ea94eb777a5887c8"
}
```

Do that for all LNDs until you see all of them synced to chain.

# Fund Exchange A by using faucets 

## balance after creating

Query Exchange A wallet balances for both `Bitcoin` and `Litecoin` after creation (we expect to see zeros, right?)
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "0"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "0"
}
```

## Create BTC and LTC addresses for deposit

Create Exchange A Segwit addresses for both `Bitcoin` and `Litecoin`
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons newaddress np2wkh --ticker=BTC
{
        "address": "2NBRavXuXmbd73tRvgCAHhTiSuPoa3LqKdd"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons newaddress np2wkh --ticker=LTC
{
        "address": "2N2yE6ZbdxePtco8DuTSQVEJNsNg74KvGd3"
}
```

## Use addresses to fund Exchange A's wallets

Send some BTC (0.2 or more is great) and some LTC (10 is great) to Exchange A's addresses via testnet faucets (see [README.bitcoin](README.bitcoin.md/#bitcoin-testnet-faucet) and [README.litecoin](README.litecoin.md/#litecoin-testnet-faucet)). Balances should appear in the wallet once the BTC/LTC transactions are mined.

## balance after funding the wallets

Query Exchange A wallet balances for both `Bitcoin` and `Litecoin` after funding
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC
{
        "balance": "32500000"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
        "balance": "1000000000"
}
```



We are now ready with Exchange A's wallets. Note that Exchange B has LTC and BTC wallets but these are empty (zero balance). There is no need to fund Exchange B for our PoC. 

We are now ready to move on to the next step and create the P2P `lnd` network.

[ [index](/README.md), [previous](/LIGHTNING-00-install.md), [next](/LIGHTNING-02-connect.md) ]
