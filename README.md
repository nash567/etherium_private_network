# etherium_private_network


You can setup a private etherium network using Geth

## Geth
    Go Ethereum is one of the three original implementations (along with C++ and Python) of the Ethereum protocol. It is written in Go, fully open source and licensed under the GNU LGPL v3.
### We will have 3 nodes and we will connect these nodes using our boot key and perforrm transactions upon these nodes
***
## Steps for fresh start
create 3 directories node1 node2 node 3
```
mkdir node1 node2 node3
```

1. Create a genesis block using genesis.json
    - The Genesis.json file is a config file for Go-Ethereum (a blockchain node), that is needed in order to create a private blockchain network. It is the file that Go-Ethereum needs to create the very first block (Genesis block) of your private network.
2.  Create a bootkey using bootnode
```
 bootnode -genkey boot.key
```   
3. Run the bootnode and copy the enode address 

    !! example of enode is 
        enode://3218cd775739a6c035d483375cec202c4afece79e177f864d107677fc9ab7b73ea58910c43581130ce7e12463f346984aae9853828c19ff870969b4ddf27004e@127.0.0.1:0?discport=30305 
```
bootnode -nodekey boot.key -addr :30305
```

4. Now Create Accounts for all your nodes using 
our genesis.json file
```
geth account new --datadir node1/
```
this will create a keystore folder inside our node1 directory repeat this for all 3 nodes

5. Create a node using genesis file 

```
geth init --datadir node1/ genesis.json
```

repeat for all other directories

6. Starting our nodes and connect them to bootnode 
```
geth --datadir node3 --port 30308 --bootnodes enode://3218cd775739a6c035d483375cec202c4afece79e177f864d107677fc9ab7b73ea58910c43581130ce7e12463f346984aae9853828c19ff870969b4ddf27004e@127.0.0.1:0?discport=30305 --networkid 150 --unlock 0xfC9cA7FCB57dB520CCD42219a1F2134326dcaf2d --password password.txt --authrpc.port 8553
```

7. Now all our nodes are running and we are set to do transaction so open 3 terminals and attach our ipc file using geth it will open a web3 js console in the terminal so that we can interact with the nodes 

```
 geth attach node1/geth.ipc
```

## Steps to do Transactions in the web3js console 
After attaching our terminal to the web3js console we can 
1. Check for accounts using 
```
eth.accounts
```
2. we can check the account balance using
```
eth.getBalance(eth.accounts[0])
```
3. Send some ether to another account

    Private transactions are transactions sent directly to miners (Proof-of-Work), or validators / block proposers (Proof-of-Stake), instead of public mempools, and is one way traders can avoid exposing their transactions to frontrunning attacks. A mempool is a shared pool of pending transactions that is known by all nodes on Ethereum’s peer-to-peer network. Pooled transactions are chosen to be included in the next block by a miner. 
```
> eth.sendTransaction({from: ACCOUNT_node1_STRING, to: ACCOUNT_node2_STRING, value: 5000})

```

4. Now we can check he transaction using the hash returned by previous command using

```
eth.getTransaction("hash")
```
5. Now after the transaction is done it is added to the pending transaction till other nodes mine it and verify it then it will be added to the block so you can start mining from other account using 
```
miner.start()
```

6. Check the pending transaction using 
```
eth.pendingTransactions()
```
Now we have done a transaction you can verify it by checking the balances of the accounts you have transacted on