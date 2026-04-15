
---

# 📦 Franchise Auction System (Solidity + NFT)

A fully on-chain auction system where franchises are sold via bidding and minted as NFTs to winning bidders. 

---

## 🚀 Overview

This project implements a decentralized auction platform for “franchises” (e.g., sports teams, league slots, or digital assets). Each franchise is auctioned, and the winner receives a unique on-chain NFT representing ownership.

Two core contracts:

* `FranchiseAuction.sol` → Auction & bidding logic
* `FranchiseNFT.sol` → NFT minting + on-chain metadata

---

## ⚙️ Features

### 🏟 Franchise Auction System

* Create auctions with name, season, price, and deadline
* Extend auction deadlines (owner only)
* Delete franchises (only if no bids exist)
* Prevent duplicate franchise names

### 💰 Bidding Engine

* Fully ETH-based bidding system
* Automatic refund tracking for outbid users
* Anti-owner participation protection
* Minimum bid enforcement

### 🧾 Auction Settlement

* Auto-closes after deadline
* Mints NFT to winner
* Tracks owner revenue
* Handles “no bid” scenarios safely

### 🖼 NFT System (On-chain)

* ERC721 implementation
* Fully on-chain SVG metadata generation
* Base64 encoded tokenURI
* Franchise-specific visual styling via SVG generator

### 🔐 Security Features

* ReentrancyGuard on ETH flows
* Pausable emergency stop
* Custom errors (gas optimized)
* Safe ETH transfer handling
* Auction state validation everywhere

---

## 🧠 Architecture

```
FranchiseAuction
    │
    ├── Stores auction state (bids, deadlines, winners)
    ├── Handles bidding + refund accounting
    ├── Calls mintPosition() on NFT contract
    │
    ▼
FranchiseNFT
    ├── Mints ERC721 tokens
    ├── Stores franchise metadata
    ├── Generates on-chain SVG + metadata
```

---

## 🔄 Auction Flow

1. Owner creates franchise auction
2. Users place ETH bids
3. Contract tracks highest bidder
4. Outbid users get refundable balance
5. After deadline:

   * Owner closes auction
   * NFT is minted to winner
   * Revenue is recorded

---

## 🧪 Key Functions

### Auction Contract

* `createFranchise()`
* `bid()`
* `closeAuction()`
* `withdrawRefund()`
* `withdrawOwnerRevenue()`

### NFT Contract

* `mintPosition()` *(only auction contract)*
* `tokenURI()` *(fully on-chain metadata)*
* `setAuction()` *(auction contract address)*

---

## 📊 Events

### Auction Events

* `FranchiseCreated`
* `BidPlaced`
* `AuctionClosed`
* `AuctionClosedNoBids`
* `RefundWithdrawn`
* `OwnerRevenueWithdrawn`

### NFT Events

* `PositionMinted`
* `AuctionSet`

---

## 🚨 Security Notes (READ THIS)

This is where your system is **good but not bulletproof yet**:

### ⚠️ Known risks / improvements

* `pendingRefunds` is safe but can become messy at scale → consider pull-payment pattern upgrade
* No bid increment percentage → MEV/sniping is still possible
* `getAllFranchises()` is O(n) → will break under large scale
* No whitelist / anti-bot bidding protection
* Auction extension logic is minimal (only extension, no shortening safeguards)
* Revenue is tracked internally but not enforced against NFT mint failures (edge risk)

If this is going into a hackathon demo: fine.
If this is going to mainnet: you still have work.

---

## 🧾 Deployment Steps

### 1. Deploy NFT contract

```solidity
FranchiseNFT nft = new FranchiseNFT();
```

### 2. Deploy Auction contract

```solidity
FranchiseAuction auction = new FranchiseAuction(admin, address(nft));
```

### 3. Link contracts

```solidity
nft.setAuction(address(auction));
```

---

## 🧪 Example Usage

### Create franchise auction

```solidity
createFranchise("Karachi Kings", 9, 1 ether, block.timestamp + 7 days);
```

### Place bid

```solidity
bid(1) payable
```

### Close auction

```solidity
closeAuction(1);
```

---

## 🖼 NFT Output

Each NFT includes:

* Franchise name
* Token ID
* Owner address
* On-chain SVG artwork
* Base64 encoded metadata

---

## 📌 Tech Stack

* Solidity ^0.8.20
* OpenZeppelin Contracts
* ERC721 Standard
* On-chain SVG generation
* Base64 encoded metadata

---

#
