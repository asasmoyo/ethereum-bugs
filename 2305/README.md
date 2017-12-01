BUG #2305
=========

Idea to reproduce:

1. Run 2 nodes.

    - `node1` becomes normal node, it just do synchronication

    - `node2` becomes a miner, it will create a block with timestamp + 10 seconds to make it a future block

2. `node2` create a block, set timestamp + 10 seconds, send it to `node1`. `node2` then wait until `node1` gives signal to send the same block again by reading file at `ipc/got_hash`. When the file exists `node2` will send the same block again to `node1`.

3. `node1` wait `node2` to send its block. Once it received it will be inserted to future blocks (might need some modification to make so). Once it received, `node1` will wait for future blocks processing runs for every 5 seconds. When `node1` takes the hash of future blocks in the loop, it will send ipc by writing file at `ipc/got_hash`, then wait until `node2` broadcast the same block again. `node1` also wait for signal that indicating the block has been processed by trying to read `ipc/block_processed`. When it does `node1` will try to get block by hash. But since it is already taken out from the future blocks, it will returns nil. Then `node1` tries to insert nil block, then panic.

Things to do:

1. Need to make `node2` to just broadcast a custom block, wait for a signal, then broadcast it again.

2. Need to make `node1` wait for a signal before getting a future blocks by hash. Also need to write a signal whenever a block is successfully processed.
