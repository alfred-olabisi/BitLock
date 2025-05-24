# 🟧 BitLock Protocol

**Bitcoin-Native Staking & Governance on Stacks**

**BitLock** is a decentralized staking and governance protocol built on the Stacks blockchain, enabling Bitcoin holders to earn rewards and participate in on-chain governance. It introduces a tiered staking system, governance proposal mechanisms, and dynamic reward calculations—all secured by Bitcoin finality.

---

## 🚀 Features

* **Bitcoin-Native Rewards**: Stake STX tokens and earn protocol-native rewards tied to your staking behavior.
* **Tiered Staking System**: Higher staking amounts and longer lock periods unlock increased reward multipliers and protocol privileges.
* **On-Chain Governance**: Propose and vote on protocol upgrades or parameters using your voting power.
* **Cooldown Unstaking Flow**: A secure two-step process for unstaking with a configurable cooldown.
* **Pause/Emergency Controls**: Protocol admins can pause operations for safety and recovery.

---

## 🛠 Architecture Overview

```text
+--------------------------+
|    User Interaction      |
|--------------------------|
| - Stake STX              |
| - Vote on Proposals      |
| - Claim Rewards          |
| - Unstake via Cooldown   |
+------------+-------------+
             |
             v
+--------------------------+
|   Smart Contract Layer   |
|--------------------------|
| Contracts written in     |
| Clarity on Stacks.       |
|                          |
| Key Modules:             |
| - Token Management       |
| - UserPositions Map      |
| - Tier Configuration     |
| - Proposal Governance    |
| - Reward Logic           |
+------------+-------------+
             |
             v
+--------------------------+
|   State & Data Storage   |
|--------------------------|
| - `UserPositions`        |
| - `StakingPositions`     |
| - `Proposals`            |
| - `TierLevels`           |
| - Protocol Variables     |
+--------------------------+
```

---

## 📦 Smart Contract Modules

### 🧱 Core Components

| Component          | Description                           |
| ------------------ | ------------------------------------- |
| `BITLOCK-TOKEN`    | Fungible governance token             |
| `UserPositions`    | Tracks user stake, tier, voting power |
| `StakingPositions` | Stores active stake details           |
| `Proposals`        | Governance proposals data             |
| `TierLevels`       | Tier thresholds & feature flags       |
| `contract-paused`  | Emergency toggle by admin             |

---

## 🧮 Reward Calculation

Rewards are calculated based on:

* **Base Rate**: Default 5% APY
* **Tier Multiplier**: Based on staking tier (1x–2x)
* **Lock Multiplier**: Based on lock period (1x, 1.25x, 1.5x)
* Formula:

  ```clarity
  (* (* (* stake-amount base-rate) tier-multiplier) lock-multiplier) / u14400000
  ```

---

## 🔐 Governance System

* **Proposal Creation**: Requires minimum voting power.
* **Voting**: Open until `end-block`, votes are counted using stored voting power.
* **Execution**: Requires reaching `minimum-votes` threshold.
* **Proposal Lifecycle**:

  1. Created with description & voting period
  2. Users vote (for/against)
  3. If quorum is met and voting period ends, it can be executed (future implementation)

---

## ⚙ Admin Functions

* `initialize-contract`: Sets tier data (only once)
* `pause-contract` / `resume-contract`: Emergency controls
* `contract-paused`: Global pause check for core operations

---

## 🧪 Setup & Deployment

### Requirements

* **Clarity Developer Environment**
* **Stacks CLI** (`clarinet`)
* **Stacks Wallet** (for testing transactions)

### Local Deployment

```bash
clarinet check       # Validate Clarity syntax
clarinet test        # Run tests
clarinet console     # Interact with the contract
```

### Contract Initialization

Run:

```clarity
(initialize-contract)
```

---

## 📘 Example Usage

### Stake STX

```clarity
(stake-stx u1000000 u4320) ;; Stake 1 STX for 1 month
```

### Create Proposal

```clarity
(create-proposal "Upgrade tier multipliers" u200)
```

### Vote on Proposal

```clarity
(vote-on-proposal u1 true)
```

---

## 🧠 Notes

* All values are in micro-STX (`uSTX`)
* Voting power and reward multipliers are recalculated on stake
* Protocol supports expansion for feature flags and deeper on-chain governance
