
# 21 · BLOCKCHAIN & WEB3

## 21.1 Blockchain

### Blockchain

**Definition:** Distributed ledger; append-only chain of blocks; cryptographic linking.

**Structure:**
- **Block:** Header + transactions
- **Header:** Previous hash, Merkle root, timestamp, nonce, difficulty
- **Chain:** Each block references previous hash

**Properties:**
- **Immutability:** Changing a block invalidates all subsequent
- **Decentralization:** No central authority
- **Transparency:** All transactions visible
- **Pseudonymity:** Addresses, not identities

**Consensus:** PoW, PoS, PBFT, DAG.

**Bitcoin:** First cryptocurrency; PoW; UTXO model.
**Ethereum:** Smart contracts; PoS (post-Merge); account model.

**Recall:** *Immutable chain.*

### Smart Contracts

**Definition:** Self-executing code on blockchain.

**Languages:** Solidity (Ethereum), Rust (Solana), Move (Aptos/Sui).

**EVM (Ethereum Virtual Machine):** Executes bytecode; gas metering.

**Example (Solidity):**
```solidity
contract SimpleStorage {
    uint256 value;
    function set(uint256 v) public { value = v; }
    function get() public view returns (uint256) { return value; }
}
```

**Use:** DeFi, NFTs, DAOs, supply chain.

**Recall:** *Code as contract.*

### Web3

**Definition:** Decentralized web; blockchain-based.

**Components:**
- **Wallets:** MetaMask, Ledger
- **DAOs:** Decentralized Autonomous Organizations
- **dApps:** Decentralized applications
- **Tokens:** ERC-20 (fungible), ERC-721 (NFT), ERC-1155 (multi)

**Recall:** *Decentralized internet.*

### Consensus

**PoW (Proof of Work):** Miners solve puzzle; energy-intensive.

**PoS (Proof of Stake):** Validators stake tokens; slashing for misbehavior.

**PBFT (Practical Byzantine Fault Tolerance):** Voting-based; 3f+1 nodes.

**DAG (Directed Acyclic Graph):** Parallel transactions; IOTA, Hedera.

**Recall:** *PoW, PoS, PBFT, DAG.*

### Layer 2

**Definition:** Scaling solutions on top of Layer 1.

**Types:**
- **Optimistic rollups:** Assume valid; challenge period (Optimism, Arbitrum)
- **ZK rollups:** Validity proofs; immediate finality (zkSync, StarkNet)
- **Sidechains:** Independent chain (Polygon PoS)
- **State channels:** Off-chain transactions (Lightning Network)

**Benefits:** Lower fees, higher throughput.

**Recall:** *Rollups, sidechains, state channels.*

### DeFi

**Definition:** Decentralized Finance; financial services on blockchain.

**Components:**
- **AMMs (Automated Market Makers):** Uniswap, Curve
- **Lending:** Aave, Compound
- **Stablecoins:** DAI, USDC
- **Oracles:** Chainlink, Pyth
- **Derivatives:** Synthetix, dYdX

**Recall:** *AMMs, lending, stablecoins, oracles.*

**SOTA:** ZK rollups, modular blockchains, account abstraction, restaking.

---
