# dash-testnet-box

Create your own private DASH testnet

Note: This was heavily borrowed from: https://github.com/freewil/bitcoin-testnet-box .
It is assumed that you are running this on 64 bit linux. (tested on Ubuntu 16.10)

You must have `dashd` and `dash-cli` installed on your system and modify the top of the
Makefile, or you can run the `make download` option.

## Starting the testnet-box

This will start up two nodes using the two datadirs `1` and `2`. They
will only connect to each other in order to remain an isolated private testnet.
Two nodes are provided, as one is used to generate blocks and it's balance
will be increased as this occurs (imitating a miner). You may want a second node
where this behavior is not observed.

Node `1` will listen on port `19000`, allowing node `2` to connect to it.

Node `1` will listen on port `19001` and node `2` will listen on port `19011`
for the JSON-RPC server.


## To download the DASH v12.1 binaries.
```
$ make download
```

## To start the DASH processes
```
$ make start
```

## Check the status of the nodes

```
$ make getinfo
{
  "version": 120100,
  "protocolversion": 70206,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 0,
  "timeoffset": 0,
  "connections": 1,
  "proxy": "",
  "difficulty": 4.656542373906925e-10,
  "testnet": false,
  "keypoololdest": 1486515110,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
/root/dashcore/dashcore-0.12.1/bin/dash-cli -datadir=2  getinfo
{
  "version": 120100,
  "protocolversion": 70206,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 0,
  "timeoffset": 0,
  "connections": 1,
  "proxy": "",
  "difficulty": 4.656542373906925e-10,
  "testnet": false,
  "keypoololdest": 1486515110,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
```

## Generating blocks

Normally on the live, real, DASH network, blocks are generated, on average,
every 2.5 minutes. Since this testnet-in-box uses DASH Core's (dashd)
regtest mode, we are able to generate a block on a private network
instantly using a simple command.

To generate a block:

```
$ make generate
```

To generate more than 1 block:

```
$ make generate BLOCKS=10
```

## Need to generate at least 100 blocks before there will be a balance in the first wallet
```
$ make generate BLOCKS=100
```

## Verify that there is a balance on the first wallet
```
$ make getinfo
```

## Generate a wallet address for the second wallet
```
$ make address2
```

## Sending DASH
To send DASH that you've generated to the second wallet: (be sure to change the ADDRESS value below to wallet address generated in the prior command)

```
$ make sendfrom1 ADDRESS=ygWoB2qkP3SKrdSWUeWVirhd5uN8bAHYax AMOUNT=10
```

## Does the balance show up?
Run the getinfo command again. Does the balance show up? Why not?
```
$ make getinfo
```

## Generate another block
```
$ make generate
```

## Stopping the testnet-box

```
$ make stop
```

To clean up any files created while running the testnet and restore to the
original state:

```
$ make clean
```

