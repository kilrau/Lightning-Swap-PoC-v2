[ [index](/README.md), [previous](/LIGHTNING-01-peers.md), [next](/LIGHTNING-03-channels.md) ]

# Lightning Peer Connection
In this step we connect Exchange A and Exchange B at network levels so they become peers of each other. We make 2 parallel P2P networks, one for BTC and one for LTC.    



## Establishing Connection
First, we extract exchange B PUBKEYs so we can use them to set connections and channels
```shell
$ XB_BTC_PUBKEY=`xb-lnd-btc getinfo|grep identity_pubkey|cut -d '"' -f 4`
$ XB_LTC_PUBKEY=`xb-lnd-ltc getinfo|grep identity_pubkey|cut -d '"' -f 4`
```


By using Exchange B's `identity_pubkey`, host and port number, Exchange A establishes connections using the following commands
```shell
$ xa-lnd-btc connect $XB_BTC_PUBKEY@127.0.0.1:20012
{
    "peer_id": 0
}

$ xa-lnd-ltc connect $XB_LTC_PUBKEY@127.0.0.1:20011
{
    "peer_id": 0
}
```

### Exchange A post-connection
```shell
$ xa-lnd-btc listpeers
{
    "peers": [
        {
            "pub_key": "02fe35d3c066601c5e9f3f5de0972e0c0b3478661544c0d0eb6991355c3e676926",
            "address": "127.0.0.1:62115",
            "bytes_sent": "792",
            "bytes_recv": "792",
            "sat_sent": "17004",
            "sat_recv": "0",
            "inbound": true,
            "ping_time": "618"
        }
    ]
}

$ xa-lnd-ltc listpeers
{
    "peers": [
        {
            "pub_key": "02ac744ae30f0e7c1d624221428e7f015ee921e557ef2c6199b7dd983a85d2b014",
            "address": "127.0.0.1:20011",
            "bytes_sent": "792",
            "bytes_recv": "792",
            "sat_sent": "0",
            "sat_recv": "1700006",
            "inbound": false,
            "ping_time": "465"
        }
    ]
}

```

We are no ready to set up payment channels. 

[ [index](/README.md), [previous](/LIGHTNING-01-peers.md), [next](/LIGHTNING-03-channels.md) ]
