BUG #2305
=========

1. There are 3 nodes (miner, node1, node2)

2. Miner mines a future block with timestamp +20

3. Miner broadcast the block to node1 and node2

    https://github.com/asasmoyo/ethereum-bugs/blob/master/2305/node0/cmd/geth/main.go#L419

4. Node1 receives the block then put it into future blocks list. The worker ticks then waits until it gets block_processed signal

    https://github.com/asasmoyo/ethereum-bugs/blob/master/2305/node1/core/blockchain.go#L706

5. Node2 will processes the block after several attempts. It then broadcast the block

    https://github.com/asasmoyo/ethereum-bugs/blob/master/2305/node2/cmd/geth/main.go#L420

6. Node 1 receives the same block, processes it, removes it from future block list then create block_processed signal

    https://github.com/asasmoyo/ethereum-bugs/blob/master/2305/node1/core/blockchain.go#L1277

7. The worker in node 1 continues and crash
