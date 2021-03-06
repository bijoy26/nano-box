## What is Nano?
A digital payment protocol designed to be accessible and lightweight, with a focus on removing inefficiencies present in other cryptocurrencies.
Nano has ultrafast transactions and zero fees on a secure, green and decentralized network.

## Features
- Minimal block size resulting in ultrafast transaction confirmations
- No traditional Proof-of-Work and mining (less energy per transaction)
- Emergent centralization forces for node operators are reduced 

Transactions in Nano occur between accounts with two separate actions:

1. The sender publishes a block debiting their own account for the amount to be sent to the receiving account
2. The receiver publishes a matching block crediting their own account for the amount sent

## Block Contents
- Follows same structure for each block
- contains all the information about an account at that point in time: **account number, balance, representative**
- Also contains a small, user-generated Proof-of-Work value

## Consensus Mechanism
- Unique consensus mechanism called [Open Representative Voting](https://docs.nano.org/glossary/#open-representative-voting-orv)
- Every account can freely delegate a **Representative** at any time to vote on their behalf
- Representatives vote on the validity of transactions they see on the network
- Representative's Voting weight is the sum of balances for accounts delegating to them
- The votes of **Principal Representatives** (high voting weight holders) are rebroadcast by other nodes in the network
- No direct monetary incentive for nodes (removes the possibility of centralization forces)


