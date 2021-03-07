# Nano Basics

## Block Lattice Design

### - Nano's ledger is built on a data-structure called a "Block Lattice". 
### - Every account (private/public key pair) has their own blockchain (account-chain). Only the holder of the private key may sign and publish blocks to their own account-chain. 
### - Each block represents a transaction.
### - Only two actions - SEND and RECEIVE funds


## Account, Key, Seed and Wallet IDs

> NOTE : There are several things that can have a similar form but may have very different functions, and mixing them up can result in loss of funds. Use caution when handling them.

### Wallet ID
 
- A series of 32 random bytes of data and is **not the seed**
- A purely local UUID that is a reference to a block of data about a specific wallet (set of seed/private keys/info about them) in your node's local database file. 
- It **will not recover funds** if that database is lost or corrupted.
- Created with `wallet_create` RPC command

### Seed
- A series of 32 random bytes of data (uppercase hex string)
- Used to deterministically derive account private keys for accounts-
		- First combines private key with an index i (32 bit big-endian unsigned int)
		- Then puts it into the following hash function-
			`PrivK[i] = blake2b(outLen = 32, input = seed || i)` (|| means concat)
- Knowing it allows to access all the derived private keys from index 0 to 2^32 − 1
- Nano reference wallet is described using **Blake2b private keys derivation** (there are also **BIP44 deterministic wallets** and mnemonic seed with different private keys)
			