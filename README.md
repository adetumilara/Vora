🗳️ Vora: Decentralized Token-Weighted Governance

## Table of Contents

- [Overview](#-overview)
  - [The Vision](#the-vision)
- [Features](#-features)
  - [Weighted Voting Logic](#weighted-voting-logic)
    - [Balance Snapshots](#balance-snapshots)
    - [Quadratic Voting (Optional)](#quadratic-voting-optional)
  - [Proposal Lifecycle Management](#proposal-lifecycle-management)
    - [Drafting](#drafting)
    - [Active Window](#active-window)
    - [Execution](#execution)
  - [Delegate Engine](#delegate-engine)
  - [Modern Dashboard](#modern-dashboard)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
  - [Smart Contracts](#smart-contracts)
  - [Frontend](#frontend)
  - [State Management](#state-management)
  - [Wallet Integration](#wallet-integration)
  - [Data Layer](#data-layer)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Smart Contract Deployment](#smart-contract-deployment)
  - [Frontend Installation](#frontend-installation)
- [Project Structure](#-project-structure)
  - [Contracts Directory](#contracts-directory)
  - [Frontend Directory](#frontend-directory)
  - [Docs Directory](#docs-directory)
- [Smart Contracts](#-smart-contracts)
  - [Governance Contract](#governance-contract)
  - [Treasury Contract](#treasury-contract)
  - [Testing](#testing)
- [Frontend Application](#-frontend-application)
  - [App Router](#app-router)
  - [Components](#components)
  - [Hooks](#hooks)
  - [Utilities](#utilities)
- [Testing](#-testing)
- [Deployment](#-deployment)
  - [Testnet Deployment](#testnet-deployment)
  - [Mainnet Deployment](#mainnet-deployment)
- [Configuration](#-configuration)
- [API Reference](#-api-reference)
- [Contributing](#-contributing)
  - [Code of Conduct](#code-of-conduct)
  - [Pull Requests](#pull-requests)
- [Roadmap](#-roadmap)
  - [Phase 1: Foundation](#phase-1-foundation)
  - [Phase 2: Expansion](#phase-2-expansion)
  - [Phase 3: Maturation](#phase-3-maturation)
- [FAQ](#-faq)
  - [What is Vora?](#what-is-vora)
  - [How does token-weighted voting work?](#how-does-token-weighted-voting-work)
  - [What wallets are supported?](#what-wallets-are-supported)
  - [Is this audited?](#is-this-audited)
- [License](#-license)
- [Support](#-support)
  - [Discord](#discord)
  - [Twitter](#twitter)
  - [Email](#email)

---

Vora is a high-performance governance protocol built on Stellar using Soroban smart contracts. It enables community-led projects to implement transparent, tamper-proof, and weighted voting systems where influence is proportional to token ownership or historical contribution.

🌟 Overview
In decentralized ecosystems, "one person, one vote" is often vulnerable to Sybil attacks. Vora solves this by implementing Token-Weighted Voting. Whether you are managing a community treasury, a DAO, or a Drips Wave project backlog, Vora ensures that those with the most "skin in the game" have a proportionate say in the direction of the project.

The Vision
To replace opaque "behind-closed-doors" decision-making with a verifiable, on-chain record of community intent, powered by the speed and low cost of the Stellar network.

🚀 Key Features

1. Weighted Voting Logic
   Balance Snapshots: Soroban contracts capture token balances at the exact moment a proposal is created to prevent "flash-loan" voting manipulation.

Quadratic Voting (Optional): A built-in toggle to reduce the power of "whales" by making each additional vote exponentially more expensive.

2. Proposal Lifecycle Management
   Drafting: Proposers can submit Markdown-based descriptions stored via IPFS.

Active Window: Customizable voting periods (e.g., 3 days, 1 week).

Execution: Automated "Enactment" hooks that trigger other smart contracts (like moving funds from a treasury) if a quorum is met.

3. Delegate Engine
   Users can delegate their voting power to trusted community experts without losing custody of their tokens.

4. Modern Dashboard
   A Next.js 14 interface with Tailwind CSS featuring real-time progress bars, quorum trackers, and voter distribution charts.

🛠 Technical Stack
Smart Contracts: Rust & Soroban (WASM)

Frontend: Next.js (App Router), Tailwind CSS, Lucide React (Icons)

State Management: TanStack Query (React Query) for fetching ledger data

Wallet: Freighter & Albedo integration via Stellar-Wallet-Adapter

Data Layer: Horizon API & Soroban RPC

🏗 System Architecture
Code snippet
graph TD
A[Token Holder] -->|Signs with Freighter| B[Vora Smart Contract]
B -->|Checks Balance| C[Stellar Ledger Snapshot]
B -->|Records Vote| D[(On-Chain State)]
D -->|Aggregates Results| E[Next.js Dashboard]
E -->|Visualizes| F[Community Member]
📂 Project Structure
Plaintext
├── contracts/
│ ├── governance/ # Main voting & quorum logic
│ ├── treasury/ # Optional: Logic for executing fund transfers
│ └── tests/ # Comprehensive Rust unit tests
├── frontend/
│ ├── app/ # Next.js App Router (Proposals, Create, Profile)
│ ├── components/ # Shadcn/ui-based voting components
│ ├── hooks/ # Custom hooks for Soroban contract calls
│ └── lib/ # Stellar SDK & XDR helpers
└── docs/ # Technical specification & API docs
🚦 Getting Started
Prerequisites
Stellar CLI installed.

Node.js 18+ & Rust (latest stable).

1. Smart Contract Deployment
   Bash

# Compile to WASM

stellar contract build

# Deploy to Testnet/Futurenet

stellar contract deploy --wasm target/wasm32-unknown-unknown/release/vora_gov.wasm --source-account <YOUR_KEY> --network testnet 2. Frontend Installation
Bash
cd frontend
npm install
npm run dev
