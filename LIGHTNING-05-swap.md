[ [index](/README.md), [previous](/LIGHTNING-04-payment.md) ]

# Lightning Cross-Chain Swap

Now that we have everything setup, we can execute swaps.

## Balance before the swap

Before we execute the swap lets record the BTC and LTC channel balances on exchange A and B (check `local_balance` and `remote_balance`)

```shell
$ xa-lnd-btc listchannels
{
    "channels": [
        {
            "active": true,
            "remote_pubkey": "022b74059a18bb77c6c906377e92023cc40ae9695920df0d056fc95f135221e69f",
            "channel_point": "246c05860d1589898f9a22317915c935b021a198ae93c1eacc6fc835ca96b5ac:0",
            "chan_id": "1513035751963164672",
            "capacity": "16000000",
            "local_balance": "12977556",
            "remote_balance": "3000000",
            "commit_fee": "22444",
            "commit_weight": "724",
            "fee_per_kw": "31000",
            "unsettled_balance": "0",
            "total_satoshis_sent": "0",
            "total_satoshis_received": "0",
            "num_updates": "0",
            "pending_htlcs": [
            ],
            "csv_delay": 1922,
            "private": false
        }
    ]
}
$ xa-lnd-ltc listchannels
{
    "channels": [
        {
            "active": true,
            "remote_pubkey": "021be8d225008d415eaa9b64da60926dfc197e9139deb0f09bae09603352878b5c",
            "channel_point": "e217a814b2f360050b6548acf042c7375d78588aeeee91fc44096d24174dd724:0",
            "chan_id": "757450261840068608",
            "capacity": "10000000",
            "local_balance": "4995475",
            "remote_balance": "5000000",
            "commit_fee": "4525",
            "commit_weight": "724",
            "fee_per_kw": "6250",
            "unsettled_balance": "0",
            "total_satoshis_sent": "0",
            "total_satoshis_received": "0",
            "num_updates": "0",
            "pending_htlcs": [
            ],
            "csv_delay": 576,
            "private": false
        }
    ]
}
```


## Now swap

Lets assume exchange B is the maker and exchange A is the Taker.
The Maker is willing to sell 10000 LTC for 100 BTC

The command is executed against the taker XUD. 

```shell
$ cd $GOPATH/src/github.com/offerm/swap-resolver

$go run cmd/xucli/main.go --rpcserver localhost:7001 takeorder --order_id=123 --maker_amount 100 --maker_coin BTC --taker_amount 10000 --taker_coin=LTC

2018/08/03 20:57:35 Starting takeOrder command -  (*swapresolver.TakeOrderReq)(0xc4200a5c80)(orderid:"123" taker_amount:10000 taker_coin:LTC maker_amount:100 )
2018/08/03 20:57:36 Swap completed successfully.
  Swap preImage is  c7256829d2a731c76f98a08fae6f40965fc642d417d8c8249d0d560830d93217 
```

## Balance after the swap

Let's see the impact in the channels (check `local_balance` and `remote_balance`)
```shell
$ xa-lnd-btc listchannels
{
    "channels": [
        {
            "active": true,
            "remote_pubkey": "022b74059a18bb77c6c906377e92023cc40ae9695920df0d056fc95f135221e69f",
            "channel_point": "246c05860d1589898f9a22317915c935b021a198ae93c1eacc6fc835ca96b5ac:0",
            "chan_id": "1513035751963164672",
            "capacity": "16000000",
            "local_balance": "12977456",
            "remote_balance": "3000100",
            "commit_fee": "22444",
            "commit_weight": "724",
            "fee_per_kw": "31000",
            "unsettled_balance": "0",
            "total_satoshis_sent": "100",
            "total_satoshis_received": "0",
            "num_updates": "2",
            "pending_htlcs": [
            ],
            "csv_delay": 1922,
            "private": false
        }
    ]
}
$ xa-lnd-ltc listchannels
{
    "channels": [
        {
            "active": true,
            "remote_pubkey": "021be8d225008d415eaa9b64da60926dfc197e9139deb0f09bae09603352878b5c",
            "channel_point": "e217a814b2f360050b6548acf042c7375d78588aeeee91fc44096d24174dd724:0",
            "chan_id": "757450261840068608",
            "capacity": "10000000",
            "local_balance": "5005475",
            "remote_balance": "4990000",
            "commit_fee": "4525",
            "commit_weight": "724",
            "fee_per_kw": "6250",
            "unsettled_balance": "0",
            "total_satoshis_sent": "0",
            "total_satoshis_received": "10000",
            "num_updates": "2",
            "pending_htlcs": [
            ],
            "csv_delay": 576,
            "private": false
        }
    ]
}
```

100 Sat more, 10000 Lat less.

Done!!!!

[ [index](/README.md), [previous](/LIGHTNING-04-payment.md) ]
