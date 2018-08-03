[ [index](/README.md), [previous](/LIGHTNING-01-peers.md), [next](/LIGHTNING-03-channels.md) ]

# Lightning Peer Connection
In this step we connect Exchange A and Exchange B at network levels so they become peers of each other. We make 2 parallel P2P networks, one for BTC and one for LTC.    



## Establishing Connection
```shell
$ XB_BTC_PUBKEY=`xb-lnd-btc getinfo|grep identity_pubkey|cut -d '"' -f 4`
$ XB_LTC_PUBKEY=`xb-lnd-ltc getinfo|grep identity_pubkey|cut -d '"' -f 4`

$ lncli --rpcserver=localhost:10002 --no-macaroons connect $X_A_ID_PUBKEY@127.0.0.1:10011
{
    "peer_id": 0
}
```


By using Exchange A's `identity_pubkey`, host and port number, Exchange B establishes a connection using the following command
```shell
$ X_A_ID_PUBKEY=`lncli --rpcserver=localhost:10001 --no-macaroons getinfo|grep identity_pubkey|cut -d '"' -f 4`

$ lncli --rpcserver=localhost:10002 --no-macaroons connect $X_A_ID_PUBKEY@127.0.0.1:10011
{
    "peer_id": 0
}
```

### Exchange A post-connection
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons listpeers
{
        "peers": [
                {
                        "pub_key": "0237cdf6b03cf17df8676af35b43da3ee0613b888bc5cd26a41064118f1241cc2f",
                        "peer_id": 1,
                        "address": "127.0.0.1:56624",
                        "bytes_sent": "149",
                        "bytes_recv": "149",
                        "sat_sent": "0",
                        "sat_recv": "0",
                        "inbound": false,
                        "ping_time": "0"
                }
        ]
}
```

### Exchange B post-connection
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons listpeers
{
        "peers": [
                {
                        "pub_key": "026374581ff7974975ffce20e65a04876ba33405502d1a13dc73c9a702b61aef31",
                        "peer_id": 1,
                        "address": "127.0.0.1:10011",
                        "bytes_sent": "149",
                        "bytes_recv": "149",
                        "sat_sent": "0",
                        "sat_recv": "0",
                        "inbound": true,
                        "ping_time": "0"
                }
        ]
}
```

We are no ready to set up payment channels. 

[ [index](/README.md), [previous](/LIGHTNING-01-peers.md), [next](/LIGHTNING-03-channels.md) ]
