[ [index](/README.md), [previous](/LIGHTNING-04-payment.md) ]

# Lightning Cross-Chain Swap

Now that we have everything setup, we can execute swaps.

## Balance before the swap

Before we execute the swap lets record the BTC and LTC channel balances on exchange A and B (check `local_balance` and `remote_balance`)

```shell
```


## Now swap

Lets assume exchange B is the maker and exchange A is the Taker.
The Maker is willing to sell 10000 LTC for 100 BTC

The command is executed against the taker XUD. 

```shell
$ cd $GOPATH/src/github.com/offerm/swap-resolver

$go run cmd/xucli/main.go --rpcserver localhost:7001 takeorder --order_id=123 --maker_amount 100 --maker_coin BTC --taker_amount 10000 --taker_coin=LTC

2018/08/03 13:04:07 Starting takeOrder command -  (*swapresolver.TakeOrderReq)(0xc4200a9c80)(orderid:"123" taker_amount:10000 taker_coin:LTC maker_amount:100 )
2018/08/03 13:04:08 Swap completed successfully.
  Swap preImage is  390eea9e6a67d1506bc4c334ed35c3ac3d84c6e856019424740bc52d16490b6e 
```

## Balance after the swap

Let's see the impact in the channels (check `local_balance` and `remote_balance`)
```shell
$  lncli --rpcserver=localhost:10001 --no-macaroons listchannels
{
        "channels": [
                {
                        "active": true,
                        "remote_pubkey": "0237cdf6b03cf17df8676af35b43da3ee0613b888bc5cd26a41064118f1241cc2f",
                        "channel_point": "1f40907fc1968319cbb57955e06c7b11d4f3b9d413c633c1ca26288b9d2e033b:0",
                        "chan_id": "1450271230199529472",
                        "capacity": "16000000",
                        "local_balance": "15895312",
                        "remote_balance": "96000",
                        "commit_fee": "8688",
                        "commit_weight": "724",
                        "fee_per_kw": "12000",
                        "unsettled_balance": "0",
                        "total_satoshis_sent": "101000",
                        "total_satoshis_received": "5000",
                        "num_updates": "8",
                        "pending_htlcs": []
                },
                {
                        "active": true,
                        "remote_pubkey": "0237cdf6b03cf17df8676af35b43da3ee0613b888bc5cd26a41064118f1241cc2f",
                        "channel_point": "3c5b1d738e251819f0eaf263e73eb268e73a2d231e5da00cdfada76b3c66e8f7:0",
                        "chan_id": "649885039294873600",
                        "capacity": "10000000",
                        "local_balance": "5063800",
                        "remote_balance": "4900000",
                        "commit_fee": "36200",
                        "commit_weight": "724",
                        "fee_per_kw": "50000",
                        "unsettled_balance": "0",
                        "total_satoshis_sent": "0",
                        "total_satoshis_received": "100000",
                        "num_updates": "2",
                        "pending_htlcs": []
                }
        ]
}
```

Done!!!!

[ [index](/README.md), [previous](/LIGHTNING-04-payment.md) ]
