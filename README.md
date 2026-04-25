# 🏆 Decentralized Digital Warranty System (Monad Blitz)

> **Elevator Pitch:** A fast, secure, and scalable NFT-based digital warranty solution designed for businesses of all sizes (retailers, e-commerce, and enterprises). It eliminates counterfeit receipts, prevents physical paper loss, and ensures verifiable ownership, fully powered by Monad's extreme high throughput and low-latency execution.

---

## 🌍 Deployed Contract Details

The smart contracts are fully deployed and verified on the Monad test network.

- **Network:** Monad Testnet (Chain ID: `10143`)
- **RPC URL:** `https://testnet-rpc.monad.xyz`
- **Contract Address:** [`0xEd937600da7A74b71154f1F070A1953A64B37fa6`](https://testnet.monadexplorer.com/address/0xEd937600da7A74b71154f1F070A1953A64B37fa6)
- **Deployer (Default Admin):** `0x33c346Ee8F1418c56Efed81d2277Fa2426401cdb`

---

## ✨ Core Features

1. 🔐 **Enterprise Access Control:** Leveraging OpenZeppelin's `AccessControl` for enterprise-grade security. Features strict separation of operations with `ADMIN_ROLE` (for catalog managers) and `MINTER_ROLE` (for automated systems/cashiers).
2. 📋 **Product Registry (Whitelisting):** An on-chain cataloging registry mapping to ensure absolute data consistency across thousands of product lines. Warranties cannot be issued for unregistered product types.
3. 🔢 **Auto-Generated Serial Numbers:** Secure, on-chain unique human-readable Serial Numbers (SN) generated using advanced `keccak256` hashing logic to prevent counterfeiting (e.g., `PROD-A1B2C3D4-2026`).
4. ⚡ **High-Volume Batch Minting:** A highly gas-efficient `unchecked` indexed loop designed to mint massive amounts of warranties at once (CSV upload ready), aggressively exploiting Monad's parallel execution and extreme scale throughput.

---

## 🛠 Tech Stack

- **Smart Contracts:** Solidity `^0.8.24`
- **Framework:** Hardhat (v2.22.15) tuned with EVM Target: `cancun`
- **Libraries:** OpenZeppelin Contracts v5 (`ERC721URIStorage`, `AccessControl`, `Strings`), Ethers.js
- **Network Target:** Monad Testnet

---

## 🚀 Local Setup & Deployment

A brief guide for judges and reviewers to test the environment locally.

### 1. Clone & Install Dependencies
```bash
git clone https://github.com/your-org/garansi-digital-monad.git
cd garansi-digital-monad

# Install Hardhat and plugins
npm install
```

### 2. Environment Configuration
Create a `.env` file at the root of the repository:
```env
PRIVATE_KEY="your_wallet_private_key_here"
```

### 3. Compile Contracts
Compile the contracts to generate the TypeChain bindings and artifacts.
```bash
npx hardhat compile
```

### 4. Deploy to Monad Testnet
Deploy the contract using the provided Hardhat script. The deployer wallet will automatically be granted the `DEFAULT_ADMIN_ROLE`.
```bash
npx hardhat run scripts/deploy.ts --network monad-testnet
```

---

## 💻 Frontend Integration Guide

When integrating the frontend, connect to the deployed contract using its ABI and the address above. Here are the primary interaction points:

### 1. Register a Product Type
*Requires `ADMIN_ROLE`.* Must be called before issuing warranties for a specific product ID.
```solidity
function registerProductType(uint256 productId, string calldata productName) external;
```

### 2. Issue a Single Warranty
*Requires `MINTER_ROLE`.* Mints an NFT warranty to the customer.
```solidity
function issueWarranty(
    address customer,
    uint256 productId,
    uint256 durationInDays,
    string calldata metadataURI
) public returns (uint256 tokenId);
```

### 3. Batch Issue Warranties
*Requires `MINTER_ROLE`.* Optimized for uploading a CSV of buyers and bulk-issuing their digital warranties in a single, high-throughput transaction.
```solidity
function batchIssueWarranty(
    address[] calldata customers,
    uint256 productId,
    uint256 durationInDays,
    string[] calldata metadataURIs
) external;
```
