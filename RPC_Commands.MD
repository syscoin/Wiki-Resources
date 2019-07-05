## Table of Contents
 
[Blockchain](#blockchain)  
[Control](#control)  
[Generating](#generating)  
[Governance](#governance)  
[Governancewallet](#governancewallet)  
[Mining](#mining)  
[Network](#network)  
[Rawtransactions](#rawtransactions)  
[Syscoin](#syscoin)  
[Syscoinwallet](#syscoinwallet)  
[Utilities](#utilities)  
[Wallet](#wallet)  
[Zmq](#zmq)  

## Blockchain

<details><summary>getbestblockhash</summary>

```

Returns the hash of the best (tip) block in the longest blockchain.

Result:
"hex"      (string) the block hash, hex-encoded

Examples:
> syscoin-cli getbestblockhash 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getbestblockhash", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblock "blockhash" ( verbosity )</summary>

```

If verbosity is 0, returns a string that is serialized, hex-encoded data for block 'hash'.
If verbosity is 1, returns an Object with information about block <hash>.
If verbosity is 2, returns an Object with information about block <hash> and information about each transaction. 

Arguments:
1. blockhash    (string, required) The block hash
2. verbosity    (numeric, optional, default=1) 0 for hex-encoded data, 1 for a json object, and 2 for json object with transaction data

Result (for verbosity = 0):
"data"             (string) A string that is serialized, hex-encoded data for block 'hash'.

Result (for verbosity = 1):
{
  "hash" : "hash",     (string) the block hash (same as provided)
  "confirmations" : n,   (numeric) The number of confirmations, or -1 if the block is not on the main chain
  "size" : n,            (numeric) The block size
  "strippedsize" : n,    (numeric) The block size excluding witness data
  "weight" : n           (numeric) The block weight as defined in BIP 141
  "height" : n,          (numeric) The block height or index
  "version" : n,         (numeric) The block version
  "versionHex" : "00000000", (string) The block version formatted in hexadecimal
  "merkleroot" : "xxxx", (string) The merkle root
  "tx" : [               (array of string) The transaction ids
     "transactionid"     (string) The transaction id
     ,...
  ],
  "time" : ttt,          (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "mediantime" : ttt,    (numeric) The median block time in seconds since epoch (Jan 1 1970 GMT)
  "nonce" : n,           (numeric) The nonce
  "bits" : "1d00ffff", (string) The bits
  "difficulty" : x.xxx,  (numeric) The difficulty
  "chainwork" : "xxxx",  (string) Expected number of hashes required to produce the chain up to this block (in hex)
  "nTx" : n,             (numeric) The number of transactions in the block.
  "previousblockhash" : "hash",  (string) The hash of the previous block
  "nextblockhash" : "hash"       (string) The hash of the next block
  "auxpow" : {           (object) The auxpow object attached to this block
    "tx" : {...},        (object) The parent chain coinbase tx of this auxpow
    "index" : 0,         (numeric) Merkle index of the parent coinbase
    "merklebranch" : [...], (array) Merke branch of the parent coinbase
    "chainindex" : n,    (numeric) Index in the auxpow Merkle tree
    "chainmerklebranch" : [...], (array) Branch in the auxpow Merkle tree
    "parentblock" : "xx" (string) The parent block serialised as hex string
  }
}

Result (for verbosity = 2):
{
  ...,                     Same output as verbosity = 1.
  "tx" : [               (array of Objects) The transactions in the format of the getrawtransaction RPC. Different from verbosity = 1 "tx" result.
         ,...
  ],
  ,...                     Same output as verbosity = 1.
}

Examples:
> syscoin-cli getblock "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblock", "params": ["00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockchaininfo</summary>

```
Returns an object containing various state info regarding blockchain processing.

Result:
{
  "chain": "xxxx",              (string) current network name as defined in BIP70 (main, test, regtest)
  "blocks": xxxxxx,             (numeric) the current number of blocks processed in the server
  "headers": xxxxxx,            (numeric) the current number of headers we have validated
  "bestblockhash": "...",       (string) the hash of the currently best block
  "difficulty": xxxxxx,         (numeric) the current difficulty
  "mediantime": xxxxxx,         (numeric) median time for the current best block
  "verificationprogress": xxxx, (numeric) estimate of verification progress [0..1]
  "initialblockdownload": xxxx, (bool) (debug information) estimate of whether this node is in Initial Block Download mode.
  "chainwork": "xxxx"           (string) total amount of work in active chain, in hexadecimal
  "size_on_disk": xxxxxx,       (numeric) the estimated size of the block and undo files on disk
  "pruned": xx,                 (boolean) if the blocks are subject to pruning
  "pruneheight": xxxxxx,        (numeric) lowest-height complete block stored (only present if pruning is enabled)
  "automatic_pruning": xx,      (boolean) whether automatic pruning is enabled (only present if pruning is enabled)
  "prune_target_size": xxxxxx,  (numeric) the target size used by pruning (only present if automatic pruning is enabled)
  "geth_sync_status": xxxxxx,   (numeric) Sync status of Geth (Ethereum network). Valid values are: synced, syncing or waiting to sync...
  "geth_total_blocks": xxxxxx,  (numeric) The highest height seen on the Ethereum network by Geth
  "geth_current_block": xxxxxx, (numeric) The highest height of the block header of Ethereum that is stored in our database
  "softforks": [                (array) status of softforks in progress
     {
        "id": "xxxx",           (string) name of softfork
        "version": xx,          (numeric) block version
        "reject": {             (object) progress toward rejecting pre-softfork blocks
           "status": xx,        (boolean) true if threshold reached
        },
     }, ...
  ],
  "bip9_softforks": {           (object) status of BIP9 softforks in progress
     "xxxx" : {                 (string) name of the softfork
        "status": "xxxx",       (string) one of "defined", "started", "locked_in", "active", "failed"
        "bit": xx,              (numeric) the bit (0-28) in the block version field used to signal this softfork (only for "started" status)
        "startTime": xx,        (numeric) the minimum median time past of a block at which the bit gains its meaning
        "timeout": xx,          (numeric) the median time past of a block at which the deployment is considered failed if not yet locked in
        "since": xx,            (numeric) height of the first block to which the status applies
        "statistics": {         (object) numeric statistics about BIP9 signalling for a softfork (only for "started" status)
           "period": xx,        (numeric) the length in blocks of the BIP9 signalling period 
           "threshold": xx,     (numeric) the number of blocks with the version bit set required to activate the feature 
           "elapsed": xx,       (numeric) the number of blocks elapsed since the beginning of the current period 
           "count": xx,         (numeric) the number of blocks with the version bit set in the current period 
           "possible": xx       (boolean) returns false if there are not enough blocks left in this period to pass activation threshold 
        }
     }
  }
  "warnings" : "...",           (string) any network and blockchain warnings.
}

Examples:
> syscoin-cli getblockchaininfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockchaininfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockcount</summary>

```

Returns the number of blocks in the longest blockchain.

Result:
n    (numeric) The current block count

Examples:
> syscoin-cli getblockcount 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockcount", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockfilter "blockhash" ( "filtertype" )</summary>

```

Retrieve a BIP 157 content filter for a particular block.

Arguments:
1. blockhash     (string, required) The hash of the block
2. filtertype    (string, optional, default=basic) The type name of the filter

Result:
{
  "filter" : (string) the hex-encoded filter data
  "header" : (string) the hex-encoded filter header
}

Examples:
> syscoin-cli getblockfilter "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09" "basic"
```
</details>

<details><summary>getblockhash height</summary>

```

Returns hash of block in best-block-chain at height provided.

Arguments:
1. height    (numeric, required) The height index

Result:
"hash"         (string) The block hash

Examples:
> syscoin-cli getblockhash 1000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockhash", "params": [1000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockheader "blockhash" ( verbose )</summary>

```

If verbose is false, returns a string that is serialized, hex-encoded data for blockheader 'hash'.
If verbose is true, returns an Object with information about blockheader <hash>.

Arguments:
1. blockhash    (string, required) The block hash
2. verbose      (boolean, optional, default=true) true for a json object, false for the hex-encoded data

Result (for verbose = true):
{
  "hash" : "hash",     (string) the block hash (same as provided)
  "confirmations" : n,   (numeric) The number of confirmations, or -1 if the block is not on the main chain
  "height" : n,          (numeric) The block height or index
  "version" : n,         (numeric) The block version
  "versionHex" : "00000000", (string) The block version formatted in hexadecimal
  "merkleroot" : "xxxx", (string) The merkle root
  "time" : ttt,          (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "mediantime" : ttt,    (numeric) The median block time in seconds since epoch (Jan 1 1970 GMT)
  "nonce" : n,           (numeric) The nonce
  "bits" : "1d00ffff", (string) The bits
  "difficulty" : x.xxx,  (numeric) The difficulty
  "chainwork" : "0000...1f3"     (string) Expected number of hashes required to produce the current chain (in hex)
  "nTx" : n,             (numeric) The number of transactions in the block.
  "previousblockhash" : "hash",  (string) The hash of the previous block
  "nextblockhash" : "hash",      (string) The hash of the next block
}

Result (for verbose=false):
"data"             (string) A string that is serialized, hex-encoded data for block 'hash'.

Examples:
> syscoin-cli getblockheader "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockheader", "params": ["00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockstats hash_or_height ( stats )</summary>

```

Compute per block statistics for a given window. All amounts are in satoshis.
It won't work for some heights with pruning.

Arguments:
1. hash_or_height    (string or numeric, required) The block hash or height of the target block
2. stats             (json array, optional, default=all values) Values to plot (see result below)
     [
       "height",     (string) Selected statistic
       "time",       (string) Selected statistic
       ...
     ]

Result:
{                           (json object)
  "avgfee": xxxxx,          (numeric) Average fee in the block
  "avgfeerate": xxxxx,      (numeric) Average feerate (in satoshis per virtual byte)
  "avgtxsize": xxxxx,       (numeric) Average transaction size
  "blockhash": xxxxx,       (string) The block hash (to check for potential reorgs)
  "feerate_percentiles": [  (array of numeric) Feerates at the 10th, 25th, 50th, 75th, and 90th percentile weight unit (in satoshis per virtual byte)
      "10th_percentile_feerate",      (numeric) The 10th percentile feerate
      "25th_percentile_feerate",      (numeric) The 25th percentile feerate
      "50th_percentile_feerate",      (numeric) The 50th percentile feerate
      "75th_percentile_feerate",      (numeric) The 75th percentile feerate
      "90th_percentile_feerate",      (numeric) The 90th percentile feerate
  ],
  "height": xxxxx,          (numeric) The height of the block
  "ins": xxxxx,             (numeric) The number of inputs (excluding coinbase)
  "maxfee": xxxxx,          (numeric) Maximum fee in the block
  "maxfeerate": xxxxx,      (numeric) Maximum feerate (in satoshis per virtual byte)
  "maxtxsize": xxxxx,       (numeric) Maximum transaction size
  "medianfee": xxxxx,       (numeric) Truncated median fee in the block
  "mediantime": xxxxx,      (numeric) The block median time past
  "mediantxsize": xxxxx,    (numeric) Truncated median transaction size
  "minfee": xxxxx,          (numeric) Minimum fee in the block
  "minfeerate": xxxxx,      (numeric) Minimum feerate (in satoshis per virtual byte)
  "mintxsize": xxxxx,       (numeric) Minimum transaction size
  "outs": xxxxx,            (numeric) The number of outputs
  "subsidy": xxxxx,         (numeric) The block subsidy
  "swtotal_size": xxxxx,    (numeric) Total size of all segwit transactions
  "swtotal_weight": xxxxx,  (numeric) Total weight of all segwit transactions divided by segwit scale factor (4)
  "swtxs": xxxxx,           (numeric) The number of segwit transactions
  "time": xxxxx,            (numeric) The block time
  "total_out": xxxxx,       (numeric) Total amount in all outputs (excluding coinbase and thus reward [ie subsidy + totalfee])
  "total_size": xxxxx,      (numeric) Total size of all non-coinbase transactions
  "total_weight": xxxxx,    (numeric) Total weight of all non-coinbase transactions divided by segwit scale factor (4)
  "totalfee": xxxxx,        (numeric) The fee total
  "txs": xxxxx,             (numeric) The number of transactions (excluding coinbase)
  "utxo_increase": xxxxx,   (numeric) The increase/decrease in the number of unspent outputs
  "utxo_size_inc": xxxxx,   (numeric) The increase/decrease in size for the utxo index (not discounting op_return and similar)
}

Examples:
> syscoin-cli getblockstats 1000 '["minfeerate","avgfeerate"]'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockstats", "params": [1000 '["minfeerate","avgfeerate"]'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getchaintips</summary>

```
Return information about all known tips in the block tree, including the main chain as well as orphaned branches.

Result:
[
  {
    "height": xxxx,         (numeric) height of the chain tip
    "hash": "xxxx",         (string) block hash of the tip
    "branchlen": 0          (numeric) zero for main chain
    "status": "active"      (string) "active" for the main chain
  },
  {
    "height": xxxx,
    "hash": "xxxx",
    "branchlen": 1          (numeric) length of branch connecting the tip to the main chain
    "status": "xxxx"        (string) status of the chain (active, valid-fork, valid-headers, headers-only, invalid)
  }
]
Possible values for status:
1.  "invalid"               This branch contains at least one invalid block
2.  "headers-only"          Not all blocks for this branch are available, but the headers are valid
3.  "valid-headers"         All blocks are available for this branch, but they were never fully validated
4.  "valid-fork"            This branch is not part of the active chain, but is fully validated
5.  "active"                This is the tip of the active main chain, which is certainly valid

Examples:
> syscoin-cli getchaintips 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getchaintips", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getchaintxstats ( nblocks "blockhash" )</summary>

```

Compute statistics about the total number and rate of transactions in the chain.

Arguments:
1. nblocks      (numeric, optional, default=one month) Size of the window in number of blocks
2. blockhash    (string, optional, default=chain tip) The hash of the block that ends the window.

Result:
{
  "time": xxxxx,                         (numeric) The timestamp for the final block in the window in UNIX format.
  "txcount": xxxxx,                      (numeric) The total number of transactions in the chain up to that point.
  "window_final_block_hash": "...",      (string) The hash of the final block in the window.
  "window_block_count": xxxxx,           (numeric) Size of the window in number of blocks.
  "window_tx_count": xxxxx,              (numeric) The number of transactions in the window. Only returned if "window_block_count" is > 0.
  "window_interval": xxxxx,              (numeric) The elapsed time in the window in seconds. Only returned if "window_block_count" is > 0.
  "txrate": x.xx,                        (numeric) The average rate of transactions per second in the window. Only returned if "window_interval" is > 0.
}

Examples:
> syscoin-cli getchaintxstats 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getchaintxstats", "params": [2016] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getdifficulty</summary>

```

Returns the proof-of-work difficulty as a multiple of the minimum difficulty.

Result:
n.nnn       (numeric) the proof-of-work difficulty as a multiple of the minimum difficulty.

Examples:
> syscoin-cli getdifficulty 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getdifficulty", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getmempoolancestors "txid" ( verbose )</summary>

```

If txid is in the mempool, returns all in-mempool ancestors.

Arguments:
1. txid       (string, required) The transaction id (must be in mempool)
2. verbose    (boolean, optional, default=false) True for a json object, false for array of transaction ids

Result (for verbose = false):
[                       (json array of strings)
  "transactionid"           (string) The transaction id of an in-mempool ancestor transaction
  ,...
]

Result (for verbose = true):
{                           (json object)
  "transactionid" : {       (json object)
    "vsize" : n,            (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "size" : n,             (numeric) (DEPRECATED) same as vsize. Only returned if syscoind is started with -deprecatedrpc=size
                              size will be completely removed in v0.20.
    "fee" : n,              (numeric) transaction fee in SYS (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in SYS
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in SYS
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in SYS
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in SYS
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
  }, ...
}

Examples:
> syscoin-cli getmempoolancestors "mytxid"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolancestors", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getmempooldescendants "txid" ( verbose )</summary>

```

If txid is in the mempool, returns all in-mempool descendants.

Arguments:
1. txid       (string, required) The transaction id (must be in mempool)
2. verbose    (boolean, optional, default=false) True for a json object, false for array of transaction ids

Result (for verbose = false):
[                       (json array of strings)
  "transactionid"           (string) The transaction id of an in-mempool descendant transaction
  ,...
]

Result (for verbose = true):
{                           (json object)
  "transactionid" : {       (json object)
    "vsize" : n,            (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "size" : n,             (numeric) (DEPRECATED) same as vsize. Only returned if syscoind is started with -deprecatedrpc=size
                              size will be completely removed in v0.20.
    "fee" : n,              (numeric) transaction fee in SYS (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in SYS
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in SYS
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in SYS
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in SYS
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
  }, ...
}

Examples:
> syscoin-cli getmempooldescendants "mytxid"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempooldescendants", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getmempoolentry "txid"</summary>

```

Returns mempool data for given transaction

Arguments:
1. txid    (string, required) The transaction id (must be in mempool)

Result:
{                           (json object)
    "vsize" : n,            (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "size" : n,             (numeric) (DEPRECATED) same as vsize. Only returned if syscoind is started with -deprecatedrpc=size
                              size will be completely removed in v0.20.
    "fee" : n,              (numeric) transaction fee in SYS (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in SYS
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in SYS
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in SYS
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in SYS
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
}

Examples:
> syscoin-cli getmempoolentry "mytxid"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolentry", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getmempoolinfo</summary>

```

Returns details on the active state of the TX memory pool.

Result:
{
  "loaded": true|false         (boolean) True if the mempool is fully loaded
  "size": xxxxx,               (numeric) Current tx count
  "bytes": xxxxx,              (numeric) Sum of all virtual transaction sizes as defined in BIP 141. Differs from actual serialized size because witness data is discounted
  "usage": xxxxx,              (numeric) Total memory usage for the mempool
  "maxmempool": xxxxx,         (numeric) Maximum memory usage for the mempool
  "mempoolminfee": xxxxx       (numeric) Minimum fee rate in SYS/kB for tx to be accepted. Is the maximum of minrelaytxfee and minimum mempool fee
  "minrelaytxfee": xxxxx       (numeric) Current minimum relay fee for transactions
}

Examples:
> syscoin-cli getmempoolinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getrawmempool ( verbose )</summary>

```

Returns all transaction ids in memory pool as a json array of string transaction ids.

Hint: use getmempoolentry to fetch a specific transaction from the mempool.

Arguments:
1. verbose    (boolean, optional, default=false) True for a json object, false for array of transaction ids

Result (for verbose = false):
[                     (json array of string)
  "transactionid"     (string) The transaction id
  ,...
]

Result: (for verbose = true):
{                           (json object)
  "transactionid" : {       (json object)
    "vsize" : n,            (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "size" : n,             (numeric) (DEPRECATED) same as vsize. Only returned if syscoind is started with -deprecatedrpc=size
                              size will be completely removed in v0.20.
    "fee" : n,              (numeric) transaction fee in SYS (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in SYS
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in SYS
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in SYS
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in SYS
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
  }, ...
}

Examples:
> syscoin-cli getrawmempool true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getrawmempool", "params": [true] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>gettxout "txid" n ( include_mempool )</summary>

```

Returns details about an unspent transaction output.

Arguments:
1. txid               (string, required) The transaction id
2. n                  (numeric, required) vout number
3. include_mempool    (boolean, optional, default=true) Whether to include the mempool. Note that an unspent output that is spent in the mempool won't appear.

Result:
{
  "bestblock":  "hash",    (string) The hash of the block at the tip of the chain
  "confirmations" : n,       (numeric) The number of confirmations
  "value" : x.xxx,           (numeric) The transaction value in SYS
  "scriptPubKey" : {         (json object)
     "asm" : "code",       (string) 
     "hex" : "hex",        (string) 
     "reqSigs" : n,          (numeric) Number of required signatures
     "type" : "pubkeyhash", (string) The type, eg pubkeyhash
     "addresses" : [          (array of string) array of syscoin addresses
        "address"     (string) syscoin address
        ,...
     ]
  },
  "coinbase" : true|false   (boolean) Coinbase or not
}

Examples:

Get unspent transactions
> syscoin-cli listunspent 

View the details
> syscoin-cli gettxout "txid" 1

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gettxout", "params": ["txid", 1] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>gettxoutproof ["txid",...] ( "blockhash" )</summary>

```

Returns a hex-encoded proof that "txid" was included in a block.

NOTE: By default this function only works sometimes. This is when there is an
unspent output in the utxo for this transaction. To make it always work,
you need to maintain a transaction index, using the -txindex command line option or
specify the block in which the transaction is included manually (by blockhash).

Arguments:
1. txids          (json array, required) A json array of txids to filter
     [
       "txid",    (string) A transaction hash
       ...
     ]
2. blockhash      (string, optional) If specified, looks for txid in the block with this hash

Result:
"data"           (string) A string that is a serialized, hex-encoded data for the proof.
```
</details>

<details><summary>gettxoutsetinfo</summary>

```

Returns statistics about the unspent transaction output set.
Note this call may take some time.

Result:
{
  "height":n,     (numeric) The current block height (index)
  "bestblock": "hex",   (string) The hash of the block at the tip of the chain
  "transactions": n,      (numeric) The number of transactions with unspent outputs
  "txouts": n,            (numeric) The number of unspent transaction outputs
  "bogosize": n,          (numeric) A meaningless metric for UTXO set size
  "hash_serialized_2": "hash", (string) The serialized hash
  "disk_size": n,         (numeric) The estimated size of the chainstate on disk
  "total_amount": x.xxx          (numeric) The total amount
}

Examples:
> syscoin-cli gettxoutsetinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gettxoutsetinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>preciousblock "blockhash"</summary>

```

Treats a block as if it were received before others with the same work.

A later preciousblock call can override the effect of an earlier one.

The effects of preciousblock are not retained across restarts.

Arguments:
1. blockhash    (string, required) the hash of the block to mark as precious

Examples:
> syscoin-cli preciousblock "blockhash"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "preciousblock", "params": ["blockhash"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>pruneblockchain height</summary>

```

Arguments:
1. height    (numeric, required) The block height to prune up to. May be set to a discrete height, or a unix timestamp
             to prune blocks whose block time is at least 2 hours older than the provided timestamp.

Result:
n    (numeric) Height of the last block pruned.

Examples:
> syscoin-cli pruneblockchain 1000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "pruneblockchain", "params": [1000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>savemempool</summary>

```

Dumps the mempool to disk. It will fail until the previous dump is fully loaded.

Examples:
> syscoin-cli savemempool 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "savemempool", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>scantxoutset "action" [scanobjects,...]</summary>

```

EXPERIMENTAL warning: this call may be removed or changed in future releases.

Scans the unspent transaction output set for entries that match certain output descriptors.
Examples of output descriptors are:
    addr(<address>)                      Outputs whose scriptPubKey corresponds to the specified address (does not include P2PK)
    raw(<hex script>)                    Outputs whose scriptPubKey equals the specified hex scripts
    combo(<pubkey>)                      P2PK, P2PKH, P2WPKH, and P2SH-P2WPKH outputs for the given pubkey
    pkh(<pubkey>)                        P2PKH outputs for the given pubkey
    sh(multi(<n>,<pubkey>,<pubkey>,...)) P2SH-multisig outputs for the given threshold and pubkeys

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one
or more path elements separated by "/", and optionally ending in "/*" (unhardened), or "/*'" or "/*h" (hardened) to specify all
unhardened or hardened child keys.
In the latter case, a range needs to be specified by below if different from 1000.
For more information on output descriptors, see the documentation in the doc/descriptors.md file.

Arguments:
1. action                        (string, required) The action to execute
                                 "start" for starting a scan
                                 "abort" for aborting the current scan (returns true when abort was successful)
                                 "status" for progress report (in %) of the current scan
2. scanobjects                   (json array, required) Array of scan objects
                                 Every scan object is either a string descriptor or an object:
     [
       "descriptor",             (string) An output descriptor
       {                         (json object) An object with output descriptor and metadata
         "desc": "str",          (string, required) An output descriptor
         "range": n or [n,n],    (numeric or array, optional, default=1000) The range of HD chain indexes to explore (either end or [begin,end])
       },
       ...
     ]

Result:
{
  "unspents": [
    {
    "txid" : "transactionid",     (string) The transaction id
    "vout": n,                    (numeric) the vout value
    "scriptPubKey" : "script",    (string) the script key
    "desc" : "descriptor",        (string) A specialized descriptor for the matched scriptPubKey
    "amount" : x.xxx,             (numeric) The total amount in SYS of the unspent output
    "height" : n,                 (numeric) Height of the unspent transaction output
   }
   ,...], 
 "total_amount" : x.xxx,          (numeric) The total amount of all found unspent outputs in SYS
]
```
</details>

<details><summary>verifychain ( checklevel nblocks )</summary>

```

Verifies blockchain database.

Arguments:
1. checklevel    (numeric, optional, default=4, range=0-4) How thorough the block verification is.
2. nblocks       (numeric, optional, default=6, 0=all) The number of blocks to check.

Result:
true|false       (boolean) Verified or not

Examples:
> syscoin-cli verifychain 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "verifychain", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>verifytxoutproof "proof"</summary>

```

Verifies that a proof points to a transaction in a block, returning the transaction it commits to
and throwing an RPC error if the block is not in our best chain

Arguments:
1. proof    (string, required) The hex-encoded proof generated by gettxoutproof

Result:
["txid"]      (array, strings) The txid(s) which the proof commits to, or empty array if the proof can not be validated.
```
</details>

## Control

<details><summary>getmemoryinfo ( "mode" )</summary>

```
Returns an object containing information about memory usage.

Arguments:
1. mode    (string, optional, default="stats") determines what kind of information is returned.
           - "stats" returns general statistics about memory usage in the daemon.
           - "mallocinfo" returns an XML string describing low-level heap state (only available if compiled with glibc 2.10+).

Result (mode "stats"):
{
  "locked": {               (json object) Information about locked memory manager
    "used": xxxxx,          (numeric) Number of bytes used
    "free": xxxxx,          (numeric) Number of bytes available in current arenas
    "total": xxxxxxx,       (numeric) Total number of bytes managed
    "locked": xxxxxx,       (numeric) Amount of bytes that succeeded locking. If this number is smaller than total, locking pages failed at some point and key data could be swapped to disk.
    "chunks_used": xxxxx,   (numeric) Number allocated chunks
    "chunks_free": xxxxx,   (numeric) Number unused chunks
  }
}

Result (mode "mallocinfo"):
"<malloc version="1">..."

Examples:
> syscoin-cli getmemoryinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmemoryinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getrpcinfo</summary>

```

Returns details of the RPC server.

Result:
{
 "active_commands" (array) All active commands
  [
   {               (object) Information about an active command
    "method"       (string)  The name of the RPC command 
    "duration"     (numeric)  The running time in microseconds
   },...
  ]
}

Examples:
> syscoin-cli getrpcinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getrpcinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>help ( "command" )</summary>

```

List all commands, or get help for a specified command.

Arguments:
1. command    (string, optional, default=all commands) The command to get help on

Result:
"text"     (string) The help text
```
</details>

<details><summary>logging ( ["include_category",...] ["exclude_category",...] )</summary>

```
Gets and sets the logging configuration.
When called without an argument, returns the list of categories with status that are currently being debug logged or not.
When called with arguments, adds or removes categories from debug logging and return the lists above.
The arguments are evaluated in order "include", "exclude".
If an item is both included and excluded, it will thus end up being excluded.
The valid logging categories are: net, tor, mempool, http, bench, zmq, db, rpc, estimatefee, addrman, selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej, libevent, coindb, qt, leveldb, threadpool, masternode, gobject, mnpayments, mnsync, spork, syscoin
In addition, the following are available as category names with special meanings:
  - "all",  "1" : represent all logging categories.
  - "none", "0" : even if other logging categories are specified, ignore all of them.

Arguments:
1. include                    (json array, optional) A json array of categories to add debug logging
     [
       "include_category",    (string) the valid logging category
       ...
     ]
2. exclude                    (json array, optional) A json array of categories to remove debug logging
     [
       "exclude_category",    (string) the valid logging category
       ...
     ]

Result:
{                   (json object where keys are the logging categories, and values indicates its status
  "category": true|false,  (bool) if being debug logged or not. false:inactive, true:active
  ...
}

Examples:
> syscoin-cli logging "[\"all\"]" "[\"http\"]"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "logging", "params": [["all"], ["libevent"]] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>stop</summary>

```

Stop Syscoin server.
```
</details>

<details><summary>uptime</summary>

```

Returns the total uptime of the server.

Result:
ttt        (numeric) The number of seconds that the server has been running

Examples:
> syscoin-cli uptime 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "uptime", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Generating

<details><summary>generate nblocks ( maxtries )</summary>

```

Mine up to nblocks blocks immediately (before the RPC call returns) to an address in the wallet.

Arguments:
1. nblocks     (numeric, required) How many blocks are generated immediately.
2. maxtries    (numeric, optional, default=100000) How many iterations to try.

Result:
[ blockhashes ]     (array) hashes of blocks generated

Examples:
> syscoin-cli generate 11
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "generate", "params": [11] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>generatetoaddress nblocks "address" ( maxtries )</summary>

```

Mine blocks immediately to a specified address (before the RPC call returns)

Arguments:
1. nblocks     (numeric, required) How many blocks are generated immediately.
2. address     (string, required) The address to send the newly generated syscoin to.
3. maxtries    (numeric, optional, default=1000000) How many iterations to try.

Result:
[ blockhashes ]     (array) hashes of blocks generated

Examples:

Generate 11 blocks to myaddress
> syscoin-cli generatetoaddress 11 "myaddress"
If you are running the syscoin core wallet, you can get a new address to send the newly generated syscoin to with:
> syscoin-cli getnewaddress 
```
</details>

## Governance

<details><summary>getgovernanceinfo</summary>

```

Returns an object containing governance parameters.

Result:
{
  "governanceminquorum": xxxxx,           (numeric) the absolute minimum number of votes needed to trigger a governance action
  "masternodewatchdogmaxseconds": xxxxx,  (numeric) sentinel watchdog expiration time in seconds (DEPRECATED)
  "sentinelpingmaxseconds": xxxxx,        (numeric) sentinel ping expiration time in seconds
  "proposalfee": xxx.xx,                  (numeric) the collateral transaction fee which must be paid to create a proposal in SYS
  "superblockcycle": xxxxx,               (numeric) the number of blocks between superblocks
  "lastsuperblock": xxxxx,                (numeric) the block number of the last superblock
  "nextsuperblock": xxxxx,                (numeric) the block number of the next superblock
  "maxgovobjdatasize": xxxxx,             (numeric) maximum governance object data size in bytes
}

Examples:
> syscoin-cli getgovernanceinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getgovernanceinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getsuperblockbudget index</summary>

```

Returns the absolute maximum sum of superblock payments allowed.

Arguments:
1. index    (numeric, required) The block index

Result:
n                (numeric) The absolute maximum sum of superblock payments allowed, in SYS

Examples:
> syscoin-cli getsuperblockbudget 1000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getsuperblockbudget", "params": [1000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>masternodelist ( "mode" "filter" )</summary>

```

Get a list of masternodes in different modes

Available modes:
  activeseconds  - Print number of seconds masternode recognized by the network as enabled
                   (since latest issued "masternode start/start-many/initialize")
  addr           - Print ip address associated with a masternode (can be additionally filtered, partial match)
  daemon         - Print daemon version of a masternode (can be additionally filtered, exact match)
  full           - Print info in format 'status protocol payee lastseen activeseconds lastpaidtime lastpaidblock IP pingretries'
                   (can be additionally filtered, partial match)
  info           - Print info in format 'status protocol payee lastseen activeseconds sentinelversion sentinelstate IP pingretries'
                   (can be additionally filtered, partial match)
  json           - Print info in JSON format (can be additionally filtered, partial match)
  lastpaidblock  - Print the last block height a node was paid on the network
  lastpaidtime   - Print the last time a node was paid on the network
  lastseen       - Print timestamp of when a masternode was last seen on the network
  payee          - Print Syscoin address associated with a masternode (can be additionally filtered,
                   partial match)
  protocol       - Print protocol of a masternode (can be additionally filtered, exact match)
  pubkey         - Print the masternode (not collateral) public key
  rank           - Print rank of a masternode based on current block
  sentinel       - Print sentinel version of a masternode (can be additionally filtered, exact match)
  status         - Print masternode status: PRE_ENABLED / ENABLED / NEW_START_REQUIRED /
                   UPDATE_REQUIRED / POSE_BAN / OUTPOINT_SPENT (can be additionally filtered, partial match)

Arguments:
1. mode      (string, optional, default=json) The mode to run list in
2. filter    (string, optional, default=) FIlter results. Partial match by outpoint by default in all modes; additional matches in some modes are also available

Examples:
> syscoin-cli masternodelist 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "masternodelist", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>mnsync "command"</summary>

```

Returns the sync status, updates to the next step or resets it entirely.

Arguments:
1. command    (string, required) The command to issue (status|next|reset)

Result:
"result"           (string) Result
Examples:
> syscoin-cli mnsync status
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "mnsync", "params": [status] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>sentinelping version</summary>

```

Arguments:
1. version    (numeric, required) Sentinel Version

Result:
true|false      (boolean) Ping result

Examples:
> syscoin-cli sentinelping 1000000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sentinelping", "params": [1000001] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>spork "command"</summary>

```

Shows information about current state of sporks

Arguments:
1. command    (string, required) "show" to show all current spork values, "active" to show which sporks are active

Result (for command = "show"):
{
  "SPORK_NAME" : spork_value,    (number) The value of the specific spork with the name SPORK_NAME
  ...
}

Result (for command = "active"):
{
  "SPORK_NAME" : true|false,     (boolean) 'true' for time-based sporks if spork is active and 'false' otherwise
  ...
}

Examples:
> syscoin-cli spork show
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "spork", "params": ["show"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>voteraw "masaternode-tx-hash" masternode-tx-index "governancehash" "vote-signal" "vote" time "vote-sig"</summary>

```

Compile and relay a governance vote with provided external signature instead of signing vote internally

Arguments:
1. masaternode-tx-hash    (string, required) The masternode transaction hash
2. masternode-tx-index    (numeric, required) The masterndoe transaction index
3. governancehash         (string, required) The governance gobject hash
4. vote-signal            (string, required) The vote-signal (funding|valid|delete|endorsed)
5. vote                   (string, required) The vote (yes|no|abstain)
6. time                   (numeric, required) The linux epoche time
7. vote-sig               (string, required) The external signature of the vote

Result:
"result"      (string) Result
Examples:
> syscoin-cli voteraw "tx-hash" "tx-index" "gov-hash" "funding" "yes" "time" "vote-sig"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "voteraw", "params": ["tx-hash", "tx-index", "gov-hash", "funding", "yes", "time", "vote-sig"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Governancewallet

<details><summary>gobject "command"</summary>

```
Manage governance objects

Available commands:
  check              - Validate governance object data (proposal only)
  prepare            - Prepare governance object by signing and creating tx
  submit             - Submit governance object to network
  deserialize        - Deserialize governance object from hex string to JSON
  count              - Count governance objects and votes (additional param: 'json' or 'all', default: 'json')
  get                - Get governance object by hash
  getvotes           - Get all votes for a governance object hash (including old votes)
  getcurrentvotes    - Get only current (tallying) votes for a governance object hash (does not include old votes)
  list               - List governance objects (can be filtered by signal and/or object type)
  diff               - List differences since last diff
  vote-name         - Vote on a governance object by masternode name (using masternode.conf setup)
  vote-conf          - Vote on a governance object by masternode configured in syscoin.conf
  vote-many          - Vote on a governance object by all masternodes (using masternode.conf setup)

Arguments:
1. command    (string, required) command to call (check|prepare|submit|deserialize|count|get|getvotes|getcurrentvotes|list|diff|vote-name|vote-conf|vote-many)

Examples:
> syscoin-cli gobject list
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gobject", "params": [list] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>masternode "command"</summary>

```

Set of commands to execute masternode related actions

Arguments:
1. "command"        (string or set of strings, required) The command to execute

Available commands:
  count        - Get information about number of masternodes (DEPRECATED options: 'total', 'enabled', 'qualify', 'all')
  current      - Print info on current masternode winner to be paid the next block (calculated locally)
  genkey       - Generate new masternodeprivkey
  outputs      - Print masternode compatible outputs
  initialize  - Start single remote masternode by assigned name configured in masternode.conf
  start-<mode> - Start remote masternodes configured in masternode.conf (<mode>: 'all', 'missing', 'disabled')
  status       - Print masternode status information
  list         - Print list of all known masternodes (see masternodelist for more info)
  list-conf    - Print masternode.conf in JSON format
  winner       - Print info on next masternode winner to vote for
  winners      - Print list of masternode winners

Arguments:
1. command    (string, required) (count|current|genkey|outputs|initialize|start-<mode>|status|list|list-conf|winner|winners

Examples:
> syscoin-cli masternode list
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "masternode", "params": [list] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>masternodebroadcast "command"</summary>

```

Set of commands to create and relay masternode broadcast messages

Available commands:
  create-name  - Create single remote masternode broadcast message by assigned name configured in masternode.conf
  create-all    - Create remote masternode broadcast messages for all masternodes configured in masternode.conf
  decode        - Decode masternode broadcast message
  relay         - Relay masternode broadcast message to the network

Arguments:
1. command    (string, required) The command to execute (create-name|create-all|decode|relay)

Examples:
> syscoin-cli masternodebroadcast decode "message"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "masternodebroadcast", "params": [decode, "message"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Mining

<details><summary>createauxblock "address"</summary>

```

Creates a new block and returns information required to merge-mine it.

Arguments:
1. address    (string, required) Payout address for the coinbase transaction

Result:
{
  "hash"               (string) hash of the created block
  "chainid"            (numeric) chain ID for this block
  "previousblockhash"  (string) hash of the previous block
  "coinbasevalue"      (numeric) value of the block's coinbase
  "bits"               (string) compressed target of the block
  "height"             (numeric) height of the block
  "_target"            (string) target in reversed byte order, deprecated
}

Examples:
> syscoin-cli createauxblock "address"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createauxblock", "params": ["address"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getauxblock ( "hash" "auxpow" )</summary>

```

Creates or submits a merge-mined block.

Without arguments, creates a new block and returns information
required to merge-mine it.  With arguments, submits a solved
auxpow for a previously returned block.

Arguments:
1. hash      (string, optional) Hash of the block to submit
2. auxpow    (string, optional) Serialised auxpow found

Result (without arguments):
{
  "hash"               (string) hash of the created block
  "chainid"            (numeric) chain ID for this block
  "previousblockhash"  (string) hash of the previous block
  "coinbasevalue"      (numeric) value of the block's coinbase
  "bits"               (string) compressed target of the block
  "height"             (numeric) height of the block
  "_target"            (string) target in reversed byte order, deprecated
}

Result (with arguments):
xxxxx        (boolean) whether the submitted block was correct

Examples:
> syscoin-cli getauxblock 
> syscoin-cli getauxblock "hash" "serialised auxpow"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getauxblock", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblocktemplate "template_request"</summary>

```

If the request parameters include a 'mode' key, that is used to explicitly select between the default 'template' request or a 'proposal'.
It returns data needed to construct a block to work on.
For full specification, see BIPs 22, 23, 9, and 145:
    https://github.com/syscoin/bips/blob/master/bip-0022.mediawiki
    https://github.com/syscoin/bips/blob/master/bip-0023.mediawiki
    https://github.com/syscoin/bips/blob/master/bip-0009.mediawiki#getblocktemplate_changes
    https://github.com/syscoin/bips/blob/master/bip-0145.mediawiki

Arguments:
1. template_request         (json object, required) A json object in the following spec
     {
       "mode": "str",       (string, optional) This must be set to "template", "proposal" (see BIP 23), or omitted
       "capabilities": [    (json array, optional) A list of strings
         "support",         (string) client side supported feature, 'longpoll', 'coinbasetxn', 'coinbasevalue', 'proposal', 'serverlist', 'workid'
         ...
       ],
       "rules": [           (json array, required) A list of strings
         "support",         (string) client side supported softfork deployment
         ...
       ],
     }

Result:
{
  "version" : n,                    (numeric) The preferred block version
  "rules" : [ "rulename", ... ],    (array of strings) specific block rules that are to be enforced
  "vbavailable" : {                 (json object) set of pending, supported versionbit (BIP 9) softfork deployments
      "rulename" : bitnumber          (numeric) identifies the bit number as indicating acceptance and readiness for the named softfork rule
      ,...
  },
  "vbrequired" : n,                 (numeric) bit mask of versionbits the server requires set in submissions
  "previousblockhash" : "xxxx",     (string) The hash of current highest block
  "transactions" : [                (array) contents of non-coinbase transactions that should be included in the next block
      {
         "data" : "xxxx",             (string) transaction data encoded in hexadecimal (byte-for-byte)
         "txid" : "xxxx",             (string) transaction id encoded in little-endian hexadecimal
         "hash" : "xxxx",             (string) hash encoded in little-endian hexadecimal (including witness data)
         "depends" : [                (array) array of numbers 
             n                          (numeric) transactions before this one (by 1-based index in 'transactions' list) that must be present in the final block if this one is
             ,...
         ],
         "fee": n,                    (numeric) difference in value between transaction inputs and outputs (in satoshis); for coinbase transactions, this is a negative Number of the total collected block fees (ie, not including the block subsidy); if key is not present, fee is unknown and clients MUST NOT assume there isn't one
         "sigops" : n,                (numeric) total SigOps cost, as counted for purposes of block limits; if key is not present, sigop cost is unknown and clients MUST NOT assume it is zero
         "weight" : n,                (numeric) total transaction weight, as counted for purposes of block limits
      }
      ,...
  ],
  "coinbaseaux" : {                 (json object) data that should be included in the coinbase's scriptSig content
      "flags" : "xx"                  (string) key name is to be ignored, and value included in scriptSig
  },
  "coinbasevalue" : n,              (numeric) maximum allowable input to coinbase transaction, including the generation award and transaction fees (in satoshis)
  "coinbasetxn" : { ... },          (json object) information for coinbase transaction
  "target" : "xxxx",                (string) The hash target
  "mintime" : xxx,                  (numeric) The minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
  "mutable" : [                     (array of string) list of ways the block template may be changed 
     "value"                          (string) A way the block template may be changed, e.g. 'time', 'transactions', 'prevblock'
     ,...
  ],
  "noncerange" : "00000000ffffffff",(string) A range of valid nonces
  "sigoplimit" : n,                 (numeric) limit of sigops in blocks
  "sizelimit" : n,                  (numeric) limit of block size
  "weightlimit" : n,                (numeric) limit of block weight
  "curtime" : ttt,                  (numeric) current timestamp in seconds since epoch (Jan 1 1970 GMT)
  "bits" : "xxxxxxxx",             (string) compressed target of next block
  "height" : n,                      (numeric) The height of the next block
  "masternode" : {                  (json object) required masternode payee that must be included in the next block
      "payee" : "xxxx",             (string) payee address
      "script" : "xxxx",            (string) payee scriptPubKey
      "amount": n                   (numeric) required amount to pay
  },
  "masternode_payments_enforced" : true|false, (boolean) true, if masternode payments are enforced
  "superblock" : [                  (array) required superblock payees that must be included in the next block
      {
         "payee" : "xxxx",          (string) payee address
         "script" : "xxxx",         (string) payee scriptPubKey
         "amount": n                (numeric) required amount to pay
      }
      ,...
  ],
  "superblocks_started" : true|false, (boolean) true, if superblock payments started
  "superblocks_enabled" : true|false  (boolean) true, if superblock payments are enabled
}

Examples:
> syscoin-cli getblocktemplate {"rules": ["segwit"]}
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblocktemplate", "params": [{"rules": ["segwit"]}] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getmininginfo</summary>

```

Returns a json object containing mining-related information.
Result:
{
  "blocks": nnn,             (numeric) The current block
  "currentblockweight": nnn, (numeric, optional) The block weight of the last assembled block (only present if a block was ever assembled)
  "currentblocktx": nnn,     (numeric, optional) The number of block transactions of the last assembled block (only present if a block was ever assembled)
  "difficulty": xxx.xxxxx    (numeric) The current difficulty
  "networkhashps": nnn,      (numeric) The network hashes per second
  "pooledtx": n              (numeric) The size of the mempool
  "chain": "xxxx",           (string) current network name as defined in BIP70 (main, test, regtest)
  "warnings": "..."          (string) any network and blockchain warnings
}

Examples:
> syscoin-cli getmininginfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmininginfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getnetworkhashps ( nblocks height )</summary>

```

Returns the estimated network hashes per second based on the last n blocks.
Pass in [blocks] to override # of blocks, -1 specifies since last difficulty change.
Pass in [height] to estimate the network speed at the time when a certain block was found.

Arguments:
1. nblocks    (numeric, optional, default=120) The number of blocks, or -1 for blocks since last difficulty change.
2. height     (numeric, optional, default=-1) To estimate at the time of the given height.

Result:
x             (numeric) Hashes per second estimated

Examples:
> syscoin-cli getnetworkhashps 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnetworkhashps", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>prioritisetransaction "txid" ( dummy ) fee_delta</summary>

```
Accepts the transaction into mined blocks at a higher (or lower) priority

Arguments:
1. txid         (string, required) The transaction id.
2. dummy        (numeric, optional) API-Compatibility for previous API. Must be zero or null.
                DEPRECATED. For forward compatibility use named arguments and omit this parameter.
3. fee_delta    (numeric, required) The fee value (in satoshis) to add (or subtract, if negative).
                Note, that this value is not a fee rate. It is a value to modify absolute fee of the TX.
                The fee is not actually paid, only the algorithm for selecting transactions into a block
                considers the transaction as it would have paid a higher (or lower) fee.

Result:
true              (boolean) Returns true

Examples:
> syscoin-cli prioritisetransaction "txid" 0.0 10000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "prioritisetransaction", "params": ["txid", 0.0, 10000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>submitauxblock "hash" "auxpow"</summary>

```

Submits a solved auxpow for a block that was previously created by 'createauxblock'.

Arguments:
1. hash      (string, required) Hash of the block to submit
2. auxpow    (string, required) Serialised auxpow found

Result:
xxxxx        (boolean) whether the submitted block was correct

Examples:
> syscoin-cli submitauxblock "hash" "serialised auxpow"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "submitauxblock", "params": ["hash" "serialised auxpow"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>submitblock "hexdata" ( "dummy" )</summary>

```

Attempts to submit new block to network.
See https://en.syscoin.it/wiki/BIP_0022 for full specification.

Arguments:
1. hexdata    (string, required) the hex-encoded block data to submit
2. dummy      (string, optional, default=ignored) dummy value, for compatibility with BIP22. This value is ignored.

Examples:
> syscoin-cli submitblock "mydata"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "submitblock", "params": ["mydata"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>submitheader "hexdata"</summary>

```

Decode the given hexdata as a header and submit it as a candidate chain tip if valid.
Throws when the header is invalid.

Arguments:
1. hexdata    (string, required) the hex-encoded block header data

Result:
None
Examples:
> syscoin-cli submitheader "aabbcc"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "submitheader", "params": ["aabbcc"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Network

<details><summary>addnode "node" "command"</summary>

```

Attempts to add or remove a node from the addnode list.
Or try a connection to a node once.
Nodes added using addnode (or -connect) are protected from DoS disconnection and are not required to be
full nodes/support SegWit as other outbound peers are (though such peers will not be synced from).

Arguments:
1. node       (string, required) The node (see getpeerinfo for nodes)
2. command    (string, required) 'add' to add a node to the list, 'remove' to remove a node from the list, 'onetry' to try a connection to the node once

Examples:
> syscoin-cli addnode "192.168.0.6:8369" "onetry"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "addnode", "params": ["192.168.0.6:8369", "onetry"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>clearbanned</summary>

```

Clear all banned IPs.

Examples:
> syscoin-cli clearbanned 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "clearbanned", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>disconnectnode ( "address" nodeid )</summary>

```

Immediately disconnects from the specified peer node.

Strictly one out of 'address' and 'nodeid' can be provided to identify the node.

To disconnect by nodeid, either set 'address' to the empty string, or call using the named 'nodeid' argument only.

Arguments:
1. address    (string, optional, default=fallback to nodeid) The IP address/port of the node
2. nodeid     (numeric, optional, default=fallback to address) The node ID (see getpeerinfo for node IDs)

Examples:
> syscoin-cli disconnectnode "192.168.0.6:8369"
> syscoin-cli disconnectnode "" 1
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "disconnectnode", "params": ["192.168.0.6:8369"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "disconnectnode", "params": ["", 1] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getaddednodeinfo ( "node" )</summary>

```

Returns information about the given added node, or all added nodes
(note that onetry addnodes are not listed here)

Arguments:
1. node    (string, optional, default=all nodes) If provided, return information about this specific node, otherwise all nodes are returned.

Result:
[
  {
    "addednode" : "192.168.0.201",   (string) The node IP address or name (as provided to addnode)
    "connected" : true|false,          (boolean) If connected
    "addresses" : [                    (list of objects) Only when connected = true
       {
         "address" : "192.168.0.201:8369",  (string) The syscoin server IP and port we're connected to
         "connected" : "outbound"           (string) connection, inbound or outbound
       }
     ]
  }
  ,...
]

Examples:
> syscoin-cli getaddednodeinfo "192.168.0.201"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getaddednodeinfo", "params": ["192.168.0.201"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getconnectioncount</summary>

```

Returns the number of connections to other nodes.

Result:
n          (numeric) The connection count

Examples:
> syscoin-cli getconnectioncount 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getconnectioncount", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getnettotals</summary>

```

Returns information about network traffic, including bytes in, bytes out,
and current time.

Result:
{
  "totalbytesrecv": n,   (numeric) Total bytes received
  "totalbytessent": n,   (numeric) Total bytes sent
  "timemillis": t,       (numeric) Current UNIX time in milliseconds
  "uploadtarget":
  {
    "timeframe": n,                         (numeric) Length of the measuring timeframe in seconds
    "target": n,                            (numeric) Target in bytes
    "target_reached": true|false,           (boolean) True if target is reached
    "serve_historical_blocks": true|false,  (boolean) True if serving historical blocks
    "bytes_left_in_cycle": t,               (numeric) Bytes left in current time cycle
    "time_left_in_cycle": t                 (numeric) Seconds left in current time cycle
  }
}

Examples:
> syscoin-cli getnettotals 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnettotals", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getnetworkinfo</summary>

```
Returns an object containing various state info regarding P2P networking.

Result:
{
  "version": xxxxx,                      (numeric) the server version
  "subversion": "/Satoshi:x.x.x/",     (string) the server subversion string
  "protocolversion": xxxxx,              (numeric) the protocol version
  "localservices": "xxxxxxxxxxxxxxxx", (string) the services we offer to the network
  "localrelay": true|false,              (bool) true if transaction relay is requested from peers
  "timeoffset": xxxxx,                   (numeric) the time offset
  "connections": xxxxx,                  (numeric) the number of connections
  "geth_sync_status": xxxxxx,   (numeric) Sync status of Geth (Ethereum network). Valid values are: synced, syncing or waiting to sync...
  "geth_total_blocks": xxxxxx,  (numeric) The highest height seen on the Ethereum network by Geth
  "geth_current_block": xxxxxx, (numeric) The highest height of the block header of Ethereum that is stored in our database
  "networkactive": true|false,           (bool) whether p2p networking is enabled
  "networks": [                          (array) information per network
  {
    "name": "xxx",                     (string) network (ipv4, ipv6 or onion)
    "limited": true|false,               (boolean) is the network limited using -onlynet?
    "reachable": true|false,             (boolean) is the network reachable?
    "proxy": "host:port"               (string) the proxy that is used for this network, or empty if none
    "proxy_randomize_credentials": true|false,  (string) Whether randomized credentials are used
  }
  ,...
  ],
  "relayfee": x.xxxxxxxx,                (numeric) minimum relay fee for transactions in SYS/kB
  "incrementalfee": x.xxxxxxxx,          (numeric) minimum fee increment for mempool limiting or BIP 125 replacement in SYS/kB
  "localaddresses": [                    (array) list of local addresses
  {
    "address": "xxxx",                 (string) network address
    "port": xxx,                         (numeric) network port
    "score": xxx                         (numeric) relative score
  }
  ,...
  ]
  "warnings": "..."                    (string) any network and blockchain warnings
}

Examples:
> syscoin-cli getnetworkinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnetworkinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getnodeaddresses ( count )</summary>

```

Return known addresses which can potentially be used to find new nodes in the network

Arguments:
1. count    (numeric, optional, default=1) How many addresses to return. Limited to the smaller of 2500 or 23% of all known addresses.

Result:
[
  {
    "time": ttt,                (numeric) Timestamp in seconds since epoch (Jan 1 1970 GMT) keeping track of when the node was last seen
    "services": n,              (numeric) The services offered
    "address": "host",          (string) The address of the node
    "port": n                   (numeric) The port of the node
  }
  ,....
]

Examples:
> syscoin-cli getnodeaddresses 8
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnodeaddresses", "params": [8] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getpeerinfo</summary>

```

Returns data about each connected network node as a json array of objects.

Result:
[
  {
    "id": n,                   (numeric) Peer index
    "addr":"host:port",      (string) The IP address and port of the peer
    "addrbind":"ip:port",    (string) Bind address of the connection to the peer
    "addrlocal":"ip:port",   (string) Local address as reported by the peer
    "services":"xxxxxxxxxxxxxxxx",   (string) The services offered
    "relaytxes":true|false,    (boolean) Whether peer has asked us to relay transactions to it
    "lastsend": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last send
    "lastrecv": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last receive
    "bytessent": n,            (numeric) The total bytes sent
    "bytesrecv": n,            (numeric) The total bytes received
    "conntime": ttt,           (numeric) The connection time in seconds since epoch (Jan 1 1970 GMT)
    "timeoffset": ttt,         (numeric) The time offset in seconds
    "pingtime": n,             (numeric) ping time (if available)
    "minping": n,              (numeric) minimum observed ping time (if any at all)
    "pingwait": n,             (numeric) ping wait (if non-zero)
    "version": v,              (numeric) The peer version, such as 70001
    "subver": "/Satoshi:0.8.5/",  (string) The string version
    "inbound": true|false,     (boolean) Inbound (true) or Outbound (false)
    "addnode": true|false,     (boolean) Whether connection was due to addnode/-connect or if it was an automatic/inbound connection
    "startingheight": n,       (numeric) The starting height (block) of the peer
    "banscore": n,             (numeric) The ban score
    "synced_headers": n,       (numeric) The last header we have in common with this peer
    "synced_blocks": n,        (numeric) The last block we have in common with this peer
    "inflight": [
       n,                        (numeric) The heights of blocks we're currently asking from this peer
       ...
    ],
    "whitelisted": true|false, (boolean) Whether the peer is whitelisted
    "minfeefilter": n,         (numeric) The minimum fee rate for transactions this peer accepts
    "bytessent_per_msg": {
       "msg": n,               (numeric) The total bytes sent aggregated by message type
                               When a message type is not listed in this json object, the bytes sent are 0.
                               Only known message types can appear as keys in the object.
       ...
    },
    "bytesrecv_per_msg": {
       "msg": n,               (numeric) The total bytes received aggregated by message type
                               When a message type is not listed in this json object, the bytes received are 0.
                               Only known message types can appear as keys in the object and all bytes received of unknown message types are listed under '*other*'.
       ...
    }
  }
  ,...
]

Examples:
> syscoin-cli getpeerinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getpeerinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listbanned</summary>

```

List all banned IPs/Subnets.

Examples:
> syscoin-cli listbanned 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listbanned", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>ping</summary>

```

Requests that a ping be sent to all other nodes, to measure ping time.
Results provided in getpeerinfo, pingtime and pingwait fields are decimal seconds.
Ping command is handled in queue with all other commands, so it measures processing backlog, not just network ping.

Examples:
> syscoin-cli ping 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "ping", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>setban "subnet" "command" ( bantime absolute )</summary>

```

Attempts to add or remove an IP/Subnet from the banned list.

Arguments:
1. subnet      (string, required) The IP/Subnet (see getpeerinfo for nodes IP) with an optional netmask (default is /32 = single IP)
2. command     (string, required) 'add' to add an IP/Subnet to the list, 'remove' to remove an IP/Subnet from the list
3. bantime     (numeric, optional, default=0) time in seconds how long (or until when if [absolute] is set) the IP is banned (0 or empty means using the default time of 24h which can also be overwritten by the -bantime startup argument)
4. absolute    (boolean, optional, default=false) If set, the bantime must be an absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

Examples:
> syscoin-cli setban "192.168.0.6" "add" 86400
> syscoin-cli setban "192.168.0.0/24" "add"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "setban", "params": ["192.168.0.6", "add", 86400] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>setnetworkactive state</summary>

```

Disable/enable all p2p network activity.

Arguments:
1. state    (boolean, required) true to enable networking, false to disable
```
</details>

## Rawtransactions

<details><summary>analyzepsbt "psbt"</summary>

```

Analyzes and provides information about the current status of a PSBT and its inputs

Arguments:
1. psbt    (string, required) A base64 string of a PSBT

Result:
{
  "inputs" : [                      (array of json objects)
    {
      "has_utxo" : true|false     (boolean) Whether a UTXO is provided
      "is_final" : true|false     (boolean) Whether the input is finalized
      "missing" : {               (json object, optional) Things that are missing that are required to complete this input
        "pubkeys" : [             (array, optional)
          "keyid"                 (string) Public key ID, hash160 of the public key, of a public key whose BIP 32 derivation path is missing
        ]
        "signatures" : [          (array, optional)
          "keyid"                 (string) Public key ID, hash160 of the public key, of a public key whose signature is missing
        ]
        "redeemscript" : "hash"   (string, optional) Hash160 of the redeemScript that is missing
        "witnessscript" : "hash"  (string, optional) SHA256 of the witnessScript that is missing
      }
      "next" : "role"             (string, optional) Role of the next person that this input needs to go to
    }
    ,...
  ]
  "estimated_vsize" : vsize       (numeric, optional) Estimated vsize of the final signed transaction
  "estimated_feerate" : feerate   (numeric, optional) Estimated feerate of the final signed transaction in SYS/kB. Shown only if all UTXO slots in the PSBT have been filled.
  "fee" : fee                     (numeric, optional) The transaction fee paid. Shown only if all UTXO slots in the PSBT have been filled.
  "next" : "role"                 (string) Role of the next person that this psbt needs to go to
}

Examples:
> syscoin-cli analyzepsbt "psbt"
```
</details>

<details><summary>combinepsbt ["psbt",...]</summary>

```

Combine multiple partially signed Syscoin transactions into one transaction.
Implements the Combiner role.

Arguments:
1. txs            (json array, required) A json array of base64 strings of partially signed transactions
     [
       "psbt",    (string) A base64 string of a PSBT
       ...
     ]

Result:
  "psbt"          (string) The base64-encoded partially signed transaction

Examples:
> syscoin-cli combinepsbt ["mybase64_1", "mybase64_2", "mybase64_3"]
```
</details>

<details><summary>combinerawtransaction ["hexstring",...]</summary>

```

Combine multiple partially signed transactions into one transaction.
The combined transaction may be another partially signed transaction or a 
fully signed transaction.
Arguments:
1. txs                 (json array, required) A json array of hex strings of partially signed transactions
     [
       "hexstring",    (string) A transaction hash
       ...
     ]

Result:
"hex"            (string) The hex-encoded raw transaction with signature(s)

Examples:
> syscoin-cli combinerawtransaction ["myhex1", "myhex2", "myhex3"]
```
</details>

<details><summary>converttopsbt "hexstring" ( permitsigdata iswitness )</summary>

```

Converts a network serialized transaction to a PSBT. This should be used only with createrawtransaction and fundrawtransaction
createpsbt and walletcreatefundedpsbt should be used for new applications.

Arguments:
1. hexstring        (string, required) The hex string of a raw transaction
2. permitsigdata    (boolean, optional, default=false) If true, any signatures in the input will be discarded and conversion.
                    will continue. If false, RPC will fail if any signatures are present.
3. iswitness        (boolean, optional, default=depends on heuristic tests) Whether the transaction hex is a serialized witness transaction.
                    If iswitness is not present, heuristic tests will be used in decoding. If true, only witness deserializaion
                    will be tried. If false, only non-witness deserialization will be tried. Only has an effect if
                    permitsigdata is true.

Result:
  "psbt"        (string)  The resulting raw transaction (base64-encoded string)

Examples:

Create a transaction
> syscoin-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"

Convert the transaction to a PSBT
> syscoin-cli converttopsbt "rawtransaction"
```
</details>

<details><summary>createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )</summary>

```

Creates a transaction in the Partially Signed Transaction format.
Implements the Creator role.

Arguments:
1. inputs                      (json array, required) A json array of json objects
     [
       {                       (json object)
         "txid": "hex",        (string, required) The transaction id
         "vout": n,            (numeric, required) The output number
         "sequence": n,        (numeric, optional, default=depends on the value of the 'replaceable' and 'locktime' arguments) The sequence number
       },
       ...
     ]
2. outputs                     (json array, required) a json array with outputs (key-value pairs), where none of the keys are duplicated.
                               That is, each address can only appear once and there can only be one 'data' object.
                               For compatibility reasons, a dictionary, which holds the key-value pairs directly, is also
                               accepted as second parameter.
     [
       {                       (json object)
         "address": amount,    (numeric or string, required) A key-value pair. The key (string) is the syscoin address, the value (float or string) is the amount in SYS
       },
       {                       (json object)
         "data": "hex",        (string, required) A key-value pair. The key must be "data", the value is hex-encoded data
       },
       ...
     ]
3. locktime                    (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
4. replaceable                 (boolean, optional, default=false) Marks this transaction as BIP125 replaceable.
                               Allows this transaction to be replaced by a transaction with higher fees. If provided, it is an error if explicit sequence numbers are incompatible.

Result:
  "psbt"        (string)  The resulting raw transaction (base64-encoded string)

Examples:
> syscoin-cli createpsbt "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"
```
</details>

<details><summary>createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )</summary>

```

Create a transaction spending the given inputs and creating new outputs.
Outputs can be addresses or data.
Returns hex-encoded raw transaction.
Note that the transaction's inputs are not signed, and
it is not stored in the wallet or transmitted to the network.

Arguments:
1. inputs                      (json array, required) A json array of json objects
     [
       {                       (json object)
         "txid": "hex",        (string, required) The transaction id
         "vout": n,            (numeric, required) The output number
         "sequence": n,        (numeric, optional, default=depends on the value of the 'replaceable' and 'locktime' arguments) The sequence number
       },
       ...
     ]
2. outputs                     (json array, required) a json array with outputs (key-value pairs), where none of the keys are duplicated.
                               That is, each address can only appear once and there can only be one 'data' object.
                               For compatibility reasons, a dictionary, which holds the key-value pairs directly, is also
                               accepted as second parameter.
     [
       {                       (json object)
         "address": amount,    (numeric or string, required) A key-value pair. The key (string) is the syscoin address, the value (float or string) is the amount in SYS
       },
       {                       (json object)
         "data": "hex",        (string, required) A key-value pair. The key must be "data", the value is hex-encoded data
       },
       ...
     ]
3. locktime                    (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
4. replaceable                 (boolean, optional, default=false) Marks this transaction as BIP125-replaceable.
                               Allows this transaction to be replaced by a transaction with higher fees. If provided, it is an error if explicit sequence numbers are incompatible.

Result:
"transaction"              (string) hex string of the transaction

Examples:
> syscoin-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"address\":0.01}]"
> syscoin-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "[{\"address\":0.01}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "[{\"data\":\"00010203\"}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>decodepsbt "psbt"</summary>

```

Return a JSON object representing the serialized, base64-encoded partially signed Syscoin transaction.

Arguments:
1. psbt    (string, required) The PSBT base64 string

Result:
{
  "tx" : {                   (json object) The decoded network-serialized unsigned transaction.
    ...                                      The layout is the same as the output of decoderawtransaction.
  },
  "unknown" : {                (json object) The unknown global fields
    "key" : "value"            (key-value pair) An unknown key-value pair
     ...
  },
  "inputs" : [                 (array of json objects)
    {
      "non_witness_utxo" : {   (json object, optional) Decoded network transaction for non-witness UTXOs
        ...
      },
      "witness_utxo" : {            (json object, optional) Transaction output for witness UTXOs
        "amount" : x.xxx,           (numeric) The value in SYS
        "scriptPubKey" : {          (json object)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
          "address" : "address"     (string) Syscoin address if there is one
        }
      },
      "partial_signatures" : {             (json object, optional)
        "pubkey" : "signature",           (string) The public key and signature that corresponds to it.
        ,...
      }
      "sighash" : "type",                  (string, optional) The sighash type to be used
      "redeem_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "witness_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "bip32_derivs" : {          (json object, optional)
        "pubkey" : {                     (json object, optional) The public key with the derivation path as the value.
          "master_fingerprint" : "fingerprint"     (string) The fingerprint of the master key
          "path" : "path",                         (string) The path
        }
        ,...
      }
      "final_scriptsig" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
        }
       "final_scriptwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
      "unknown" : {                (json object) The unknown global fields
        "key" : "value"            (key-value pair) An unknown key-value pair
         ...
      },
    }
    ,...
  ]
  "outputs" : [                 (array of json objects)
    {
      "redeem_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "witness_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
      }
      "bip32_derivs" : [          (array of json objects, optional)
        {
          "pubkey" : "pubkey",                     (string) The public key this path corresponds to
          "master_fingerprint" : "fingerprint"     (string) The fingerprint of the master key
          "path" : "path",                         (string) The path
          }
        }
        ,...
      ],
      "unknown" : {                (json object) The unknown global fields
        "key" : "value"            (key-value pair) An unknown key-value pair
         ...
      },
    }
    ,...
  ]
  "fee" : fee                      (numeric, optional) The transaction fee paid if all UTXOs slots in the PSBT have been filled.
}

Examples:
> syscoin-cli decodepsbt "psbt"
```
</details>

<details><summary>decoderawtransaction "hexstring" ( iswitness )</summary>

```

Return a JSON object representing the serialized, hex-encoded transaction.

Arguments:
1. hexstring    (string, required) The transaction hex string
2. iswitness    (boolean, optional, default=depends on heuristic tests) Whether the transaction hex is a serialized witness transaction
                If iswitness is not present, heuristic tests will be used in decoding

Result:
{
  "txid" : "id",        (string) The transaction id
  "hash" : "id",        (string) The transaction hash (differs from txid for witness transactions)
  "size" : n,             (numeric) The transaction size
  "vsize" : n,            (numeric) The virtual transaction size (differs from size for witness transactions)
  "weight" : n,           (numeric) The transaction's weight (between vsize*4 - 3 and vsize*4)
  "version" : n,          (numeric) The version
  "locktime" : ttt,       (numeric) The lock time
  "vin" : [               (array of json objects)
     {
       "txid": "id",    (string) The transaction id
       "vout": n,         (numeric) The output number
       "scriptSig": {     (json object) The script
         "asm": "asm",  (string) asm
         "hex": "hex"   (string) hex
       },
       "txinwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
       "sequence": n     (numeric) The script sequence number
     }
     ,...
  ],
  "vout" : [             (array of json objects)
     {
       "value" : x.xxx,            (numeric) The value in SYS
       "n" : n,                    (numeric) index
       "scriptPubKey" : {          (json object)
         "asm" : "asm",          (string) the asm
         "hex" : "hex",          (string) the hex
         "reqSigs" : n,            (numeric) The required sigs
         "type" : "pubkeyhash",  (string) The type, eg 'pubkeyhash'
         "addresses" : [           (json array of string)
           "12tvKAXCxZjSmdNbao16dKXC8tRWfcF5oc"   (string) syscoin address
           ,...
         ]
       }
     }
     ,...
  ],
}

Examples:
> syscoin-cli decoderawtransaction "hexstring"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "decoderawtransaction", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>decodescript "hexstring"</summary>

```

Decode a hex-encoded script.

Arguments:
1. hexstring    (string, required) the hex-encoded script

Result:
{
  "asm":"asm",          (string) Script public key
  "type":"type",        (string) The output type (e.g. nonstandard, pubkey, pubkeyhash, scripthash, multisig, nulldata, witness_v0_scripthash, witness_v0_keyhash, witness_unknown)
  "reqSigs": n,         (numeric) The required signatures
  "addresses": [        (json array of string)
     "address"          (string) syscoin address
     ,...
  ],
  "p2sh":"str"          (string) address of P2SH script wrapping this redeem script (not returned if the script is already a P2SH).
  "segwit": {           (json object) Result of a witness script public key wrapping this redeem script (not returned if the script is a P2SH or witness).
    "asm":"str",        (string) String representation of the script public key
    "hex":"hexstr",     (string) Hex string of the script public key
    "type":"str",       (string) The type of the script public key (e.g. witness_v0_keyhash or witness_v0_scripthash)
    "reqSigs": n,       (numeric) The required signatures (always 1)
    "addresses": [      (json array of string) (always length 1)
      "address"         (string) segwit address
       ,...
    ],
    "p2sh-segwit":"str" (string) address of the P2SH script wrapping this witness redeem script.
}

Examples:
> syscoin-cli decodescript "hexstring"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "decodescript", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>finalizepsbt "psbt" ( extract )</summary>

```
Finalize the inputs of a PSBT. If the transaction is fully signed, it will produce a
network serialized transaction which can be broadcast with sendrawtransaction. Otherwise a PSBT will be
created which has the final_scriptSig and final_scriptWitness fields filled for inputs that are complete.
Implements the Finalizer and Extractor roles.

Arguments:
1. psbt       (string, required) A base64 string of a PSBT
2. extract    (boolean, optional, default=true) If true and the transaction is complete,
              extract and return the complete transaction in normal network serialization instead of the PSBT.

Result:
{
  "psbt" : "value",          (string) The base64-encoded partially signed transaction if not extracted
  "hex" : "value",           (string) The hex-encoded network transaction if extracted
  "complete" : true|false,   (boolean) If the transaction has a complete set of signatures
  ]
}

Examples:
> syscoin-cli finalizepsbt "psbt"
```
</details>

<details><summary>fundrawtransaction "hexstring" ( options iswitness )</summary>

```
Add inputs to a transaction until it has enough in value to meet its out value.
This will not modify existing inputs, and will add at most one change output to the outputs.
No existing outputs will be modified unless "subtractFeeFromOutputs" is specified.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.
The inputs added will not be signed, use signrawtransactionwithkey
 or signrawtransactionwithwallet for that.
Note that all existing inputs must have their previous output transaction be in the wallet.
Note that all inputs selected must be of standard form and P2SH scripts must be
in the wallet using importaddress or addmultisigaddress (to calculate fees).
You can see whether this is the case by checking the "solvable" field in the listunspent output.
Only pay-to-pubkey, multisig, and P2SH versions thereof are currently supported for watch-only

Arguments:
1. hexstring                          (string, required) The hex string of the raw transaction
2. options                            (json object, optional) for backward compatibility: passing in a true instead of an object will result in {"includeWatching":true}
     {
       "changeAddress": "str",        (string, optional, default=pool address) The syscoin address to receive the change
       "changePosition": n,           (numeric, optional, default=random) The index of the change output
       "change_type": "str",          (string, optional, default=set by -changetype) The output type to use. Only valid if changeAddress is not specified. Options are "legacy", "p2sh-segwit", and "bech32".
       "includeWatching": bool,       (boolean, optional, default=false) Also select inputs which are watch only
       "lockUnspents": bool,          (boolean, optional, default=false) Lock selected unspent outputs
       "feeRate": amount,             (numeric or string, optional, default=not set: makes wallet determine the fee) Set a specific fee rate in SYS/kB
       "subtractFeeFromOutputs": [    (json array, optional, default=empty array) A json array of integers.
                                      The fee will be equally deducted from the amount of each specified output.
                                      Those recipients will receive less syscoins than you enter in their corresponding amount field.
                                      If no outputs are specified here, the sender pays the fee.
         vout_index,                  (numeric) The zero-based output index, before a change output is added.
         ...
       ],
       "replaceable": bool,           (boolean, optional, default=fallback to wallet's default) Marks this transaction as BIP125 replaceable.
                                      Allows this transaction to be replaced by a transaction with higher fees
       "conf_target": n,              (numeric, optional, default=fallback to wallet's default) Confirmation target (in blocks)
       "estimate_mode": "str",        (string, optional, default=UNSET) The fee estimate mode, must be one of:
                                      "UNSET"
                                      "ECONOMICAL"
                                      "CONSERVATIVE"
     }
3. iswitness                          (boolean, optional, default=depends on heuristic tests) Whether the transaction hex is a serialized witness transaction 
                                      If iswitness is not present, heuristic tests will be used in decoding

Result:
{
  "hex":       "value", (string)  The resulting raw transaction (hex-encoded string)
  "fee":       n,         (numeric) Fee in SYS the resulting transaction pays
  "changepos": n          (numeric) The position of the added change output, or -1
}

Examples:

Create a transaction with no inputs
> syscoin-cli createrawtransaction "[]" "{\"myaddress\":0.01}"

Add sufficient unsigned inputs to meet the output value
> syscoin-cli fundrawtransaction "rawtransactionhex"

Sign the transaction
> syscoin-cli signrawtransactionwithwallet "fundedtransactionhex"

Send the transaction
> syscoin-cli sendrawtransaction "signedtransactionhex"
```
</details>

<details><summary>getrawtransaction "txid" ( verbose "blockhash" )</summary>

```

Return the raw transaction data.

By default this function only works for mempool transactions. When called with a blockhash
argument, getrawtransaction will return the transaction if the specified block is available and
the transaction is found in that block. When called without a blockhash argument, getrawtransaction
will return the transaction if it is in the mempool, or if -txindex is enabled and the transaction
is in a block in the blockchain.

Hint: Use gettransaction for wallet transactions.

If verbose is 'true', returns an Object with information about 'txid'.
If verbose is 'false' or omitted, returns a string that is serialized, hex-encoded data for 'txid'.

Arguments:
1. txid         (string, required) The transaction id
2. verbose      (boolean, optional, default=false) If false, return a string, otherwise return a json object
3. blockhash    (string, optional) The block in which to look for the transaction

Result (if verbose is not set or set to false):
"data"      (string) The serialized, hex-encoded data for 'txid'

Result (if verbose is set to true):
{
  "in_active_chain": b, (bool) Whether specified block is in the active chain or not (only present with explicit "blockhash" argument)
  "hex" : "data",       (string) The serialized, hex-encoded data for 'txid'
  "txid" : "id",        (string) The transaction id (same as provided)
  "hash" : "id",        (string) The transaction hash (differs from txid for witness transactions)
  "size" : n,             (numeric) The serialized transaction size
  "vsize" : n,            (numeric) The virtual transaction size (differs from size for witness transactions)
  "weight" : n,           (numeric) The transaction's weight (between vsize*4-3 and vsize*4)
  "version" : n,          (numeric) The version
  "locktime" : ttt,       (numeric) The lock time
  "vin" : [               (array of json objects)
     {
       "txid": "id",    (string) The transaction id
       "vout": n,         (numeric) 
       "scriptSig": {     (json object) The script
         "asm": "asm",  (string) asm
         "hex": "hex"   (string) hex
       },
       "sequence": n      (numeric) The script sequence number
       "txinwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
     }
     ,...
  ],
  "vout" : [              (array of json objects)
     {
       "value" : x.xxx,            (numeric) The value in SYS
       "n" : n,                    (numeric) index
       "scriptPubKey" : {          (json object)
         "asm" : "asm",          (string) the asm
         "hex" : "hex",          (string) the hex
         "reqSigs" : n,            (numeric) The required sigs
         "type" : "pubkeyhash",  (string) The type, eg 'pubkeyhash'
         "addresses" : [           (json array of string)
           "address"        (string) syscoin address
           ,...
         ]
       }
     }
     ,...
  ],
  "blockhash" : "hash",   (string) the block hash
  "confirmations" : n,      (numeric) The confirmations
  "blocktime" : ttt         (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "time" : ttt,             (numeric) Same as "blocktime"
}

Examples:
> syscoin-cli getrawtransaction "mytxid"
> syscoin-cli getrawtransaction "mytxid" true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getrawtransaction", "params": ["mytxid", true] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> syscoin-cli getrawtransaction "mytxid" false "myblockhash"
> syscoin-cli getrawtransaction "mytxid" true "myblockhash"
```
</details>

<details><summary>joinpsbts ["psbt",...]</summary>

```

Joins multiple distinct PSBTs with different inputs and outputs into one PSBT with inputs and outputs from all of the PSBTs
No input in any of the PSBTs can be in more than one of the PSBTs.

Arguments:
1. txs            (json array, required) A json array of base64 strings of partially signed transactions
     [
       "psbt",    (string, required) A base64 string of a PSBT
       ...
     ]

Result:
  "psbt"          (string) The base64-encoded partially signed transaction

Examples:
> syscoin-cli joinpsbts "psbt"
```
</details>

<details><summary>sendrawtransaction "hexstring" ( maxfeerate )</summary>

```

Submits raw transaction (serialized, hex-encoded) to local node and network.

Also see createrawtransaction and signrawtransactionwithkey calls.

Arguments:
1. hexstring     (string, required) The hex string of the raw transaction
2. maxfeerate    (numeric or string, optional, default=0.10) Reject transactions whose fee rate is higher than the specified value, expressed in SYS/kB.
                 Set to 0 to accept any fee rate.
                 

Result:
"hex"             (string) The transaction hash in hex

Examples:

Create a transaction
> syscoin-cli createrawtransaction "[{\"txid\" : \"mytxid\",\"vout\":0}]" "{\"myaddress\":0.01}"
Sign the transaction, and get back the hex
> syscoin-cli signrawtransactionwithwallet "myhex"

Send the transaction (signed hex)
> syscoin-cli sendrawtransaction "signedhex"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendrawtransaction", "params": ["signedhex"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>signrawtransactionwithkey "hexstring" ["privatekey",...] ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )</summary>

```

Sign inputs for raw transaction (serialized, hex-encoded).
The second argument is an array of base58-encoded private
keys that will be the only keys used to sign the transaction.
The third optional argument (may be null) is an array of previous transaction outputs that
this transaction depends on but may not yet be in the block chain.

Arguments:
1. hexstring                        (string, required) The transaction hex string
2. privkeys                         (json array, required) A json array of base58-encoded private keys for signing
     [
       "privatekey",                (string) private key in base58-encoding
       ...
     ]
3. prevtxs                          (json array, optional) A json array of previous dependent transaction outputs
     [
       {                            (json object)
         "txid": "hex",             (string, required) The transaction id
         "vout": n,                 (numeric, required) The output number
         "scriptPubKey": "hex",     (string, required) script key
         "redeemScript": "hex",     (string) (required for P2SH) redeem script
         "witnessScript": "hex",    (string) (required for P2WSH or P2SH-P2WSH) witness script
         "amount": amount,          (numeric or string) (required for Segwit inputs) the amount spent
       },
       ...
     ]
4. sighashtype                      (string, optional, default=ALL) The signature hash type. Must be one of:
                                    "ALL"
                                    "NONE"
                                    "SINGLE"
                                    "ALL|ANYONECANPAY"
                                    "NONE|ANYONECANPAY"
                                    "SINGLE|ANYONECANPAY"
                                    

Result:
{
  "hex" : "value",                  (string) The hex-encoded raw transaction with signature(s)
  "complete" : true|false,          (boolean) If the transaction has a complete set of signatures
  "errors" : [                      (json array of objects) Script verification errors (if there are any)
    {
      "txid" : "hash",              (string) The hash of the referenced, previous transaction
      "vout" : n,                   (numeric) The index of the output to spent and used as input
      "scriptSig" : "hex",          (string) The hex-encoded signature script
      "sequence" : n,               (numeric) Script sequence number
      "error" : "text"              (string) Verification or signing error related to the input
    }
    ,...
  ]
}

Examples:
> syscoin-cli signrawtransactionwithkey "myhex"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "signrawtransactionwithkey", "params": ["myhex"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>testmempoolaccept ["rawtx",...] ( maxfeerate )</summary>

```

Returns result of mempool acceptance tests indicating if raw transaction (serialized, hex-encoded) would be accepted by mempool.

This checks if the transaction violates the consensus or policy rules.

See sendrawtransaction call.

Arguments:
1. rawtxs          (json array, required) An array of hex strings of raw transactions.
                   Length must be one for now.
     [
       "rawtx",    (string)
       ...
     ]
2. maxfeerate      (numeric or string, optional, default=0.10) Reject transactions whose fee rate is higher than the specified value, expressed in SYS/kB
                   

Result:
[                   (array) The result of the mempool acceptance test for each raw transaction in the input array.
                            Length is exactly one for now.
 {
  "txid"           (string) The transaction hash in hex
  "allowed"        (boolean) If the mempool allows this tx to be inserted
  "reject-reason"  (string) Rejection string (only present when 'allowed' is false)
 }
]

Examples:

Create a transaction
> syscoin-cli createrawtransaction "[{\"txid\" : \"mytxid\",\"vout\":0}]" "{\"myaddress\":0.01}"
Sign the transaction, and get back the hex
> syscoin-cli signrawtransactionwithwallet "myhex"

Test acceptance of the transaction (signed hex)
> syscoin-cli testmempoolaccept ["signedhex"]

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "testmempoolaccept", "params": [["signedhex"]] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>utxoupdatepsbt "psbt"</summary>

```

Updates a PSBT with witness UTXOs retrieved from the UTXO set or the mempool.

Arguments:
1. psbt    (string, required) A base64 string of a PSBT

Result:
  "psbt"          (string) The base64-encoded partially signed transaction with inputs updated

Examples:
> syscoin-cli utxoupdatepsbt "psbt"
```
</details>

## Syscoin

<details><summary>addressbalance "address"</summary>

```

Show the Syscoin balance of an address

Arguments:
1. address    (string, required) Address to holding the balance

Result:
{
  "amount": xx            (numeric) Syscoin balance of the address
}

Examples:
> syscoin-cli addressbalance "sysrt1qea3v4dj5kjxjgtysdxd3mszjz56530ugw467dq"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "addressbalance", "params": ["sysrt1qea3v4dj5kjxjgtysdxd3mszjz56530ugw467dq"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationbalance asset_guid "address"</summary>

```

Show stored balance of a single asset allocation.

Arguments:
1. asset_guid    (numeric, required) The guid of the asset
2. address       (string, required) The address of the allocation owner

Result:
{
  "amount": xx        (numeric) The balance of a single asset allocation.
}

Examples:
> syscoin-cli assetallocationbalance "asset_guid" "address"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationbalance", "params": ["asset_guid", "address"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationinfo asset_guid "address"</summary>

```

Show stored values of a single asset allocation.

Arguments:
1. asset_guid    (numeric, required) The guid of the asset
2. address       (string, required) The address of the owner

Result:
{
    "asset_allocation":   (string) The unique key for this allocation
    "asset_guid":         (string) The guid of the asset
    "symbol":             (string) The asset symbol
    "address":            (string) The address of the owner of this allocation
    "balance":            (numeric) The current balance
    "balance_zdag":       (numeric) The zdag balance
    "locked_outpoint":    (string) The locked UTXO if applicable for this allocation
}

Examples:
> syscoin-cli assetallocationinfo "assetguid" "address"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationinfo", "params": ["assetguid", "address"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationsenderstatus <asset_guid> <address> <txid></summary>

```
Show status as it pertains to any current Z-DAG conflicts or warnings related to a sender or sender/txid combination of an asset allocation transfer. Leave txid empty if you are not checking for a specific transfer.
Return value is in the status field and can represent 3 levels(0, 1 or 2)
Level -1 means not found, not a ZDAG transaction, perhaps it is already confirmed.
Level 0 means OK.
Level 1 means warning (checked that in the mempool there are more spending balances than current POW sender balance). An active stance should be taken and perhaps a deeper analysis as to potential conflicts related to the sender.
Level 2 means an active double spend was found and any depending asset allocation sends are also flagged as dangerous and should wait for POW confirmation before proceeding.

Arguments:
1. "asset_guid":      (numeric, required) The guid of the asset
2. "address":    (string, required) The address of the sender
3. "txid":       (string, required) The transaction id of the assetallocationsend

Result:
{
  "status":      (numeric) The status level of the transaction
}

Examples:
> syscoin-cli assetallocationsenderstatus "asset_guid" "address" "txid"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationsenderstatus", "params": ["asset_guid", "address", "txid"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetinfo asset_guid</summary>

```

Show stored values of a single asset and its.

Arguments:
1. asset_guid    (numeric, required) The asset guid

Result:
{
  "asset_guid":          (numeric) The asset guid
  "txid":         (string) The transaction id that created this asset
  "public_value":  (string) The public value attached to this asset
  "address":      (string) The address that controls this address
  "contract":     (string) The ethereum contract address
  "balance":      (numeric) The current balance
  "total_supply": (numeric) The total supply of this asset
  "max_supply":   (numeric) The maximum supply of this asset
  "update_flag":  (numeric) The flag in decimal 
  "precision":    (numeric) The precision of this asset 
}

Examples:
> syscoin-cli assetinfo "assetguid"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetinfo", "params": ["assetguid"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>convertaddress "address"</summary>

```

Convert between Syscoin 3 and Syscoin 4 formats. P2WPKH can be shown as P2PKH in Syscoin 3.

Arguments:
1. address    (string, required) The syscoin address to get the information of.

Result:
{
  "bitcoinaddress" : "address",   (string) The syscoin 3 or Bitcoin address validated
  "v3address" : "address",        (string) The syscoin 3 address validated
  "v4address" : "address",        (string) The syscoin 4 address validated
}

Examples:
> syscoin-cli convertaddress "sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "convertaddress", "params": ["sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getblockhashbytxid "txid"</summary>

```

Returns hash of block in best-block-chain at txid provided.

Arguments:
1. txid    (string, required) A transaction that is in the block.

Result:
{
  "hex": "hexstring"     (string) The block hash that contains the txid
}

Examples:
> syscoin-cli getblockhashbytxid dfc7eac24fa89b0226c64885f7bedaf132fc38e8980b5d446d76707027254490
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockhashbytxid", "params": [dfc7eac24fa89b0226c64885f7bedaf132fc38e8980b5d446d76707027254490] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassetallocationmempoolbalances ( count from {"addresses_array":["sender_address",...]} )</summary>

```

Scan through all asset allocation mempool balances. Useful for ZDAG analysis on senders of allocations.

Arguments:
1. count                       (numeric, optional, default=10) The number of results to return.
2. from                        (numeric, optional, default=0) The number of results to skip.
3. options                     (json object) A json object with options to filter results.
     {
       "addresses_array": [    (json array) A json array with owners
         "sender_address",     (string) Address to filter
         ...
       ],
     }

Examples:
> syscoin-cli listassetallocationmempoolbalances 0
> syscoin-cli listassetallocationmempoolbalances 10 10
> syscoin-cli listassetallocationmempoolbalances 0 0 '{"senders":[{"address":"sysrt1q9hrtqlcpvd089hswwa3gtsy29f8pugc3wah3fl"},{"address":"sysrt1qea3v4dj5kjxjgtysdxd3mszjz56530ugw467dq"}]}'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocationmempoolbalances", "params": [0] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocationmempoolbalances", "params": [10, 10] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocationmempoolbalances", "params": [0, 0, '{"senders":[{"address":"sysrt1q9hrtqlcpvd089hswwa3gtsy29f8pugc3wah3fl"},{"address":"sysrt1qea3v4dj5kjxjgtysdxd3mszjz56530ugw467dq"}]}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassetallocations ( count from {"asset_guid":n,"addresses":["address",...]} )</summary>

```

Scan through all asset allocations.

Arguments:
1. count                   (numeric, optional, default=10) The number of results to return.
2. from                    (numeric, optional, default=0) The number of results to skip.
3. options                 (json object) A json object with options to filter results.
     {
       "asset_guid": n,    (numeric) Asset GUID to filter
       "addresses": [      (json array) A json array with owners
         "address",        (string) Address to filter
         ...
       ],
     }

Result:
[
  {
    "asset_allocation":   (string) The unique key for this allocation
    "asset_guid":         (string) The guid of the asset
    "symbol":             (string) The asset symbol
    "address":            (string) The address of the owner of this allocation
    "balance":            (numeric) The current balance
    "balance_zdag":       (numeric) The zdag balance
    "locked_outpoint":    (string) The locked UTXO if applicable for this allocation
  }
  ...
]

Examples:
> syscoin-cli listassetallocations 0
> syscoin-cli listassetallocations 10 10
> syscoin-cli listassetallocations 0 0 '{"asset_guid":92922}'
> syscoin-cli listassetallocations 0 0 '{"addresses":[{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"},{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}]}'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocations", "params": [0] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocations", "params": [10, 10] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocations", "params": [0, 0, '{"asset_guid":92922}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetallocations", "params": [0, 0, '{"addresses":[{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"},{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}]}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassetindex ( page ) [asset_guid,"address",...]</summary>

```

Scan through all asset index and return paged results based on page number passed in. Requires assetindex config parameter enabled and optional assetindexpagesize which is 25 by default.

Arguments:
1. page               (numeric, optional, default=0) Return specific page number of transactions. Lower page number means more recent transactions.
2. options            (json array, required) A json object with options to filter results
     [
       asset_guid,    (numeric, required) Asset GUID to filter.
       "address",     (string) Address to filter.  Leave empty to scan globally through asset
       ...
     ]

Result:
[
  {
    "asset_guid":   (numeric) The asset guid
    "symbol":       (string) The asset symbol
    "txid":         (string) The transaction id that created this asset
    "public_value":  (string) The public value attached to this asset
    "address":      (string) The address that controls this address
    "contract":     (string) The ethereum contract address
    "balance":      (numeric) The current balance
    "total_supply": (numeric) The total supply of this asset
    "max_supply":   (numeric) The maximum supply of this asset
    "update_flag":  (numeric) The flag in decimal 
    "precision":    (numeric) The precision of this asset 
  },
  ...
]

Examples:
> syscoin-cli listassetindex 0 '{"asset_guid":92922}'
> syscoin-cli listassetindex 2 '{"asset_guid":92922, "address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetindex", "params": [0, '{"asset_guid":92922}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetindex", "params": [2, '{"asset_guid":92922, "address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassetindexallocations address</summary>

```

Return a list of asset allocations an address is associated with.

Arguments:
1. address    (numeric, required) Address to find assets associated with.

Result:
[
  {
    "asset_allocation":   (string) The unique key for this allocation
    "asset_guid":         (string) The guid of the asset
    "symbol":             (string) The asset symbol
    "address":            (string) The address of the owner of this allocation
    "balance":            (numeric) The current balance
    "balance_zdag":       (numeric) The zdag balance
    "locked_outpoint":    (string) The locked UTXO if applicable for this allocation
  },
  ...
]

Examples:
> syscoin-cli listassetindexallocations sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetindexallocations", "params": [sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassetindexassets address</summary>

```

Return a list of assets an address is associated with.

Arguments:
1. address    (numeric, required) Address to find assets associated with.

Result:
[
  {
    "asset_guid":   (numeric) The asset guid
    "symbol":       (string) The asset symbol
    "txid":         (string) The transaction id that created this asset
    "public_value":  (string) The public value attached to this asset
    "address":      (string) The address that controls this address
    "contract":     (string) The ethereum contract address
    "balance":      (numeric) The current balance
    "total_supply": (numeric) The total supply of this asset
    "max_supply":   (numeric) The maximum supply of this asset
    "update_flag":  (numeric) The flag in decimal 
    "precision":    (numeric) The precision of this asset 
  },
  ...
]

Examples:
> syscoin-cli listassetindexassets sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassetindexassets", "params": [sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listassets ( count from {"txid":"str","asset_guid":n,"addresses":["address",...]} )</summary>

```

Scan through all assets.

Arguments:
1. count                   (numeric, optional, default=10) The number of results to return.
2. from                    (numeric, optional, default=0) The number of results to skip.
3. options                 (json object) A json object with options to filter results.
     {
       "txid": "str",      (string) Transaction ID to filter results for
       "asset_guid": n,    (numeric) Asset GUID to filter
       "addresses": [      (json array) A json array with owners
         "address",        (string) Address to filter
         ...
       ],
     }

Result:
[
  {
    "asset_guid":   (numeric) The asset guid
    "symbol":       (string) The asset symbol
    "txid":         (string) The transaction id that created this asset
    "public_value":  (string) The public value attached to this asset
    "address":      (string) The address that controls this address
    "contract":     (string) The ethereum contract address
    "balance":      (numeric) The current balance
    "total_supply": (numeric) The total supply of this asset
    "max_supply":   (numeric) The maximum supply of this asset
    "update_flag":  (numeric) The flag in decimal 
    "precision":    (numeric) The precision of this asset 
  },
  ...
]

Examples:
> syscoin-cli listassets 0
> syscoin-cli listassets 10 10
> syscoin-cli listassets 0 0 '{"addresses":[{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"},{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}]}'
> syscoin-cli listassets 0 0 '{"asset_guid":3473733}'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassets", "params": [0, 0, '{"addresses":[{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"},{"address":"sys1qw40fdue7g7r5ugw0epzk7xy24tywncm26hu4a7"}]}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listassets", "params": [0, 0, '{"asset_guid":3473733}'] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoindecoderawtransaction "hexstring"</summary>

```
Decode raw syscoin transaction (serialized, hex-encoded) and display information pertaining to the service that is included in the transactiion data output(OP_RETURN)

Arguments:
1. hexstring    (string, required) The transaction hex string.

Result:
{
  "txtype" : "txtype",         (string) The syscoin transaction type
  "asset_guid" : n,              (numeric) The asset guid
  "symbol" : "symbol",         (string) The asset symbol
  "txid" : "id",               (string) The transaction id
  "height" : n,                  (numeric) The blockheight of the transaction 
  "sender" : "address",        (string) The address of the sender
  "allocations" : [              (array of json objects)
    {
      "address": "address",    (string) The address of the receiver
      "amount" : n,              (numeric) The amount of the transaction
    },
    ...
  ]
  "total" : n,                   (numeric) The total amount in this transaction
  "confirmed" : true|false       (boolean) If the transaction is confirmed
}

Examples:
> syscoin-cli syscoindecoderawtransaction "hexstring"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoindecoderawtransaction", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoingetspvproof "txid" ( "blockhash" )</summary>

```
Returns SPV proof for use with inter-chain transfers.

Arguments:
1. txid         (string, required) A transaction that is in the block
2. blockhash    (string, optional, default="") Block containing txid

Result:
"proof"         (string) JSON representation of merkl/ nj   nk ne proof (transaction index, siblings and block header and some other information useful for moving coins/assets to another chain)

Examples:
> syscoin-cli syscoingetspvproof dfc7eac24fa89b0226c64885f7bedaf132fc38e8980b5d446d76707027254490
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoingetspvproof", "params": [dfc7eac24fa89b0226c64885f7bedaf132fc38e8980b5d446d76707027254490] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/

```
</details>


<details><summary>syscoinsetethheaders [blocknumber, blockhash, previoushash, txroot, txreceiptroot] ...</summary>

```
Sets Ethereum headers in Syscoin to validate transactions through the SYSX bridge.

Arguments:
1. headers                   (json array, required) An array of arrays (block number, tx root) from Ethereum blockchain
     [
       [                     (json array) An array of [block number, tx root] 
         block_number,       (numeric) The block height number
         "block_hash",       (string) Hash of the block
         "previous_hash",    (string) Hash of the previous block
         "tx_root",          (string) The Ethereum TX root of the block height
         "receipt_root",     (string) The Ethereum TX Receipt root of the block height
         ...
       ],
       ...
     ]

Result:
{
    "status": xx     (string) Result
}

Examples:
> syscoin-cli syscoinsetethheaders "[[7043888,\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\",\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\",\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\"],...]"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinsetethheaders", "params": ["[[7043888,\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\",\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\",\"0xd8ac75c7b4084c85a89d6e28219ff162661efb8b794d4b66e6e9ea52b4139b10\"],...]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoinsetethstatus "syncing_status" highest_block</summary>

```

Sets ethereum syncing and network status for indication status of network sync.

Arguments:
1. syncing_status    (string, required) Sycning status ether 'syncing' or 'synced'
2. highest_block     (numeric, required) What the highest block height on Ethereum is found to be.  Usually coupled with syncing_status of 'syncing'.  Set to 0 if sync_status is 'synced'

Result:
{
    "status": xx     (string) Result
}

Examples:
> syscoin-cli syscoinsetethstatus "syncing" 7000000
> syscoin-cli syscoinsetethstatus "synced" 0
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinsetethstatus", "params": ["syncing", 7000000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinsetethstatus", "params": ["synced", 0] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoinstartgeth</summary>

```

Starts Geth and the relayer.

Result:
{
    "status": xx     (string) Result
}

Examples:
> syscoin-cli syscoinstartgeth 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinstartgeth", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoinstopgeth</summary>

```

Stops Geth and the relayer from running.

Result:
{
    "status": xx     (string) Result
}

Examples:
> syscoin-cli syscoinstopgeth 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinstopgeth", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>tpstestadd [starttime] [{"tx":"hex"},...]</summary>

```

Adds raw transactions to the test raw tx queue to be sent to the network at starttime.

Arguments:
1. starttime                  (numeric, required) Unix epoch time in micro seconds for when to send the raw transaction queue to the network. If set to 0, will not send transactions until you call this function again with a defined starttime.
2. "raw transactions"                (array, not-required) A json array of signed raw transaction strings
     [
       {
         "tx":"hex",    (string, required) The transaction hex
       } 
       ,...
     ]

Example:
> syscoin-cli tpstestadd "223233433839384" "[{\"tx\":\"first raw hex tx\"},{\"tx\":\"second raw hex tx\"}]"
```
</details>

<details><summary>tpstestinfo</summary>

```
Gets TPS Test information for receivers of assetallocation transfers
```
</details>

<details><summary>tpstestsetenabled [enabled]</summary>

```

Set TPS Test to enabled/disabled state. Must have -tpstest configuration set to make this call.

Arguments:
1. enabled                  (boolean, required) TPS Test enabled state. Set to true for enabled and false for disabled.

Example:
> syscoin-cli tpstestsetenabled true
```
</details>

## Syscoinwallet


<details><summary>assetallocationburn asset_guid "address" amount "ethereum_destination_address"</summary>

```

Burn an asset allocation in order to use the bridge

Arguments:
1. asset_guid                      (numeric, required) Asset guid
2. address                         (string, required) Address that owns this asset allocation
3. amount                          (numeric or string, required) Amount of asset to burn to SYSX
4. ethereum_destination_address    (string, required) The 20 byte (40 character) hex string of the ethereum destination address. Leave empty to burn as normal without the bridge.  If it is left empty this will process as a normal assetallocationsend to the burn address

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetallocationburn "asset_guid" "address" "amount" "ethereum_destination_address"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationburn", "params": ["asset_guid", "address", "amount", "ethereum_destination_address"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationlock asset_guid "addressfrom" "txid" output_index ( "witness" )</summary>

```

Lock an asset allocation to a specific UTXO (txid/output). This is useful for things such as hashlock and CLTV type operations where script checks are done on UTXO prior to spending which extend to an assetallocationsend.

Arguments:
1. asset_guid      (numeric, required) Asset guid
2. addressfrom     (string, required) Address that owns this asset allocation
3. txid            (string, required) Transaction hash
4. output_index    (numeric, required) Output index inside the transaction output array
5. witness         (string, optional, default="") Witness address that will sign for web-of-trust notarization of this transaction

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetallocationlock "asset_guid" "addressfrom" "txid" "output_index" ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationlock", "params": ["asset_guid","addressfrom","txid","output_index",""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationmint asset_guid "address" amount blocknumber "tx_hex" "txroot_hex" "txmerkleproof_hex" "merklerootpath_hex" "receipt_hex" "receiptroot_hex" "receiptmerkleproof_hex" ( "witness" )</summary>

```

Mint assetallocation to come back from the bridge

Arguments:
1. asset_guid                 (numeric, required) Asset guid
2. address                    (string, required) Mint to this address.
3. amount                     (numeric or string, required) Amount of asset to mint.  Note that fees will be taken from the owner address
4. blocknumber                (numeric, required) Block number of the block that included the burn transaction on Ethereum.
5. tx_hex                     (string, required) Transaction hex.
6. txroot_hex                 (string, required) The transaction merkle root that commits this transaction to the block header.
7. txmerkleproof_hex          (string, required) The list of parent nodes of the Merkle Patricia Tree for SPV proof of transaction merkle root.
8. merklerootpath_hex         (string, required) The merkle path to walk through the tree to recreate the merkle hash for both transaction and receipt root.
9. receipt_hex                (string, required) Transaction Receipt Hex.
10. receiptroot_hex           (string, required) The transaction receipt merkle root that commits this receipt to the block header.
11. receiptmerkleproof_hex    (string, required) The list of parent nodes of the Merkle Patricia Tree for SPV proof of transaction receipt merkle root.
12. witness                   (string, optional, default="") Witness address that will sign for web-of-trust notarization of this transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetallocationmint "assetguid" "address" "amount" "blocknumber" "tx_hex" "txroot_hex" "txmerkleproof_hex" "txmerkleproofpath_hex" "receipt_hex" "receiptroot_hex" "receiptmerkleproof" "witness"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationmint", "params": ["assetguid", "address", "amount", "blocknumber", "tx_hex", "txroot_hex", "txmerkleproof_hex", "txmerkleproofpath_hex", "receipt_hex", "receiptroot_hex", "receiptmerkleproof", ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationsend asset_guid "address_sender" "address_receiver" amount</summary>

```

Send an asset allocation you own to another address.

Arguments:
1. asset_guid          (numeric, required) The asset guid
2. address_sender      (string, required) The address to send the allocation from
3. address_receiver    (string, required) The address to send the allocation to
4. amount              (numeric or string, required) The quantity of asset to send

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetallocationsend "assetguid" "addressfrom" "address" "amount"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationsend", "params": ["assetguid", "addressfrom", "address", "amount"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetallocationsendmany asset_guid "addressfrom" [assetallocationsend object]... ( "witness" )</summary>

```

Send an asset allocation you own to another address. Maximum recipients is 250.

Arguments:
1. asset_guid                 (numeric, required) Asset guid
2. addressfrom                (string, required) Address that owns this asset allocation
3. amounts                    (json array, required) Array of assetallocationsend objects
     [
       {                      (json object) The assetallocationsend object
         "address": "str",    (string) Address to transfer to
         "amount": amount,    (numeric or string) Quantity of asset to send
       },
       ...
     ]
4. witness                    (string, optional, default="") Witness address that will sign for web-of-trust notarization of this transaction

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetallocationsendmany "assetguid" "addressfrom" '[{"address":"sysaddress1","amount":100},{"address":"sysaddress2","amount":200}]' ""
> syscoin-cli assetallocationsendmany "assetguid" "addressfrom" "[{\"address\":\"sysaddress1\",\"amount\":100},{\"address\":\"sysaddress2\",\"amount\":200}]" ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationsendmany", "params": ["assetguid", "addressfrom", '[{"address":"sysaddress1","amount":100},{"address":"sysaddress2","amount":200}]', ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetallocationsendmany", "params": ["assetguid", "addressfrom", "[{\"address\":\"sysaddress1\",\"amount\":100},{\"address\":\"sysaddress2\",\"amount\":200}]", ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetnew "address" "symbol" "public_value" "contract" precision total_supply max_supply update_flags "witness"</summary>

```

Create a new asset

Arguments:
1. address         (string, required) An address that you own.
2. symbol          (string, required) Asset symbol (1-8 characters)
3. public_value    (string, required) public data, 256 characters max.
4. contract        (string, required) Ethereum token contract for SyscoinX bridge. Must be in hex and not include the '0x' format tag. For example contract '0xb060ddb93707d2bc2f8bcc39451a5a28852f8d1d' should be set as 'b060ddb93707d2bc2f8bcc39451a5a28852f8d1d'. Leave empty for no smart contract bridge.
5. precision       (numeric, required) Precision of balances. Must be between 0 and 8. The lower it is the higher possible max_supply is available since the supply is represented as a 64 bit integer. With a precision of 8 the max supply is 10 billion.
6. total_supply    (numeric or string, required) Initial supply of asset. Can mint more supply up to total_supply amount or if total_supply is -1 then minting is uncapped.
7. max_supply      (numeric or string, required) Maximum supply of this asset. Set to -1 for uncapped. Depends on the precision value that is set, the lower the precision the higher max_supply can be.
8. update_flags    (numeric, required) Ability to update certain fields. Must be decimal value which is a bitmask for certain rights to update. The bitmask represents 0x01(1) to give admin status (needed to update flags), 0x10(2) for updating public data field, 0x100(4) for updating the smart contract field, 0x1000(8) for updating supply, 0x10000(16) for being able to update flags (need admin access to update flags as well). 0x11111(31) for all.
9. witness         (string, required) Witness address that will sign for web-of-trust notarization of this transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
  "assetguid": xxxx        (numeric) The guid of asset to be created
}

Examples:
> syscoin-cli assetnew "myaddress" "CAT" "publicvalue" "contractaddr" 8 100 1000 31 ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetnew", "params": ["myaddress", "CAT", "publicvalue", "contractaddr", 8, 100, 1000, 31, ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetsend asset_guid "address" amount</summary>

```

Send an asset you own to another address.

Arguments:
1. asset_guid    (numeric, required) The asset guid.
2. address       (string, required) The address to send the asset to (creates an asset allocation).
3. amount        (numeric or string, required) The quantity of asset to send.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetsend "assetguid" "address" "amount"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetsend", "params": ["assetguid", "address", "amount"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetsendmany asset_guid [assetsendobjects,...] ( "witness" )</summary>

```

Send an asset you own to another address/addresses as an asset allocation. Maximum recipients is 250.

Arguments:
1. asset_guid                 (numeric, required) Asset guid.
2. array                      (json array, required) Array of asset send objects.
     [
       {                      (json object, required) An assetsend obj
         "address": "str",    (string, required) Address to transfer to
         "amount": amount,    (numeric or string, required) Quantity of asset to send
       },
       ...
     ]
3. witness                    (string, optional, default="") Witnesses address that will sign for web-of-trust notarization of this transaction

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetsendmany "assetguid" '[{"address":"sysaddress1","amount":100},{"address":"sysaddress2","amount":200}]' ""
> syscoin-cli assetsendmany "assetguid" "[{\"address\":\"sysaddress1\",\"amount\":100},{\"address\":\"sysaddress2\",\"amount\":200}]" ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetsendmany", "params": ["assetguid",'[{"address":"sysaddress1","amount":100},{"address":"sysaddress2","amount":200}]' ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetsendmany", "params": ["assetguid","[{\"address\":\"sysaddress1\",\"amount\":100},{\"address\":\"sysaddress2\",\"amount\":200}]" ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assettransfer asset_guid "address" "witness"</summary>

```

Transfer an asset you own to another address.

Arguments:
1. asset_guid    (numeric, required) Asset guid.
2. address       (string, required) Address to transfer to.
3. witness       (string, required) Witness address that will sign for web-of-trust notarization of this transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assettransfer "asset_guid" "address" ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assettransfer", "params": ["asset_guid", "address", ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>assetupdate asset_guid "public_value" "contract" supply update_flags "witness"</summary>

```

Perform an update on an asset you control.

Arguments:
1. asset_guid      (numeric, required) Asset guid
2. public_value    (string, required) Public data, 256 characters max.
3. contract        (string, required) Ethereum token contract for SyscoinX bridge. Leave empty for no smart contract bridg.
4. supply          (numeric or string, required) New supply of asset. Can mint more supply up to total_supply amount or if max_supply is -1 then minting is uncapped. If greator than zero, minting is assumed otherwise set to 0 to not mint any additional tokens.
5. update_flags    (numeric, required) Ability to update certain fields. Must be decimal value which is a bitmask for certain rights to update. The bitmask represents 0x01(1) to give admin status (needed to update flags), 0x10(2) for updating public data field, 0x100(4) for updating the smart contract field, 0x1000(8) for updating supply, 0x10000(16) for being able to update flags (need admin access to update flags as well). 0x11111(31) for all.
6. witness         (string, required) Witness address that will sign for web-of-trust notarization of this transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli assetupdate "assetguid" "publicvalue" "contractaddress" "supply" "update_flags" ""
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "assetupdate", "params": ["assetguid", "publicvalue", "contractaddress", "supply", "update_flags", ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoinburn "funding_address" amount "ethereum_destination_address"</summary>

```

Burns the syscoin for bridging to Ethereum token

Arguments:
1. funding_address                 (string, required) Funding address to burn SYS from
2. amount                          (numeric or string, required) Amount of SYS to burn. Note that fees are applied on top. It is not inclusive of fees.
3. ethereum_destination_address    (string, required) The 20 bytes (40 character) hex string of the ethereum destination address.  Leave empty to burn as normal without the bridge

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli syscoinburn "funding_address" "amount" "ethaddress"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinburn", "params": ["funding_address", "amount", "ethaddress"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscoinmint "address" amount blocknumber "tx_hex" "txroot_hex" "txmerkleproof_hex" "merklerootpath_hex" "receipt_hex" "receiptroot_hex" "receiptmerkleproof_hex" ( "witness" )</summary>

```

Mint syscoin to come back from the ethereum bridge

Arguments:
1. address                    (string, required) Mint to this address.
2. amount                     (numeric or string, required) Amount of SYS to mint.  Note that fees are applied on top.  It is not inclusive of fees
3. blocknumber                (numeric, required) Block number of the block that included the burn transaction on Ethereum.
4. tx_hex                     (string, required) Transaction hex.
5. txroot_hex                 (string, required) The transaction merkle root that commits this transaction to the block header.
6. txmerkleproof_hex          (string, required) The list of parent nodes of the Merkle Patricia Tree for SPV proof of transaction merkle root.
7. merklerootpath_hex         (string, required) The merkle path to walk through the tree to recreate the merkle hash for both transaction and receipt root.
8. receipt_hex                (string, required) Transaction Receipt Hex.
9. receiptroot_hex            (string, required) The transaction receipt merkle root that commits this receipt to the block header.
10. receiptmerkleproof_hex    (string, required) The list of parent nodes of the Merkle Patricia Tree for SPV proof of transaction receipt merkle root.
11. witness                   (string, optional, default="") Witness address that will sign for web-of-trust notarization of this transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned transaction hexstring.
}

Examples:
> syscoin-cli syscoinmint "address" "amount" "blocknumber" "tx_hex" "txroot_hex" "txmerkleproof" "txmerkleproofpath" "receipt_hex" "receiptroot_hex" "receiptmerkleproof"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscoinmint", "params": ["address", "amount", "blocknumber", "tx_hex", "txroot_hex", "txmerkleproof", "txmerkleproofpath", "receipt_hex", "receiptroot_hex", "receiptmerkleproof", ""] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>syscointxfund "hexstring" "address" ( output_index )</summary>

```

Funds a new syscoin transaction with inputs used from wallet or an array of addresses specified. Note that any inputs to the transaction added prior to calling this will not be accounted and new outputs will be added every time you call this function.

Arguments:
1. hexstring       (string, required) The raw syscoin transaction output given from rpc
2. address         (string, required) Address belonging to this asset transaction.
3. output_index    (numeric) Output index from available UTXOs in address. Defaults to selecting all that are needed to fund the transaction.

Result:
{
  "hex": "hexstring"       (string) the unsigned funded transaction hexstring.
}

Examples:
> syscoin-cli syscointxfund <hexstring> "sys1qtyf33aa2tl62xhrzhralpytka0krxvt0a4e8ee"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "syscointxfund", "params": [<hexstring>, "sys1qtyf33aa2tl62xhrzhralpytka0krxvt0a4e8ee", 0] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Utilities

<details><summary>createmultisig nrequired ["key",...] ( "address_type" )</summary>

```

Creates a multi-signature address with n signature of m keys required.
It returns a json object with the address and redeemScript.

Arguments:
1. nrequired       (numeric, required) The number of required signatures out of the n keys.
2. keys            (json array, required) A json array of hex-encoded public keys.
     [
       "key",      (string) The hex-encoded public key
       ...
     ]
3. address_type    (string, optional, default=legacy) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32".

Result:
{
  "address":"multisigaddress",  (string) The value of the new multisig address.
  "redeemScript":"script"       (string) The string value of the hex-encoded redemption script.
}

Examples:

Create a multisig address from 2 public keys
> syscoin-cli createmultisig 2 "[\"03789ed0bb717d88f7d321a368d905e7430207ebbd82bd342cf11ae157a7ace5fd\",\"03dbc6764b8884a92e871274b87583e6d5c2a58819473e17e107ef3f6aa5a61626\"]"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createmultisig", "params": [2, "[\"03789ed0bb717d88f7d321a368d905e7430207ebbd82bd342cf11ae157a7ace5fd\",\"03dbc6764b8884a92e871274b87583e6d5c2a58819473e17e107ef3f6aa5a61626\"]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>deriveaddresses "descriptor" ( range )</summary>

```

Derives one or more addresses corresponding to an output descriptor.
Examples of output descriptors are:
    pkh(<pubkey>)                        P2PKH outputs for the given pubkey
    wpkh(<pubkey>)                       Native segwit P2PKH outputs for the given pubkey
    sh(multi(<n>,<pubkey>,<pubkey>,...)) P2SH-multisig outputs for the given threshold and pubkeys
    raw(<hex script>)                    Outputs whose scriptPubKey equals the specified hex scripts

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one
or more path elements separated by "/", where "h" represents a hardened child key.
For more information on output descriptors, see the documentation in the doc/descriptors.md file.

Arguments:
1. descriptor    (string, required) The descriptor.
2. range         (numeric or array, optional) If a ranged descriptor is used, this specifies the end or the range (in [begin,end] notation) to derive.

Result:
[ address ] (array) the derived addresses

Examples:
First three native segwit receive addresses
> syscoin-cli deriveaddresses "wpkh([d34db33f/84h/0h/0h]xpub6DJ2dNUysrn5Vt36jH2KLBT2i1auw1tTSSomg8PhqNiUtx8QX2SvC9nrHu81fT41fvDUnhMjEzQgXnQjKEu3oaqMSzhSrHMxyyoEAmUHQbY/0/*)#cjjspncu" "[0,2]"
```
</details>

<details><summary>estimatesmartfee conf_target ( "estimate_mode" )</summary>

```

Estimates the approximate fee per kilobyte needed for a transaction to begin
confirmation within conf_target blocks if possible and return the number of blocks
for which the estimate is valid. Uses virtual transaction size as defined
in BIP 141 (witness data is discounted).

Arguments:
1. conf_target      (numeric, required) Confirmation target in blocks (1 - 1008)
2. estimate_mode    (string, optional, default=CONSERVATIVE) The fee estimate mode.
                    Whether to return a more conservative estimate which also satisfies
                    a longer history. A conservative estimate potentially returns a
                    higher feerate and is more likely to be sufficient for the desired
                    target, but is not as responsive to short term drops in the
                    prevailing fee market.  Must be one of:
                    "UNSET"
                    "ECONOMICAL"
                    "CONSERVATIVE"

Result:
{
  "feerate" : x.x,     (numeric, optional) estimate fee rate in SYS/kB
  "errors": [ str... ] (json array of strings, optional) Errors encountered during processing
  "blocks" : n         (numeric) block number where estimate was found
}

The request target will be clamped between 2 and the highest target
fee estimation is able to return based on how long it has been running.
An error is returned if not enough transactions and blocks
have been observed to make an estimate for any number of blocks.

Examples:
> syscoin-cli estimatesmartfee 6
```
</details>

<details><summary>getdescriptorinfo "descriptor"</summary>

```

Analyses a descriptor.

Arguments:
1. descriptor    (string, required) The descriptor.

Result:
{
  "descriptor" : "desc",         (string) The descriptor in canonical form, without private keys
  "isrange" : true|false,        (boolean) Whether the descriptor is ranged
  "issolvable" : true|false,     (boolean) Whether the descriptor is solvable
  "hasprivatekeys" : true|false, (boolean) Whether the input descriptor contained at least one private key
}

Examples:
Analyse a descriptor
> syscoin-cli getdescriptorinfo "wpkh([d34db33f/84h/0h/0h]0279be667ef9dcbbac55a06295Ce870b07029Bfcdb2dce28d959f2815b16f81798)"
```
</details>

<details><summary>signmessagewithprivkey "privkey" "message"</summary>

```

Sign a message with the private key of an address

Arguments:
1. privkey    (string, required) The private key to sign the message with.
2. message    (string, required) The message to create a signature of.

Result:
"signature"          (string) The signature of the message encoded in base 64

Examples:

Create the signature
> syscoin-cli signmessagewithprivkey "privkey" "my message"

Verify the signature
> syscoin-cli verifymessage "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "signature" "my message"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "signmessagewithprivkey", "params": ["privkey", "my message"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>validateaddress "address"</summary>

```

Return information about the given syscoin address.

Arguments:
1. address    (string, required) The syscoin address to validate

Result:
{
  "isvalid" : true|false,       (boolean) If the address is valid or not. If not, this is the only property returned.
  "address" : "address",        (string) The syscoin address validated
  "scriptPubKey" : "hex",       (string) The hex-encoded scriptPubKey generated by the address
  "isscript" : true|false,      (boolean) If the key is a script
  "iswitness" : true|false,     (boolean) If the address is a witness address
  "witness_version" : version   (numeric, optional) The version number of the witness program
  "witness_program" : "hex"     (string, optional) The hex value of the witness program
}

Examples:
> syscoin-cli validateaddress "1PSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "validateaddress", "params": ["1PSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>verifymessage "address" "signature" "message"</summary>

```

Verify a signed message

Arguments:
1. address      (string, required) The syscoin address to use for the signature.
2. signature    (string, required) The signature provided by the signer in base 64 encoding (see signmessage).
3. message      (string, required) The message that was signed.

Result:
true|false   (boolean) If the signature is verified or not.

Examples:

Unlock the wallet for 30 seconds
> syscoin-cli walletpassphrase "mypassphrase" 30

Create the signature
> syscoin-cli signmessage "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "my message"

Verify the signature
> syscoin-cli verifymessage "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "signature" "my message"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "verifymessage", "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "signature", "my message"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

## Wallet

<details><summary>abandontransaction "txid"</summary>

```
Mark in-wallet transaction <txid> as abandoned
This will mark this transaction and all its in-wallet descendants as abandoned which will allow
for their inputs to be respent.  It can be used to replace "stuck" or evicted transactions.
It only works on transactions which are not included in a block and are not currently in the mempool.
It has no effect on transactions which are already abandoned.

Arguments:
1. txid    (string, required) The transaction id

Examples:
> syscoin-cli abandontransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "abandontransaction", "params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>abortrescan</summary>

```
Stops current wallet rescan triggered by an RPC call, e.g. by an importprivkey call.
Note: Use "getwalletinfo" to query the scanning progress.

Examples:

Import a private key
> syscoin-cli importprivkey "mykey"

Abort the running wallet rescan
> syscoin-cli abortrescan 

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "abortrescan", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>addmultisigaddress nrequired ["key",...] ( "label" "address_type" )</summary>

```
Add a nrequired-to-sign multisignature address to the wallet. Requires a new wallet backup.
Each key is a Syscoin address or hex-encoded public key.
This functionality is only intended for use with non-watchonly addresses.
See `importaddress` for watchonly p2sh address support.
If 'label' is specified, assign address to that label.

Arguments:
1. nrequired       (numeric, required) The number of required signatures out of the n keys or addresses.
2. keys            (json array, required) A json array of syscoin addresses or hex-encoded public keys
     [
       "key",      (string) syscoin address or hex-encoded public key
       ...
     ]
3. label           (string, optional) A label to assign the addresses to.
4. address_type    (string, optional, default=set by -addresstype) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32".

Result:
{
  "address":"multisigaddress",    (string) The value of the new multisig address.
  "redeemScript":"script"         (string) The string value of the hex-encoded redemption script.
}

Examples:

Add a multisig address from 2 addresses
> syscoin-cli addmultisigaddress 2 "[\"16sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\",\"171sgjn4YtPu27adkKGrdDwzRTxnRkBfKV\"]"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "addmultisigaddress", "params": [2, "[\"16sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\",\"171sgjn4YtPu27adkKGrdDwzRTxnRkBfKV\"]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>backupwallet "destination"</summary>

```

Safely copies current wallet file to destination, which can be a directory or a path with filename.

Arguments:
1. destination    (string, required) The destination directory or file

Examples:
> syscoin-cli backupwallet "backup.dat"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "backupwallet", "params": ["backup.dat"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>bumpfee "txid" ( options )</summary>

```

Bumps the fee of an opt-in-RBF transaction T, replacing it with a new transaction B.
An opt-in RBF transaction with the given txid must be in the wallet.
The command will pay the additional fee by reducing change outputs or adding inputs when necessary. It may add a new change output if one does not already exist.
If `totalFee` is given, adding inputs is not supported, so there must be a single change output that is big enough or it will fail.
All inputs in the original transaction will be included in the replacement transaction.
The command will fail if the wallet or mempool contains a transaction that spends one of T's outputs.
By default, the new fee will be calculated automatically using estimatesmartfee.
The user can specify a confirmation target for estimatesmartfee.
Alternatively, the user can specify totalFee, or use RPC settxfee to set a higher fee rate.
At a minimum, the new fee rate must be high enough to pay an additional new relay fee (incrementalfee
returned by getnetworkinfo) to enter the node's mempool.

Arguments:
1. txid                           (string, required) The txid to be bumped
2. options                        (json object, optional)
     {
       "confTarget": n,           (numeric, optional, default=fallback to wallet's default) Confirmation target (in blocks)
       "totalFee": n,             (numeric, optional, default=fallback to 'confTarget') Total fee (NOT feerate) to pay, in satoshis.
                                  In rare cases, the actual fee paid might be slightly higher than the specified
                                  totalFee if the tx change output has to be removed because it is too close to
                                  the dust threshold.
       "replaceable": bool,       (boolean, optional, default=true) Whether the new transaction should still be
                                  marked bip-125 replaceable. If true, the sequence numbers in the transaction will
                                  be left unchanged from the original. If false, any input sequence numbers in the
                                  original transaction that were less than 0xfffffffe will be increased to 0xfffffffe
                                  so the new transaction will not be explicitly bip-125 replaceable (though it may
                                  still be replaceable in practice, for example if it has unconfirmed ancestors which
                                  are replaceable).
       "estimate_mode": "str",    (string, optional, default=UNSET) The fee estimate mode, must be one of:
                                  "UNSET"
                                  "ECONOMICAL"
                                  "CONSERVATIVE"
     }

Result:
{
  "txid":    "value",   (string)  The id of the new transaction
  "origfee":  n,         (numeric) Fee of the replaced transaction
  "fee":      n,         (numeric) Fee of the new transaction
  "errors":  [ str... ] (json array of strings) Errors encountered during processing (may be empty)
}

Examples:

Bump the fee, get the new transaction's txid
> syscoin-cli bumpfee <txid>
```
</details>

<details><summary>createwallet "wallet_name" ( disable_private_keys blank "passphrase" )</summary>

```

Creates and loads a new wallet.

Arguments:
1. wallet_name             (string, required) The name for the new wallet. If this is a path, the wallet will be created at the path location.
2. disable_private_keys    (boolean, optional, default=false) Disable the possibility of private keys (only watchonlys are possible in this mode).
3. blank                   (boolean, optional, default=false) Create a blank wallet. A blank wallet has no keys or HD seed. One can be set using sethdseed.
4. passphrase              (string) Encrypt the wallet with this passphrase.

Result:
{
  "name" :    <wallet_name>,        (string) The wallet name if created successfully. If the wallet was created using a full path, the wallet_name will be the full path.
  "warning" : <warning>,            (string) Warning message if wallet was not loaded cleanly.
}

Examples:
> syscoin-cli createwallet "testwallet"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createwallet", "params": ["testwallet"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>dumpprivkey "address"</summary>

```

Reveals the private key corresponding to 'address'.
Then the importprivkey can be used with this output

Arguments:
1. address    (string, required) The syscoin address for the private key

Result:
"key"                (string) The private key

Examples:
> syscoin-cli dumpprivkey "myaddress"
> syscoin-cli importprivkey "mykey"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "dumpprivkey", "params": ["myaddress"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>dumpwallet "filename"</summary>

```

Dumps all wallet keys in a human-readable format to a server-side file. This does not allow overwriting existing files.
Imported scripts are included in the dumpfile, but corresponding BIP173 addresses, etc. may not be added automatically by importwallet.
Note that if your wallet contains keys which are not derived from your HD seed (e.g. imported keys), these are not covered by
only backing up the seed itself, and must be backed up too (e.g. ensure you back up the whole dumpfile).

Arguments:
1. filename    (string, required) The filename with path (either absolute or relative to syscoind)

Result:
{                           (json object)
  "filename" : {        (string) The filename with full absolute path
}

Examples:
> syscoin-cli dumpwallet "test"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "dumpwallet", "params": ["test"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>encryptwallet "passphrase"</summary>

```

Encrypts the wallet with 'passphrase'. This is for first time encryption.
After this, any calls that interact with private keys such as sending or signing 
will require the passphrase to be set prior the making these calls.
Use the walletpassphrase call for this, and then walletlock call.
If the wallet is already encrypted, use the walletpassphrasechange call.

Arguments:
1. passphrase    (string, required) The pass phrase to encrypt the wallet with. It must be at least 1 character, but should be long.

Examples:

Encrypt your wallet
> syscoin-cli encryptwallet "my pass phrase"

Now set the passphrase to use the wallet, such as for signing or sending syscoin
> syscoin-cli walletpassphrase "my pass phrase"

Now we can do something like sign
> syscoin-cli signmessage "address" "test message"

Now lock the wallet again by removing the passphrase
> syscoin-cli walletlock 

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "encryptwallet", "params": ["my pass phrase"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getaddressesbylabel "label"</summary>

```

Returns the list of addresses assigned the specified label.

Arguments:
1. label    (string, required) The label.

Result:
{ (json object with addresses as keys)
  "address": { (json object with information about address)
    "purpose": "string" (string)  Purpose of address ("send" for sending address, "receive" for receiving address)
  },...
}

Examples:
> syscoin-cli getaddressesbylabel "tabby"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getaddressesbylabel", "params": ["tabby"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getaddressinfo "address"</summary>

```

Return information about the given syscoin address. Some information requires the address
to be in the wallet.

Arguments:
1. address    (string, required) The syscoin address to get the information of.

Result:
{
  "address" : "address",        (string) The syscoin address validated
  "scriptPubKey" : "hex",       (string) The hex-encoded scriptPubKey generated by the address
  "ismine" : true|false,        (boolean) If the address is yours or not
  "iswatchonly" : true|false,   (boolean) If the address is watchonly
  "solvable" : true|false,      (boolean) Whether we know how to spend coins sent to this address, ignoring the possible lack of private keys
  "desc" : "desc",            (string, optional) A descriptor for spending coins sent to this address (only when solvable)
  "isscript" : true|false,      (boolean) If the key is a script
  "ischange" : true|false,      (boolean) If the address was used for change output
  "iswitness" : true|false,     (boolean) If the address is a witness address
  "witness_version" : version   (numeric, optional) The version number of the witness program
  "witness_program" : "hex"     (string, optional) The hex value of the witness program
  "script" : "type"             (string, optional) The output script type. Only if "isscript" is true and the redeemscript is known. Possible types: nonstandard, pubkey, pubkeyhash, scripthash, multisig, nulldata, witness_v0_keyhash, witness_v0_scripthash, witness_unknown
  "hex" : "hex",                (string, optional) The redeemscript for the p2sh address
  "pubkeys"                     (string, optional) Array of pubkeys associated with the known redeemscript (only if "script" is "multisig")
    [
      "pubkey"
      ,...
    ]
  "sigsrequired" : xxxxx        (numeric, optional) Number of signatures required to spend multisig output (only if "script" is "multisig")
  "pubkey" : "publickeyhex",    (string, optional) The hex value of the raw public key, for single-key addresses (possibly embedded in P2SH or P2WSH)
  "embedded" : {...},           (object, optional) Information about the address embedded in P2SH or P2WSH, if relevant and known. It includes all getaddressinfo output fields for the embedded address, excluding metadata ("timestamp", "hdkeypath", "hdseedid") and relation to the wallet ("ismine", "iswatchonly").
  "iscompressed" : true|false,  (boolean, optional) If the pubkey is compressed
  "label" :  "label"         (string) The label associated with the address, "" is the default label
  "timestamp" : timestamp,      (number, optional) The creation time of the key if available in seconds since epoch (Jan 1 1970 GMT)
  "hdkeypath" : "keypath"       (string, optional) The HD keypath if the key is HD and available
  "hdseedid" : "<hash160>"      (string, optional) The Hash160 of the HD seed
  "hdmasterfingerprint" : "<hash160>" (string, optional) The fingperint of the master key.
  "labels"                      (object) Array of labels associated with the address.
    [
      { (json object of label data)
        "name": "labelname" (string) The label
        "purpose": "string" (string) Purpose of address ("send" for sending address, "receive" for receiving address)
      },...
    ]
}

Examples:
> syscoin-cli getaddressinfo "1PSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getaddressinfo", "params": ["1PSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getbalance ( "dummy" minconf include_watchonly )</summary>

```

Returns the total available balance.
The available balance is what the wallet considers currently spendable, and is
thus affected by options which limit spendability such as -spendzeroconfchange.

Arguments:
1. dummy                (string, optional) Remains for backward compatibility. Must be excluded or set to "*".
2. minconf              (numeric, optional, default=0) Only include transactions confirmed at least this many times.
3. include_watchonly    (boolean, optional, default=false) Also include balance in watch-only addresses (see 'importaddress')

Result:
amount              (numeric) The total amount in SYS received for this wallet.

Examples:

The total amount in the wallet with 1 or more confirmations
> syscoin-cli getbalance 

The total amount in the wallet at least 6 blocks confirmed
> syscoin-cli getbalance "*" 6

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getbalance", "params": ["*", 6] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getbalances</summary>

```
Returns an object with all balances in SYS.

Result:
{
    "mine": {                        (object) balances from outputs that the wallet can sign
      "trusted": xxx                 (numeric) trusted balance (outputs created by the wallet or confirmed outputs)
      "untrusted_pending": xxx       (numeric) untrusted pending balance (outputs created by others that are in the mempool)
      "immature": xxx                (numeric) balance from immature coinbase outputs
    },
    "watchonly": {                   (object) watchonly balances (not present if wallet does not watch anything)
      "trusted": xxx                 (numeric) trusted balance (outputs created by the wallet or confirmed outputs)
      "untrusted_pending": xxx       (numeric) untrusted pending balance (outputs created by others that are in the mempool)
      "immature": xxx                (numeric) balance from immature coinbase outputs
    },
}

Examples:
> syscoin-cli getbalances 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getbalances", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getnewaddress ( "label" "address_type" )</summary>

```

Returns a new Syscoin address for receiving payments.
If 'label' is specified, it is added to the address book 
so payments received with the address will be associated with 'label'.

Arguments:
1. label           (string, optional, default="") The label name for the address to be linked to. It can also be set to the empty string "" to represent the default label. The label does not need to exist, it will be created if there is no label by the given name.
2. address_type    (string, optional, default=set by -addresstype) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32".

Result:
"address"    (string) The new syscoin address

Examples:
> syscoin-cli getnewaddress 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnewaddress", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getrawchangeaddress ( "address_type" )</summary>

```

Returns a new Syscoin address, for receiving change.
This is for use with raw transactions, NOT normal use.

Arguments:
1. address_type    (string, optional, default=set by -changetype) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32".

Result:
"address"    (string) The address

Examples:
> syscoin-cli getrawchangeaddress 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getrawchangeaddress", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getreceivedbyaddress "address" ( minconf )</summary>

```

Returns the total amount received by the given address in transactions with at least minconf confirmations.

Arguments:
1. address    (string, required) The syscoin address for transactions.
2. minconf    (numeric, optional, default=1) Only include transactions confirmed at least this many times.

Result:
amount   (numeric) The total amount in SYS received at this address.

Examples:

The amount from transactions with at least 1 confirmation
> syscoin-cli getreceivedbyaddress "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX"

The amount including unconfirmed transactions, zero confirmations
> syscoin-cli getreceivedbyaddress "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" 0

The amount with at least 6 confirmations
> syscoin-cli getreceivedbyaddress "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" 6

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getreceivedbyaddress", "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", 6] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getreceivedbylabel "label" ( minconf )</summary>

```

Returns the total amount received by addresses with <label> in transactions with at least [minconf] confirmations.

Arguments:
1. label      (string, required) The selected label, may be the default label using "".
2. minconf    (numeric, optional, default=1) Only include transactions confirmed at least this many times.

Result:
amount              (numeric) The total amount in SYS received for this label.

Examples:

Amount received by the default label with at least 1 confirmation
> syscoin-cli getreceivedbylabel ""

Amount received at the tabby label including unconfirmed amounts with zero confirmations
> syscoin-cli getreceivedbylabel "tabby" 0

The amount with at least 6 confirmations
> syscoin-cli getreceivedbylabel "tabby" 6

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getreceivedbylabel", "params": ["tabby", 6] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>gettransaction "txid" ( include_watchonly )</summary>

```

Get detailed information about in-wallet transaction <txid>

Arguments:
1. txid                 (string, required) The transaction id
2. include_watchonly    (boolean, optional, default=false) Whether to include watch-only addresses in balance calculation and details[]

Result:
{
  "amount" : x.xxx,        (numeric) The transaction amount in SYS
  "fee": x.xxx,            (numeric) The amount of the fee in SYS. This is negative and only available for the 
                              'send' category of transactions.
  "confirmations" : n,     (numeric) The number of confirmations
  "blockhash" : "hash",  (string) The block hash
  "blockindex" : xx,       (numeric) The index of the transaction in the block that includes it
  "blocktime" : ttt,       (numeric) The time in seconds since epoch (1 Jan 1970 GMT)
  "txid" : "transactionid",   (string) The transaction id.
  "time" : ttt,            (numeric) The transaction time in seconds since epoch (1 Jan 1970 GMT)
  "timereceived" : ttt,    (numeric) The time received in seconds since epoch (1 Jan 1970 GMT)
  "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                   may be unknown for unconfirmed transactions not in the mempool
  "details" : [
    {
      "address" : "address",          (string) The syscoin address involved in the transaction
      "category" :                      (string) The transaction category.
                   "send"                  Transactions sent.
                   "receive"               Non-coinbase transactions received.
                   "generate"              Coinbase transactions received with more than 100 confirmations.
                   "immature"              Coinbase transactions received with 100 or fewer confirmations.
                   "orphan"                Orphaned coinbase transactions received.
      "amount" : x.xxx,                 (numeric) The amount in SYS
      "label" : "label",              (string) A comment for the address/transaction, if any
      "vout" : n,                       (numeric) the vout value
      "fee": x.xxx,                     (numeric) The amount of the fee in SYS. This is negative and only available for the 
                                           'send' category of transactions.
      "abandoned": xxx                  (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 
                                           'send' category of transactions.
    }
    ,...
  ],
  "hex" : "data"         (string) Raw data for transaction
}

Examples:
> syscoin-cli gettransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"
> syscoin-cli gettransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d" true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gettransaction", "params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>getunconfirmedbalance</summary>

```
DEPRECATED
Identical to getbalances().mine.untrusted_pending
```
</details>

<details><summary>getwalletinfo</summary>

```
Returns an object containing various wallet state info.

Result:
{
  "walletname": xxxxx,               (string) the wallet name
  "walletversion": xxxxx,            (numeric) the wallet version
  "balance": xxxxxxx,                (numeric) DEPRECATED. Identical to getbalances().mine.trusted
  "unconfirmed_balance": xxx,        (numeric) DEPRECATED. Identical to getbalances().mine.untrusted_pending
  "immature_balance": xxxxxx,        (numeric) DEPRECATED. Identical to getbalances().mine.immature
  "txcount": xxxxxxx,                (numeric) the total number of transactions in the wallet
  "keypoololdest": xxxxxx,           (numeric) the timestamp (seconds since Unix epoch) of the oldest pre-generated key in the key pool
  "keypoolsize": xxxx,               (numeric) how many new keys are pre-generated (only counts external keys)
  "keypoolsize_hd_internal": xxxx,   (numeric) how many new keys are pre-generated for internal use (used for change outputs, only appears if the wallet is using this feature, otherwise external keys are used)
  "unlocked_until": ttt,             (numeric) the timestamp in seconds since epoch (midnight Jan 1 1970 GMT) that the wallet is unlocked for transfers, or 0 if the wallet is locked
  "paytxfee": x.xxxx,                (numeric) the transaction fee configuration, set in SYS/kB
  "hdseedid": "<hash160>"            (string, optional) the Hash160 of the HD seed (only present when HD is enabled)
  "private_keys_enabled": true|false (boolean) false if privatekeys are disabled for this wallet (enforced watch-only wallet)
  "scanning":                        (json object) current scanning details, or false if no scan is in progress
    {
      "duration" : xxxx              (numeric) elapsed seconds since scan start
      "progress" : x.xxxx,           (numeric) scanning progress percentage [0.0, 1.0]
    }
}

Examples:
> syscoin-cli getwalletinfo 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getwalletinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>importaddress "address" ( "label" rescan p2sh )</summary>

```

Adds an address or script (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported address exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.
If you have the full public key, you should call importpubkey instead of this.
Hint: use importmulti to import more than one address.

Note: If you import a non-standard raw script in hex form, outputs sending to it will be treated
as change, and not show up in many RPCs.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. address    (string, required) The Syscoin address (or hex-encoded script)
2. label      (string, optional, default="") An optional label
3. rescan     (boolean, optional, default=true) Rescan the wallet for transactions
4. p2sh       (boolean, optional, default=false) Add the P2SH version of the script as well

Examples:

Import an address with rescan
> syscoin-cli importaddress "myaddress"

Import using a label without rescan
> syscoin-cli importaddress "myaddress" "testing" false

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importaddress", "params": ["myaddress", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>importmulti "requests" ( "options" )</summary>

```

Import addresses/scripts (with private or public keys, redeem script (P2SH)), optionally rescanning the blockchain from the earliest creation time of the imported scripts. Requires a new wallet backup.
If an address/script is imported without all of the private keys required to spend from that address, it will be watchonly. The 'watchonly' option must be set to true in this case or a warning will be returned.
Conversely, if all the private keys are provided and the address/script is spendable, the watchonly option must be set to false, or a warning will be returned.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported keys, addresses or scripts exist but related transactions are still missing.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. requests                                                         (json array, required) Data to be imported
     [
       {                                                            (json object)
         "desc": "str",                                             (string) Descriptor to import. If using descriptor, do not also provide address/scriptPubKey, scripts, or pubkeys
         "scriptPubKey": "<script>" | { "address":"<address>" },    (string / json, required) Type of scriptPubKey (string for script, json for address). Should not be provided if using a descriptor
         "timestamp": timestamp | "now",                            (integer / string, required) Creation time of the key in seconds since epoch (Jan 1 1970 GMT),
                                                                    or the string "now" to substitute the current synced blockchain time. The timestamp of the oldest
                                                                    key will determine how far back blockchain rescans need to begin for missing wallet transactions.
                                                                    "now" can be specified to bypass scanning, for keys which are known to never have been used, and
                                                                    0 can be specified to scan the entire blockchain. Blocks up to 2 hours before the earliest key
                                                                    creation time of all keys being imported by the importmulti call will be scanned.
         "redeemscript": "str",                                     (string) Allowed only if the scriptPubKey is a P2SH or P2SH-P2WSH address/scriptPubKey
         "witnessscript": "str",                                    (string) Allowed only if the scriptPubKey is a P2SH-P2WSH or P2WSH address/scriptPubKey
         "pubkeys": [                                               (json array, optional, default=empty array) Array of strings giving pubkeys to import. They must occur in P2PKH or P2WPKH scripts. They are not required when the private key is also provided (see the "keys" argument).
           "pubKey",                                                (string)
           ...
         ],
         "keys": [                                                  (json array, optional, default=empty array) Array of strings giving private keys to import. The corresponding public keys must occur in the output or redeemscript.
           "key",                                                   (string)
           ...
         ],
         "range": n or [n,n],                                       (numeric or array) If a ranged descriptor is used, this specifies the end or the range (in the form [begin,end]) to import
         "internal": bool,                                          (boolean, optional, default=false) Stating whether matching outputs should be treated as not incoming payments (also known as change)
         "watchonly": bool,                                         (boolean, optional, default=false) Stating whether matching outputs should be considered watchonly.
         "label": "str",                                            (string, optional, default='') Label to assign to the address, only allowed with internal=false
         "keypool": bool,                                           (boolean, optional, default=false) Stating whether imported public keys should be added to the keypool for when users request new addresses. Only allowed when wallet private keys are disabled
       },
       ...
     ]
2. options                                                          (json object, optional)
     {
       "rescan": bool,                                              (boolean, optional, default=true) Stating if should rescan the blockchain after all imports
     }

Result:

Response is an array with the same size as the input that has the execution result :
  [{"success": true}, {"success": true, "warnings": ["Ignoring irrelevant private key"]}, {"success": false, "error": {"code": -1, "message": "Internal Server Error"}}, ...]

Examples:
> syscoin-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }, { "scriptPubKey": { "address": "<my 2nd address>" }, "label": "example 2", "timestamp": 1455191480 }]'
> syscoin-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }]' '{ "rescan": false}'
```
</details>

<details><summary>importprivkey "privkey" ( "label" rescan )</summary>

```

Adds a private key (as returned by dumpprivkey) to your wallet. Requires a new wallet backup.
Hint: use importmulti to import more than one private key.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported key exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. privkey    (string, required) The private key (see dumpprivkey)
2. label      (string, optional, default=current label if address exists, otherwise "") An optional label
3. rescan     (boolean, optional, default=true) Rescan the wallet for transactions

Examples:

Dump a private key
> syscoin-cli dumpprivkey "myaddress"

Import the private key with rescan
> syscoin-cli importprivkey "mykey"

Import using a label and without rescan
> syscoin-cli importprivkey "mykey" "testing" false

Import using default blank label and without rescan
> syscoin-cli importprivkey "mykey" "" false

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importprivkey", "params": ["mykey", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>importprunedfunds "rawtransaction" "txoutproof"</summary>

```

Imports funds without rescan. Corresponding address or script must previously be included in wallet. Aimed towards pruned wallets. The end-user is responsible to import additional transactions that subsequently spend the imported outputs or rescan after the point in the blockchain the transaction is included.

Arguments:
1. rawtransaction    (string, required) A raw transaction in hex funding an already-existing address in wallet
2. txoutproof        (string, required) The hex output from gettxoutproof that contains the transaction
```
</details>

<details><summary>importpubkey "pubkey" ( "label" rescan )</summary>

```

Adds a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.
Hint: use importmulti to import more than one public key.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported pubkey exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. pubkey    (string, required) The hex-encoded public key
2. label     (string, optional, default="") An optional label
3. rescan    (boolean, optional, default=true) Rescan the wallet for transactions

Examples:

Import a public key with rescan
> syscoin-cli importpubkey "mypubkey"

Import using a label without rescan
> syscoin-cli importpubkey "mypubkey" "testing" false

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importpubkey", "params": ["mypubkey", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>importwallet "filename"</summary>

```

Imports keys from a wallet dump file (see dumpwallet). Requires a new wallet backup to include imported keys.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. filename    (string, required) The wallet file

Examples:

Dump the wallet
> syscoin-cli dumpwallet "test"

Import the wallet
> syscoin-cli importwallet "test"

Import using the json rpc call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importwallet", "params": ["test"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>keypoolrefill ( newsize )</summary>

```

Fills the keypool.

Arguments:
1. newsize    (numeric, optional, default=100) The new keypool size

Examples:
> syscoin-cli keypoolrefill 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "keypoolrefill", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listaddressgroupings</summary>

```

Lists groups of addresses which have had their common ownership
made public by common use as inputs or as the resulting change
in past transactions

Result:
[
  [
    [
      "address",            (string) The syscoin address
      amount,                 (numeric) The amount in SYS
      "label"               (string, optional) The label
    ]
    ,...
  ]
  ,...
]

Examples:
> syscoin-cli listaddressgroupings 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listaddressgroupings", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listlabels ( "purpose" )</summary>

```

Returns the list of all labels, or labels that are assigned to addresses with a specific purpose.

Arguments:
1. purpose    (string, optional) Address purpose to list labels for ('send','receive'). An empty string is the same as not providing this argument.

Result:
[               (json array of string)
  "label",      (string) Label name
  ...
]

Examples:

List all labels
> syscoin-cli listlabels 

List labels that have receiving addresses
> syscoin-cli listlabels receive

List labels that have sending addresses
> syscoin-cli listlabels send

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listlabels", "params": [receive] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listlockunspent</summary>

```

Returns list of temporarily unspendable outputs.
See the lockunspent call to lock and unlock transactions for spending.

Result:
[
  {
    "txid" : "transactionid",     (string) The transaction id locked
    "vout" : n                      (numeric) The vout value
  }
  ,...
]

Examples:

List the unspent transactions
> syscoin-cli listunspent 

Lock an unspent transaction
> syscoin-cli lockunspent false "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

List the locked transactions
> syscoin-cli listlockunspent 

Unlock the transaction again
> syscoin-cli lockunspent true "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listlockunspent", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )</summary>

```

List balances by receiving address.

Arguments:
1. minconf              (numeric, optional, default=1) The minimum number of confirmations before payments are included.
2. include_empty        (boolean, optional, default=false) Whether to include addresses that haven't received any payments.
3. include_watchonly    (boolean, optional, default=false) Whether to include watch-only addresses (see 'importaddress').
4. address_filter       (string, optional) If present, only return information on this address.

Result:
[
  {
    "involvesWatchonly" : true,        (bool) Only returned if imported addresses were involved in transaction
    "address" : "receivingaddress",  (string) The receiving address
    "amount" : x.xxx,                  (numeric) The total amount in SYS received by the address
    "confirmations" : n,               (numeric) The number of confirmations of the most recent transaction included
    "label" : "label",               (string) The label of the receiving address. The default label is "".
    "txids": [
       "txid",                         (string) The ids of transactions received with the address 
       ...
    ]
  }
  ,...
]

Examples:
> syscoin-cli listreceivedbyaddress 
> syscoin-cli listreceivedbyaddress 6 true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbyaddress", "params": [6, true, true] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbyaddress", "params": [6, true, true, "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listreceivedbylabel ( minconf include_empty include_watchonly )</summary>

```

List received transactions by label.

Arguments:
1. minconf              (numeric, optional, default=1) The minimum number of confirmations before payments are included.
2. include_empty        (boolean, optional, default=false) Whether to include labels that haven't received any payments.
3. include_watchonly    (boolean, optional, default=false) Whether to include watch-only addresses (see 'importaddress').

Result:
[
  {
    "involvesWatchonly" : true,   (bool) Only returned if imported addresses were involved in transaction
    "amount" : x.xxx,             (numeric) The total amount received by addresses with this label
    "confirmations" : n,          (numeric) The number of confirmations of the most recent transaction included
    "label" : "label"           (string) The label of the receiving address. The default label is "".
  }
  ,...
]

Examples:
> syscoin-cli listreceivedbylabel 
> syscoin-cli listreceivedbylabel 6 true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbylabel", "params": [6, true, true] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )</summary>

```

Get all transactions in blocks since block [blockhash], or all transactions if omitted.
If "blockhash" is no longer a part of the main chain, transactions from the fork point onward are included.
Additionally, if include_removed is set, transactions affecting the wallet which were removed are returned in the "removed" array.

Arguments:
1. blockhash               (string, optional) If set, the block hash to list transactions since, otherwise list all transactions.
2. target_confirmations    (numeric, optional, default=1) Return the nth block hash from the main chain. e.g. 1 would mean the best block hash. Note: this is not used as a filter, but only affects [lastblock] in the return value
3. include_watchonly       (boolean, optional, default=false) Include transactions to watch-only addresses (see 'importaddress')
4. include_removed         (boolean, optional, default=true) Show transactions that were removed due to a reorg in the "removed" array
                           (not guaranteed to work on pruned nodes)

Result:
{
  "transactions": [
    "address":"address",    (string) The syscoin address of the transaction.
    "category":               (string) The transaction category.
                "send"                  Transactions sent.
                "receive"               Non-coinbase transactions received.
                "generate"              Coinbase transactions received with more than 100 confirmations.
                "immature"              Coinbase transactions received with 100 or fewer confirmations.
                "orphan"                Orphaned coinbase transactions received.
    "amount": x.xxx,          (numeric) The amount in SYS. This is negative for the 'send' category, and is positive
                                         for all other categories
    "vout" : n,               (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in SYS. This is negative and only available for the 'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction.
                                          When it's < 0, it means the transaction conflicted that many blocks ago.
    "blockhash": "hashvalue",     (string) The block hash containing the transaction.
    "blockindex": n,          (numeric) The index of the transaction in the block that includes it.
    "blocktime": xxx,         (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid",  (string) The transaction id.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (Jan 1 1970 GMT).
    "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                   may be unknown for unconfirmed transactions not in the mempool
    "abandoned": xxx,         (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
    "comment": "...",       (string) If a comment is associated with the transaction.
    "label" : "label"       (string) A comment for the address/transaction, if any
    "to": "...",            (string) If a comment to is associated with the transaction.
  ],
  "removed": [
    <structure is the same as "transactions" above, only present if include_removed=true>
    Note: transactions that were re-added in the active chain will appear as-is in this array, and may thus have a positive confirmation count.
  ],
  "lastblock": "lastblockhash"     (string) The hash of the block (target_confirmations-1) from the best block on the main chain. This is typically used to feed back into listsinceblock the next time you call it. So you would generally use a target_confirmations of say 6, so you will be continually re-notified of transactions until they've reached 6 confirmations plus any new ones
}

Examples:
> syscoin-cli listsinceblock 
> syscoin-cli listsinceblock "000000000000000bacf66f7497b7dc45ef753ee9a7d38571037cdb1a57f663ad" 6
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listsinceblock", "params": ["000000000000000bacf66f7497b7dc45ef753ee9a7d38571037cdb1a57f663ad", 6] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listtransactions ( "label" count skip include_watchonly )</summary>

```

If a label name is provided, this will return only incoming transactions paying to addresses with the specified label.

Returns up to 'count' most recent transactions skipping the first 'from' transactions.

Arguments:
1. label                (string, optional) If set, should be a valid label name to return only incoming transactions
                        with the specified label, or "*" to disable filtering and return all transactions.
2. count                (numeric, optional, default=10) The number of transactions to return
3. skip                 (numeric, optional, default=0) The number of transactions to skip
4. include_watchonly    (boolean, optional, default=false) Include transactions to watch-only addresses (see 'importaddress')

Result:
[
  {
    "address":"address",    (string) The syscoin address of the transaction.
    "category":               (string) The transaction category.
                "send"                  Transactions sent.
                "receive"               Non-coinbase transactions received.
                "generate"              Coinbase transactions received with more than 100 confirmations.
                "immature"              Coinbase transactions received with 100 or fewer confirmations.
                "orphan"                Orphaned coinbase transactions received.
    "amount": x.xxx,          (numeric) The amount in SYS. This is negative for the 'send' category, and is positive
                                        for all other categories
    "label": "label",       (string) A comment for the address/transaction, if any
    "vout": n,                (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in SYS. This is negative and only available for the 
                                         'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction. Negative confirmations indicate the
                                         transaction conflicts with the block chain
    "trusted": xxx,           (bool) Whether we consider the outputs of this unconfirmed transaction safe to spend.
    "blockhash": "hashvalue", (string) The block hash containing the transaction.
    "blockindex": n,          (numeric) The index of the transaction in the block that includes it.
    "blocktime": xxx,         (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid", (string) The transaction id.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (midnight Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (midnight Jan 1 1970 GMT).
    "comment": "...",       (string) If a comment is associated with the transaction.
    "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                     may be unknown for unconfirmed transactions not in the mempool
    "abandoned": xxx          (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 
                                         'send' category of transactions.
  }
]

Examples:

List the most recent 10 transactions in the systems
> syscoin-cli listtransactions 

List transactions 100 to 120
> syscoin-cli listtransactions "*" 20 100

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listtransactions", "params": ["*", 20, 100] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listunspent ( minconf maxconf ["address",...] include_unsafe query_options )</summary>

```

Returns array of unspent transaction outputs
with between minconf and maxconf (inclusive) confirmations.
Optionally filter to only include txouts paid to specified addresses.

Arguments:
1. minconf                            (numeric, optional, default=1) The minimum confirmations to filter
2. maxconf                            (numeric, optional, default=9999999) The maximum confirmations to filter
3. addresses                          (json array, optional, default=empty array) A json array of syscoin addresses to filter
     [
       "address",                     (string) syscoin address
       ...
     ]
4. include_unsafe                     (boolean, optional, default=true) Include outputs that are not safe to spend
                                      See description of "safe" attribute below.
5. query_options                      (json object, optional) JSON with query options
     {
       "minimumAmount": amount,       (numeric or string, optional, default=0) Minimum value of each UTXO in SYS
       "maximumAmount": amount,       (numeric or string, optional, default=unlimited) Maximum value of each UTXO in SYS
       "maximumCount": n,             (numeric, optional, default=unlimited) Maximum number of UTXOs
       "minimumSumAmount": amount,    (numeric or string, optional, default=unlimited) Minimum sum value of all UTXOs in SYS
     }

Result:
[                   (array of json object)
  {
    "txid" : "txid",          (string) the transaction id 
    "vout" : n,               (numeric) the vout value
    "address" : "address",    (string) the syscoin address
    "label" : "label",        (string) The associated label, or "" for the default label
    "scriptPubKey" : "key",   (string) the script key
    "amount" : x.xxx,         (numeric) the transaction output amount in SYS
    "confirmations" : n,      (numeric) The number of confirmations
    "redeemScript" : "script" (string) The redeemScript if scriptPubKey is P2SH
    "witnessScript" : "script" (string) witnessScript if the scriptPubKey is P2WSH or P2SH-P2WSH
    "spendable" : xxx,        (bool) Whether we have the private keys to spend this output
    "solvable" : xxx,         (bool) Whether we know how to spend this output, ignoring the lack of keys
    "desc" : xxx,             (string, only when solvable) A descriptor for spending this output
    "safe" : xxx              (bool) Whether this output is considered safe to spend. Unconfirmed transactions
                              from outside keys and unconfirmed replacement transactions are considered unsafe
                              and are not eligible for spending by fundrawtransaction and sendtoaddress.
  }
  ,...
]

Examples:
> syscoin-cli listunspent 
> syscoin-cli listunspent 6 9999999 "[\"1PGFqEzfmQch1gKD3ra4k18PNj3tTUUSqg\",\"1LtvqCaApEdUGFkpKMM4MstjcaL4dKg8SP\"]"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listunspent", "params": [6, 9999999 "[\"1PGFqEzfmQch1gKD3ra4k18PNj3tTUUSqg\",\"1LtvqCaApEdUGFkpKMM4MstjcaL4dKg8SP\"]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
> syscoin-cli listunspent 6 9999999 '[]' true '{ "minimumAmount": 0.005 }'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listunspent", "params": [6, 9999999, [] , true, { "minimumAmount": 0.005 } ] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listwalletdir</summary>

```
Returns a list of wallets in the wallet directory.

Result:
{
  "wallets" : [                (json array of objects)
    {
      "name" : "name"          (string) The wallet name
    }
    ,...
  ]
}

Examples:
> syscoin-cli listwalletdir 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listwalletdir", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>listwallets</summary>

```
Returns a list of currently loaded wallets.
For full information on the wallet, use "getwalletinfo"

Result:
[                         (json array of strings)
  "walletname"            (string) the wallet name
   ...
]

Examples:
> syscoin-cli listwallets 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listwallets", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>loadwallet "filename"</summary>

```

Loads a wallet from a wallet file or directory.
Note that all wallet command-line options used when starting syscoind will be
applied to the new wallet (eg -zapwallettxes, upgradewallet, rescan, etc).

Arguments:
1. filename    (string, required) The wallet directory or .dat file.

Result:
{
  "name" :    <wallet_name>,        (string) The wallet name if loaded successfully.
  "warning" : <warning>,            (string) Warning message if wallet was not loaded cleanly.
}

Examples:
> syscoin-cli loadwallet "test.dat"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "loadwallet", "params": ["test.dat"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>lockunspent unlock ( [{"txid":"hex","vout":n},...] )</summary>

```

Updates list of temporarily unspendable outputs.
Temporarily lock (unlock=false) or unlock (unlock=true) specified transaction outputs.
If no transaction outputs are specified when unlocking then all current locked transaction outputs are unlocked.
A locked transaction output will not be chosen by automatic coin selection, when spending syscoins.
Locks are stored in memory only. Nodes start with zero locked outputs, and the locked output list
is always cleared (by virtue of process exit) when a node stops or fails.
Also see the listunspent call

Arguments:
1. unlock                  (boolean, required) Whether to unlock (true) or lock (false) the specified transactions
2. transactions            (json array, optional, default=empty array) A json array of objects. Each object the txid (string) vout (numeric).
     [
       {                   (json object)
         "txid": "hex",    (string, required) The transaction id
         "vout": n,        (numeric, required) The output number
       },
       ...
     ]

Result:
true|false    (boolean) Whether the command was successful or not

Examples:

List the unspent transactions
> syscoin-cli listunspent 

Lock an unspent transaction
> syscoin-cli lockunspent false "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

List the locked transactions
> syscoin-cli listlockunspent 

Unlock the transaction again
> syscoin-cli lockunspent true "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "lockunspent", "params": [false, "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>removeprunedfunds "txid"</summary>

```

Deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will affect wallet balances.

Arguments:
1. txid    (string, required) The hex-encoded id of the transaction you are deleting

Examples:
> syscoin-cli removeprunedfunds "a8d0c0184dde994a09ec054286f1ce581bebf46446a512166eae7628734ea0a5"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "removeprunedfunds", "params": ["a8d0c0184dde994a09ec054286f1ce581bebf46446a512166eae7628734ea0a5"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>rescanblockchain ( start_height stop_height )</summary>

```

Rescan the local blockchain for wallet related transactions.
Note: Use "getwalletinfo" to query the scanning progress.

Arguments:
1. start_height    (numeric, optional, default=0) block height where the rescan should start
2. stop_height     (numeric, optional) the last block height that should be scanned. If none is provided it will rescan up to the tip at return time of this call.

Result:
{
  "start_height"     (numeric) The block height where the rescan started (the requested height or 0)
  "stop_height"      (numeric) The height of the last rescanned block. May be null in rare cases if there was a reorg and the call didn't scan any blocks because they were already scanned in the background.
}

Examples:
> syscoin-cli rescanblockchain 100000 120000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "rescanblockchain", "params": [100000, 120000] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" )</summary>

```

Send multiple times. Amounts are double-precision floating point numbers.

Arguments:
1. dummy                     (string, required) Must be set to "" for backwards compatibility.
2. amounts                   (json object, required) A json object with addresses and amounts
     {
       "address": amount,    (numeric or string, required) The syscoin address is the key, the numeric amount (can be string) in SYS is the value
     }
3. minconf                   (numeric, optional) Ignored dummy value
4. comment                   (string, optional) A comment
5. subtractfeefrom           (json array, optional) A json array with addresses.
                             The fee will be equally deducted from the amount of each selected address.
                             Those recipients will receive less syscoins than you enter in their corresponding amount field.
                             If no addresses are specified here, the sender pays the fee.
     [
       "address",            (string) Subtract fee from this address
       ...
     ]
6. replaceable               (boolean, optional, default=fallback to wallet's default) Allow this transaction to be replaced by a transaction with higher fees via BIP 125
7. conf_target               (numeric, optional, default=fallback to wallet's default) Confirmation target (in blocks)
8. estimate_mode             (string, optional, default=UNSET) The fee estimate mode, must be one of:
                             "UNSET"
                             "ECONOMICAL"
                             "CONSERVATIVE"

Result:
"txid"                   (string) The transaction id for the send. Only 1 transaction is created regardless of 
                                    the number of addresses.

Examples:

Send two amounts to two different addresses:
> syscoin-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}"

Send two amounts to two different addresses setting the confirmation and comment:
> syscoin-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 6 "testing"

Send two amounts to two different addresses, subtract fee from amount:
> syscoin-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 1 "" "[\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\",\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\"]"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendmany", "params": ["", {"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX":0.01,"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz":0.02}, 6, "testing"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" )</summary>

```

Send an amount to a given address.

Arguments:
1. address                  (string, required) The syscoin address to send to.
2. amount                   (numeric or string, required) The amount in SYS to send. eg 0.1
3. comment                  (string, optional) A comment used to store what the transaction is for.
                            This is not part of the transaction, just kept in your wallet.
4. comment_to               (string, optional) A comment to store the name of the person or organization
                            to which you're sending the transaction. This is not part of the 
                            transaction, just kept in your wallet.
5. subtractfeefromamount    (boolean, optional, default=false) The fee will be deducted from the amount being sent.
                            The recipient will receive less syscoins than you enter in the amount field.
6. replaceable              (boolean, optional, default=fallback to wallet's default) Allow this transaction to be replaced by a transaction with higher fees via BIP 125
7. conf_target              (numeric, optional, default=fallback to wallet's default) Confirmation target (in blocks)
8. estimate_mode            (string, optional, default=UNSET) The fee estimate mode, must be one of:
                            "UNSET"
                            "ECONOMICAL"
                            "CONSERVATIVE"

Result:
"txid"                  (string) The transaction id.

Examples:
> syscoin-cli sendtoaddress "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1
> syscoin-cli sendtoaddress "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1 "donation" "seans outpost"
> syscoin-cli sendtoaddress "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1 "" "" true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendtoaddress", "params": ["1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd", 0.1, "donation", "seans outpost"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>sethdseed ( newkeypool "seed" )</summary>

```

Set or generate a new HD wallet seed. Non-HD wallets will not be upgraded to being a HD wallet. Wallets that are already
HD will have a new HD seed set so that new keys added to the keypool will be derived from this new seed.

Note that you will need to MAKE A NEW BACKUP of your wallet after setting the HD wallet seed.

Arguments:
1. newkeypool    (boolean, optional, default=true) Whether to flush old unused addresses, including change addresses, from the keypool and regenerate it.
                 If true, the next address from getnewaddress and change address from getrawchangeaddress will be from this new seed.
                 If false, addresses (including change addresses if the wallet already had HD Chain Split enabled) from the existing
                 keypool will be used until it has been depleted.
2. seed          (string, optional, default=random seed) The WIF private key to use as the new HD seed.
                 The seed value can be retrieved using the dumpwallet command. It is the private key marked hdseed=1

Examples:
> syscoin-cli sethdseed 
> syscoin-cli sethdseed false
> syscoin-cli sethdseed true "wifkey"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sethdseed", "params": [true, "wifkey"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>setlabel "address" "label"</summary>

```

Sets the label associated with the given address.

Arguments:
1. address    (string, required) The syscoin address to be associated with a label.
2. label      (string, required) The label to assign to the address.

Examples:
> syscoin-cli setlabel "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "tabby"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "setlabel", "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "tabby"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>settxfee amount</summary>

```

Set the transaction fee per kB for this wallet. Overrides the global -paytxfee command line parameter.

Arguments:
1. amount    (numeric or string, required) The transaction fee in SYS/kB

Result:
true|false        (boolean) Returns true if successful

Examples:
> syscoin-cli settxfee 0.00001
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "settxfee", "params": [0.00001] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>signmessage "address" "message"</summary>

```

Sign a message with the private key of an address

Arguments:
1. address    (string, required) The syscoin address to use for the private key.
2. message    (string, required) The message to create a signature of.

Result:
"signature"          (string) The signature of the message encoded in base 64

Examples:

Unlock the wallet for 30 seconds
> syscoin-cli walletpassphrase "mypassphrase" 30

Create the signature
> syscoin-cli signmessage "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "my message"

Verify the signature
> syscoin-cli verifymessage "1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "signature" "my message"

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "signmessage", "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "my message"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )</summary>

```

Sign inputs for raw transaction (serialized, hex-encoded).
The second optional argument (may be null) is an array of previous transaction outputs that
this transaction depends on but may not yet be in the block chain.

Arguments:
1. hexstring                        (string, required) The transaction hex string
2. prevtxs                          (json array, optional) A json array of previous dependent transaction outputs
     [
       {                            (json object)
         "txid": "hex",             (string, required) The transaction id
         "vout": n,                 (numeric, required) The output number
         "scriptPubKey": "hex",     (string, required) script key
         "redeemScript": "hex",     (string) (required for P2SH) redeem script
         "witnessScript": "hex",    (string) (required for P2WSH or P2SH-P2WSH) witness script
         "amount": amount,          (numeric or string) (required for Segwit inputs) the amount spent
       },
       ...
     ]
3. sighashtype                      (string, optional, default=ALL) The signature hash type. Must be one of
                                    "ALL"
                                    "NONE"
                                    "SINGLE"
                                    "ALL|ANYONECANPAY"
                                    "NONE|ANYONECANPAY"
                                    "SINGLE|ANYONECANPAY"

Result:
{
  "hex" : "value",                  (string) The hex-encoded raw transaction with signature(s)
  "complete" : true|false,          (boolean) If the transaction has a complete set of signatures
  "errors" : [                      (json array of objects) Script verification errors (if there are any)
    {
      "txid" : "hash",              (string) The hash of the referenced, previous transaction
      "vout" : n,                   (numeric) The index of the output to spent and used as input
      "scriptSig" : "hex",          (string) The hex-encoded signature script
      "sequence" : n,               (numeric) Script sequence number
      "error" : "text"              (string) Verification or signing error related to the input
    }
    ,...
  ]
}

Examples:
> syscoin-cli signrawtransactionwithwallet "myhex"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "signrawtransactionwithwallet", "params": ["myhex"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>unloadwallet ( "wallet_name" )</summary>

```
Unloads the wallet referenced by the request endpoint otherwise unloads the wallet specified in the argument.
Specifying the wallet name on a wallet endpoint is invalid.
Arguments:
1. wallet_name    (string, optional, default=the wallet name from the RPC request) The name of the wallet to unload.

Examples:
> syscoin-cli unloadwallet wallet_name
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "unloadwallet", "params": [wallet_name] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>walletcreatefundedpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime options bip32derivs )</summary>

```

Creates and funds a transaction in the Partially Signed Transaction format. Inputs will be added if supplied inputs are not enough
Implements the Creator and Updater roles.

Arguments:
1. inputs                             (json array, required) A json array of json objects
     [
       {                              (json object)
         "txid": "hex",               (string, required) The transaction id
         "vout": n,                   (numeric, required) The output number
         "sequence": n,               (numeric, required) The sequence number
       },
       ...
     ]
2. outputs                            (json array, required) a json array with outputs (key-value pairs), where none of the keys are duplicated.
                                      That is, each address can only appear once and there can only be one 'data' object.
                                      For compatibility reasons, a dictionary, which holds the key-value pairs directly, is also
                                      accepted as second parameter.
     [
       {                              (json object)
         "address": amount,           (numeric or string, required) A key-value pair. The key (string) is the syscoin address, the value (float or string) is the amount in SYS
       },
       {                              (json object)
         "data": "hex",               (string, required) A key-value pair. The key must be "data", the value is hex-encoded data
       },
       ...
     ]
3. locktime                           (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
4. options                            (json object, optional)
     {
       "changeAddress": "hex",        (string, optional, default=pool address) The syscoin address to receive the change
       "changePosition": n,           (numeric, optional, default=random) The index of the change output
       "change_type": "str",          (string, optional, default=set by -changetype) The output type to use. Only valid if changeAddress is not specified. Options are "legacy", "p2sh-segwit", and "bech32".
       "includeWatching": bool,       (boolean, optional, default=false) Also select inputs which are watch only
       "lockUnspents": bool,          (boolean, optional, default=false) Lock selected unspent outputs
       "feeRate": amount,             (numeric or string, optional, default=not set: makes wallet determine the fee) Set a specific fee rate in SYS/kB
       "subtractFeeFromOutputs": [    (json array, optional, default=empty array) A json array of integers.
                                      The fee will be equally deducted from the amount of each specified output.
                                      Those recipients will receive less syscoins than you enter in their corresponding amount field.
                                      If no outputs are specified here, the sender pays the fee.
         vout_index,                  (numeric) The zero-based output index, before a change output is added.
         ...
       ],
       "replaceable": bool,           (boolean, optional, default=false) Marks this transaction as BIP125 replaceable.
                                      Allows this transaction to be replaced by a transaction with higher fees
       "conf_target": n,              (numeric, optional, default=Fallback to wallet's confirmation target) Confirmation target (in blocks)
       "estimate_mode": "str",        (string, optional, default=UNSET) The fee estimate mode, must be one of:
                                      "UNSET"
                                      "ECONOMICAL"
                                      "CONSERVATIVE"
     }
5. bip32derivs                        (boolean, optional, default=false) If true, includes the BIP 32 derivation paths for public keys if we know them

Result:
{
  "psbt": "value",        (string)  The resulting raw transaction (base64-encoded string)
  "fee":       n,         (numeric) Fee in SYS the resulting transaction pays
  "changepos": n          (numeric) The position of the added change output, or -1
}

Examples:

Create a transaction with no inputs
> syscoin-cli walletcreatefundedpsbt "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"
```
</details>

<details><summary>walletlock</summary>

```

Removes the wallet encryption key from memory, locking the wallet.
After calling this method, you will need to call walletpassphrase again
before being able to call any methods which require the wallet to be unlocked.

Examples:

Set the passphrase for 2 minutes to perform a transaction
> syscoin-cli walletpassphrase "my pass phrase" 120

Perform a send (requires passphrase set)
> syscoin-cli sendtoaddress "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 1.0

Clear the passphrase since we are done before 2 minutes is up
> syscoin-cli walletlock 

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "walletlock", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>walletpassphrase "passphrase" timeout</summary>

```

Stores the wallet decryption key in memory for 'timeout' seconds.
This is needed prior to performing transactions related to private keys such as sending syscoins

Note:
Issuing the walletpassphrase command while the wallet is already unlocked will set a new unlock
time that overrides the old one.

Arguments:
1. passphrase    (string, required) The wallet passphrase
2. timeout       (numeric, required) The time to keep the decryption key in seconds; capped at 100000000 (~3 years).

Examples:

Unlock the wallet for 60 seconds
> syscoin-cli walletpassphrase "my pass phrase" 60

Lock the wallet again (before 60 seconds)
> syscoin-cli walletlock 

As a JSON-RPC call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "walletpassphrase", "params": ["my pass phrase", 60] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>walletpassphrasechange "oldpassphrase" "newpassphrase"</summary>

```

Changes the wallet passphrase from 'oldpassphrase' to 'newpassphrase'.

Arguments:
1. oldpassphrase    (string, required) The current passphrase
2. newpassphrase    (string, required) The new passphrase

Examples:
> syscoin-cli walletpassphrasechange "old one" "new one"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "walletpassphrasechange", "params": ["old one", "new one"] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>

<details><summary>walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )</summary>

```

Update a PSBT with input information from our wallet and then sign inputs
that we can sign for.

Arguments:
1. psbt           (string, required) The transaction base64 string
2. sign           (boolean, optional, default=true) Also sign the transaction when updating
3. sighashtype    (string, optional, default=ALL) The signature hash type to sign with if not specified by the PSBT. Must be one of
                  "ALL"
                  "NONE"
                  "SINGLE"
                  "ALL|ANYONECANPAY"
                  "NONE|ANYONECANPAY"
                  "SINGLE|ANYONECANPAY"
4. bip32derivs    (boolean, optional, default=false) If true, includes the BIP 32 derivation paths for public keys if we know them

Result:
{
  "psbt" : "value",          (string) The base64-encoded partially signed transaction
  "complete" : true|false,   (boolean) If the transaction has a complete set of signatures
  ]
}

Examples:
> syscoin-cli walletprocesspsbt "psbt"
```
</details>

## Zmq
<details><summary>getzmqnotifications</summary>

```

Returns information about the active ZeroMQ notifications.

Result:
[
  {                        (json object)
    "type": "pubhashtx",   (string) Type of notification
    "address": "...",      (string) Address of the publisher
    "hwm": n                 (numeric) Outbound message high water mark
  },
  ...
]

Examples:
> syscoin-cli getzmqnotifications 
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getzmqnotifications", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8370/
```
</details>
