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
- Used to deterministically derive **account private keys** for accounts-
		- First combines private key with an index i (32 bit big-endian unsigned int)
		- Then puts it into the following hash function-
			`PrivK[i] = blake2b(outLen = 32, input = seed || i)` (|| means concat)
- Knowing it allows to access all the derived private keys from index 0 to 2^32 − 1
- Nano reference wallet is described using **Blake2b private keys derivation** (there are also **BIP44 deterministic wallets** and mnemonic seed with different private keys)

## Account private key
- A 32 byte value of uppercase hexadecimal string(0-9A-F)
- It can either be random or derived from a seed.
- It represents control of a specific account on the ledger

## Account public key
- A 32 byte value of uppercase hexadecimal string(0-9A-F)
- Derived from an **account private key** by using the _ED25519 curve_ using **Blake2b-512** as the hash function (instead of SHA-512)

## Account public address
- This is someone's actual Nano address with prefix nano_ (previously xrb_)
- After prefix, has **52 characters public key** and 8 **encoded checksum characters** (60 char length in total)
- Account **public key** is encoded with a specific _base32 encoding algorithm_ to prevent human transcription errors 
- Final 8 characters are **Blake2b-40 checksum** of the account public key to aid in discovering typos, also encoded with same base32 algorithm

NOTE: So for the address `nano_1anrzcuwe64rwxzcco8dkhpyxpi8kd7zsjc1oeimpc3ppca4mrjtwnqposrs`, we get:

| Prefix | Encoded Account Public Key                           | Checksum |
|--------|------------------------------------------------------|----------|
| nano_  | 1anrzcuwe64rwxzcco8dkhpyxpi8kd7zsjc1oeimpc3ppca4mrjt | wnqposrs |

## Address Validation
For basic address validation, the following regular expression can be used: `^(nano|xrb)_[13]{1}[13456789abcdefghijkmnopqrstuwxyz]{59}$`.

## Units
- Nano can be represented using more than one unit of measurement. While the most common unit is the **Nano**, the smallest unit is the **Raw**.
- Formula for conversion: `1 nano = 10^30 raw`
- All RPC commands expect units to be represented as raw (use integer value always)
- To perform arithmetic directly on raw, a big number library need to be imported depending on the implementation language
- API calls must be done carefully to avoid loss of funds (final balances are recorded rather than transaction amounts)

## Blocks Specifications & Format
- All transactions are communicated via blocks which contains the account's entire state, including the balance after each transaction.
- The first block, often referred to as "opening the account", must be receiving funds only. 

Block anatomy -

| Key            | Description                                     |
|----------------|-------------------------------------------------|
| type           | "state"                                         |
| account        | This account's nano_ address                    |
| previous       | Previous head block on account; 0 if open block |
| representative | Representative nano_ address                    |
| balance        | Resulting balance (in raw)                      |
| link           | Multipurpose field                              |
| signature      | ED25519+Blake2b 512-bit signature               |
| work           | Proof of Work Nonce                             |
	
Depending on the action each transaction intends to perform, the "link" field will have a different value for `block_create` RPC command.
But in signed transaction json, the "link" field is always hexadecimal.

| Action  |  Description                                |
|---------|---------------------------------------------|
| Change  | Must be "0"  (change representative)		  |
| Send    | Destination "nano_" address                 |
| Receive | Pairing block's hash (block sending funds)  |

## Self-Signed Blocks

- implementation of your own signing is possible
- Follow the order of data (in bytes) to hash prior to signing-
	1. block preamble (32-Bytes, value `0x6`)
	2. account (32-Bytes)
	3. previous (32-Bytes)
	4. representative (32-Bytes)
	5. balance (16-Bytes)
	6. link (32-Bytes) 
- All values are binary representations (No ASCII/UTF-8 strings)
- The digital signing algorithm (which internally applies another Blake2b hashing) is applied on the resulting digest.


## URI Standard

- Send nano(raw) to an address- `nano:nano_<encoded address>[?][amount=<raw amount>][&][label=<label>][&][message=<message>]`
- Representative change - `nanorep:nano_<encoded address>[?][label=<label>][&][message=<message>]`
- Private Key Import - `nanokey:<encoded private key>[?][label=<label>][&][message=<message>]`
- Seed import - `nanoseed:<encoded seed>[?][label=<label>][&][message=<message>][&][lastindex=<index>]`
- Process a JSON blob block - `nanoblock:<blob>`









