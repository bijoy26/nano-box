## Run a Node
Running a node is a key way to help decentralize the network and provide a network access point for systems built on top of Nano.

### Node types
1. **Non-voting nodes:** No participation in consensus, recommended for all types of integrations
2. **Representative nodes:** Configured (voting weight less than 0.1%) to validate and vote on transactions seen on the network. 
3. **Principal Representative nodes:** Representative nodes with minimum 0.1% of the voting weight delegated. Votes are rebroadcast by their network peers.

### Hardware Specs

Nodes consume CPU, RAM, disk IO and bandwidth IO resources based on various factors like how often _RPC calls_ are made.

### Minimum Requirements for Non-voting and Representative Nodes

- 2GB RAM (additional RAM or swap space may be needed if bootstrapping a new node from scratch)
- Dual-Core CPU
- 100 Mbps bandwidth (2TB or more of available monthly bandwidth)
- SSD-based hard drive with 80GB+ of free space