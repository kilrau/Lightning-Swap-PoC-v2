[ [index](/README.md), [previous](/LIGHTNING-02-connect.md), [next](/LIGHTNING-04-payment.md) ]

# Lightning Payment Channels

## Exchange A opens 0.16 BTC payment channel to Exchange B pushing 0.3 BTC to exchange B

```shell
$ xa-lnd-btc openchannel --node_key=$XB_BTC_PUBKEY --local_amt=16000000 --push_amt=3000000
{
        "funding_txid": "1f40907fc1968319cbb57955e06c7b11d4f3b9d413c633c1ca26288b9d2e033b"
}
```

The funding transaction must be confirmed before a channel is opened. The default number of confirmations is 1.

Until confirmed, the pending channels can be seen with the `pendingchannels` command
```shell
$ xa-lnd-btc openchannel pendingchannels --ticker=BTC
```

Once the channel is opened, Exchange A lists the `Bitcoin` payment channel as follows
```shell
$ xa-lnd-btc listchannels

ADD HERE

```



## Exchange A opens a 0.1 LTC payment channel to Exchange B and pushes 0.05 LTC to Exchange B

$ xa-lnd-ltc openchannel --node_key=$XB_LTC_PUBKEY --local_amt=10000000 --push_amt=5000000 
{
        "funding_txid": "3c5b1d738e251819f0eaf263e73eb268e73a2d231e5da00cdfada76b3c66e8f7"
}
```

Until confirmed, Exchange B list the new channel as pending channel.
```shell
$ xb-lnd-ltc pendingchannels

ADD HERE

```

Once confirmed, Exchange B lists the `Litecoin` payment channel as follows
```shell
$ xb-lnd-ltc listchannels

ADD HERE
```

## Let's move on!

[ [index](/README.md), [previous](/LIGHTNING-02-connect.md), [next](/LIGHTNING-04-payment.md) ]

