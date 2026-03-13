🗳️ Vora: Decentralized Token-Weighted Governance
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
│   ├── governance/            # Main voting & quorum logic
│   ├── treasury/              # Optional: Logic for executing fund transfers
│   └── tests/                 # Comprehensive Rust unit tests
├── frontend/
│   ├── app/                   # Next.js App Router (Proposals, Create, Profile)
│   ├── components/            # Shadcn/ui-based voting components
│   ├── hooks/                 # Custom hooks for Soroban contract calls
│   └── lib/                   # Stellar SDK & XDR helpers
└── docs/                      # Technical specification & API docs
🚦 Getting Started
Prerequisites
Stellar CLI installed.

Node.js 18+ & Rust (latest stable).

1. Smart Contract Deployment
Bash
# Compile to WASM
stellar contract build

# Deploy to Testnet/Futurenet
stellar contract deploy --wasm target/wasm32-unknown-unknown/release/vora_gov.wasm --source-account <YOUR_KEY> --network testnet
2. Frontend Installation
Bash
cd frontend
npm install
npm run dev
