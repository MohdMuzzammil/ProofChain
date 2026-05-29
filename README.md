# ProofChain

ProofChain is a JavaScript blockchain prototype that demonstrates the core mechanics behind distributed ledger systems: blocks, transactions, proof of work, peer-to-peer node registration, transaction broadcasting, mining rewards, and longest-chain consensus.

This project is intentionally lightweight and readable. It is designed to show how blockchain concepts work under the hood without hiding the logic behind frameworks or external blockchain platforms.

## Why This Project Matters

Blockchain is often discussed as a business buzzword, but the real value comes from a few practical engineering ideas:

- A shared append-only ledger that multiple nodes can inspect
- Tamper-evident blocks linked by cryptographic hashes
- Network-wide transaction broadcasting
- Consensus rules for resolving competing versions of the chain
- Incentives through mining rewards

ProofChain turns those ideas into a working Node.js API, making it useful for demos, interviews, technical learning, and architecture discussions.

## Technical Highlights

- Built a working blockchain model from first principles in JavaScript
- Implemented proof-of-work mining using SHA-256 hashing
- Designed REST APIs for transactions, mining, node registration, and consensus
- Simulated a decentralized network using multiple local Express servers
- Added transaction broadcasting and mining rewards across peer nodes
- Implemented longest-chain consensus to synchronize distributed nodes
- Kept the codebase small and readable for quick review and discussion

## Tech Stack

- Node.js
- Express.js
- SHA-256 hashing
- UUID transaction and node identifiers
- request-promise for inter-node communication
- Nodemon for local multi-node development

## Core Features

### Blockchain Ledger

Each block contains:

- Block index
- Timestamp
- Transactions
- Nonce
- Current block hash
- Previous block hash

The chain starts with a genesis block and grows as new blocks are mined.

### Transactions

Transactions include an amount, sender, recipient, and unique transaction ID. New transactions are first stored as pending transactions, then included in the next mined block.

### Proof of Work

The mining process searches for a nonce that creates a block hash beginning with `0000`. This demonstrates the computational work required to create a valid block.

### Mining Rewards

After a block is mined, the node receives a reward transaction. This mirrors the incentive model used by many blockchain systems.

### Multi-Node Network

Multiple local nodes can be started on different ports. Nodes can register with each other, broadcast transactions, receive newly mined blocks, and synchronize their chains.

### Consensus

The `/consensus` endpoint checks peer nodes and replaces the local chain when a longer valid chain exists. This demonstrates the longest-chain rule used to resolve distributed state differences.

## Project Structure

```text
.
|-- index.js              # Express API and network behavior
|-- src/
|   `-- blockchain.js     # Blockchain data model and core algorithms
|-- package.json          # Scripts and dependencies
`-- README.md
```

## Getting Started

### Prerequisites

- Node.js
- npm

### Install Dependencies

```bash
npm install
```

### Start Local Nodes

Open separate terminal windows and run:

```bash
npm run node_1
npm run node_2
npm run node_3
npm run node_4
```

The scripts start nodes on ports `3001`, `3002`, `3003`, and `3004`.

## API Reference

### Health Check

```http
GET /
```

Returns a simple status response.

### View Blockchain

```http
GET /blockchain
```

Returns the current node's blockchain, pending transactions, node URL, and known network nodes.

### Create Transaction on Current Node

```http
POST /transaction
Content-Type: application/json

{
  "amount": 100,
  "sender": "sender-address",
  "recipient": "recipient-address"
}
```

Adds a transaction to the current node's pending transaction pool.

### Create and Broadcast Transaction

```http
POST /transaction/broadcast
Content-Type: application/json

{
  "amount": 100,
  "sender": "sender-address",
  "recipient": "recipient-address"
}
```

Creates a transaction and broadcasts it to every known node.

### Mine a Block

```http
GET /mine
```

Runs proof of work, creates a new block, broadcasts it to peer nodes, and creates a mining reward transaction.

### Register and Broadcast a New Node

```http
POST /register-and-broadcast-node
Content-Type: application/json

{
  "newNodeUrl": "http://localhost:3002"
}
```

Registers a new node with the current node and broadcasts that node to the rest of the network.

### Register a Node

```http
POST /register-node
Content-Type: application/json

{
  "newNodeUrl": "http://localhost:3002"
}
```

Registers a node with the current node.

### Bulk Register Nodes

```http
POST /register-node-bulk
Content-Type: application/json

{
  "allNetworkNodes": [
    "http://localhost:3001",
    "http://localhost:3002",
    "http://localhost:3003"
  ]
}
```

Registers multiple known network nodes at once.

### Run Consensus

```http
GET /consensus
```

Compares the current chain with peer chains and adopts the longest valid chain when appropriate.

## Example Workflow

1. Start two or more nodes.
2. Register the nodes with each other using `/register-and-broadcast-node`.
3. Create a transaction using `/transaction/broadcast`.
4. Mine a block using `/mine`.
5. Inspect each node using `/blockchain`.
6. Run `/consensus` to synchronize nodes when needed.

## Business Use Cases Demonstrated

ProofChain can help explain or demo:

- Distributed ledgers for financial transactions
- Audit trails where tamper evidence matters
- Multi-party record synchronization
- Consensus-based data integrity
- Basic blockchain architecture for product or stakeholder conversations

## Current Scope

ProofChain is a prototype for learning and demonstration. It focuses on clarity and blockchain fundamentals, not production readiness.

Production-grade blockchain systems would also require:

- Persistent storage
- Stronger networking and failure handling
- Authentication and authorization
- Transaction signatures and wallet management
- Better validation and security hardening
- Automated test coverage
- Modern replacement for deprecated request libraries

## What This Shows About the Developer

This project demonstrates comfort with backend JavaScript, REST API design, distributed-system concepts, algorithmic thinking, and the ability to translate abstract technical ideas into a working implementation.

## License

ISC
