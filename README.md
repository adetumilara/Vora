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

## 🌟 Overview

Vora is a high-performance governance protocol built on Stellar using Soroban smart contracts. It enables community-led projects to implement transparent, tamper-proof, and weighted voting systems where influence is proportional to token ownership or historical contribution.

In decentralized ecosystems, "one person, one vote" is often vulnerable to Sybil attacks where malicious actors create multiple identities to manipulate voting outcomes. Vora solves this by implementing Token-Weighted Voting, ensuring that those with the most "skin in the game" have a proportionate say in the direction of the project.

Whether you are managing a community treasury, a DAO, or a Drips Wave project backlog, Vora provides the infrastructure for fair, transparent, and efficient governance that scales with your community's needs.

### The Vision

To replace opaque "behind-closed-doors" decision-making with a verifiable, on-chain record of community intent, powered by the speed and low cost of the Stellar network.

Our vision extends beyond simple voting mechanisms. We aim to create a governance ecosystem where:

- Every decision is transparent and auditable
- Community members can participate regardless of technical expertise
- Voting power reflects genuine commitment and contribution
- Governance processes are efficient, cost-effective, and accessible
- DAOs can evolve their governance models without migrating platforms

By leveraging Stellar's Soroban smart contracts, Vora delivers sub-second finality and transaction costs measured in fractions of a cent, making governance accessible to communities of all sizes.

## 🚀 Features

Vora provides a comprehensive suite of governance tools designed for flexibility, security, and user experience.

### Weighted Voting Logic

At the core of Vora is a sophisticated weighted voting system that ensures fair representation while preventing manipulation.

**How It Works:**
- Each token holder's voting power is proportional to their token balance
- Votes are weighted automatically based on the snapshot balance
- Multiple voting strategies can be configured per proposal
- Support for both simple majority and supermajority thresholds

**Key Benefits:**
- Prevents Sybil attacks by tying voting power to economic stake
- Aligns incentives between voters and project success
- Flexible enough to support various governance models
- Transparent calculation visible on-chain

#### Balance Snapshots

Balance snapshots are critical for preventing vote manipulation and ensuring fair governance.

**Snapshot Mechanism:**
- Token balances are captured at the exact block when a proposal is created
- Snapshots are immutable and stored on-chain
- Prevents "flash loan" attacks where users temporarily borrow tokens to vote
- Ensures voters cannot transfer tokens to multiple wallets to vote multiple times

**Technical Implementation:**
```rust
// Pseudo-code representation
fn create_proposal(env: Env, proposer: Address) {
    let snapshot_ledger = env.ledger().sequence();
    let voter_balance = token_contract.balance_of(&voter, snapshot_ledger);
    // Store snapshot reference with proposal
}
```

**Use Cases:**
- Preventing last-minute token accumulation for vote manipulation
- Ensuring consistent voting power throughout the voting period
- Creating historical records of token distribution at decision points

#### Quadratic Voting (Optional)

Quadratic voting is an advanced voting mechanism that reduces the influence of large token holders while still respecting economic stake.

**How Quadratic Voting Works:**
- Voting power = √(token_balance)
- A holder with 100 tokens gets 10 votes (√100)
- A holder with 10,000 tokens gets 100 votes (√10,000)
- This creates a more balanced power distribution

**When to Use Quadratic Voting:**
- Communities concerned about whale dominance
- Proposals requiring broad community consensus
- Decisions affecting smaller stakeholders disproportionately
- Experimental governance models

**Configuration:**
```javascript
// Enable quadratic voting for a proposal
const proposalConfig = {
  votingStrategy: 'quadratic',
  quorumPercentage: 20,
  passingThreshold: 51
};
```

**Trade-offs:**
- ✅ More democratic distribution of power
- ✅ Encourages broader participation
- ❌ More complex to understand for new users
- ❌ May reduce incentive for large token holders

### Proposal Lifecycle Management

Every proposal in Vora follows a structured lifecycle ensuring transparency and predictability.

#### Drafting

The drafting phase allows proposers to create, refine, and prepare proposals before submitting them for voting.

**Drafting Features:**
- Rich Markdown editor with preview
- IPFS integration for decentralized storage of proposal content
- Attachment support for supporting documents
- Community feedback before formal submission
- Draft versioning and edit history

**Creating a Draft:**
```javascript
const draft = await createProposal({
  title: "Allocate 10,000 XLM for Marketing Campaign",
  description: "## Proposal Summary\n\nThis proposal requests...",
  category: "treasury",
  status: "draft"
});
```

**Best Practices:**
- Share drafts in community channels for feedback
- Include clear objectives and success metrics
- Provide detailed budget breakdowns for funding requests
- Allow at least 48 hours for community review before activation

#### Active Window

The active window is the period during which community members can cast their votes.

**Active Window Configuration:**
- Minimum duration: 24 hours (prevents rushed decisions)
- Maximum duration: 30 days (prevents indefinite proposals)
- Customizable per proposal type
- Countdown timers visible in the UI

**During the Active Window:**
- Voters can cast, change, or revoke their votes
- Real-time vote tallies are displayed
- Quorum progress is tracked
- Notifications sent at key milestones (50% quorum, 24h remaining, etc.)

**Voting Process:**
```javascript
await castVote({
  proposalId: "prop_123",
  vote: "for", // "for", "against", "abstain"
  reason: "I support this initiative because..." // Optional
});
```

**Vote Privacy:**
- All votes are public and on-chain by default
- Vote reasoning is optional but encouraged
- Historical voting records build reputation

#### Execution

Execution is the automated process that occurs when a proposal passes, triggering on-chain actions.

**Execution Triggers:**
- Proposal reaches quorum threshold
- Voting period ends
- Passing threshold is met (e.g., >50% approval)
- Timelock period expires (if configured)

**Automated Actions:**
- Treasury fund transfers
- Smart contract parameter updates
- Role assignments or revocations
- Multi-step execution workflows

**Execution Example:**
```rust
// Simplified execution logic
fn execute_proposal(env: Env, proposal_id: u64) {
    let proposal = get_proposal(&env, proposal_id);
    
    if proposal.status == ProposalStatus::Passed {
        // Execute treasury transfer
        treasury_contract.transfer(
            proposal.recipient,
            proposal.amount
        );
        
        proposal.status = ProposalStatus::Executed;
    }
}
```

**Safety Features:**
- Timelock delays for high-value proposals
- Multi-signature requirements for critical actions
- Execution simulation before actual execution
- Automatic rollback on execution failure

### Delegate Engine

The delegate engine allows token holders to delegate their voting power to trusted representatives without transferring token custody.

**Delegation Features:**
- Delegate to any address with a single transaction
- Retain full token custody and transfer rights
- Revoke delegation at any time
- Partial delegation support (delegate only a percentage)
- Multi-level delegation (delegates can re-delegate)

**How Delegation Works:**
```javascript
// Delegate voting power
await delegateVotes({
  delegate: "GDELEGATE_ADDRESS...",
  percentage: 100 // Delegate 100% of voting power
});

// Revoke delegation
await revokeDelegation();
```

**Use Cases:**
- Busy token holders who trust community experts
- Increasing participation rates in governance
- Building reputation for active community members
- Enabling liquid democracy models

**Delegate Dashboard:**
- View who has delegated to you
- See total delegated voting power
- Track voting history and decisions
- Delegate profiles with voting rationale

**Security Considerations:**
- Delegates cannot transfer or access your tokens
- Delegation is transparent and auditable
- Original token holder can override delegate votes
- Delegation automatically expires if tokens are transferred

### Modern Dashboard

The Vora dashboard provides an intuitive interface for all governance activities.

**Dashboard Features:**
- 📊 Real-time proposal analytics
- 🗳️ One-click voting interface
- 📈 Historical governance metrics
- 👥 Delegate discovery and management
- 🔔 Customizable notifications
- 📱 Responsive design for mobile governance

**Key Sections:**

**Proposals View:**
- Filter by status (active, passed, failed, executed)
- Sort by voting deadline, creation date, or popularity
- Search by title, proposer, or category
- Visual indicators for quorum and approval status

**Voting Interface:**
- Clear display of proposal details
- Your voting power calculation
- Current vote distribution (for/against/abstain)
- Time remaining countdown
- Gas-free voting (sponsored transactions)

**Analytics Dashboard:**
- Total proposals created
- Participation rates over time
- Top delegates by voting power
- Treasury balance and allocation
- Voter distribution charts

**User Profile:**
- Your voting history
- Delegations given and received
- Reputation score
- Governance tokens held
- Participation statistics

## 🏗 Architecture

Vora follows a modular, layered architecture designed for scalability, security, and maintainability.

```
┌─────────────────────────────────────────────────────────────┐
│                     Frontend Layer                          │
│  Next.js 14 App Router │ Tailwind CSS │ React Query        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Wallet Integration                        │
│     Freighter │ Albedo │ Stellar Wallet Adapter            │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    API & Data Layer                         │
│    Horizon API │ Soroban RPC │ IPFS Gateway                │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                  Smart Contract Layer                       │
│   Governance Contract │ Treasury │ Token Contract           │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Stellar Network                          │
│         Soroban Runtime │ Ledger State                      │
└─────────────────────────────────────────────────────────────┘
```

**Architecture Principles:**
- **Separation of Concerns:** Clear boundaries between UI, business logic, and blockchain interaction
- **Modularity:** Each component can be upgraded independently
- **Security First:** Multiple validation layers and fail-safe mechanisms
- **Performance:** Optimized for low latency and high throughput
- **Extensibility:** Plugin architecture for custom governance modules

## 🛠 Tech Stack

### Smart Contracts

**Language & Runtime:**
- **Rust:** Memory-safe systems programming language
- **Soroban SDK:** Stellar's smart contract framework
- **WASM:** WebAssembly compilation target for cross-platform execution

**Key Libraries:**
```toml
[dependencies]
soroban-sdk = "20.0.0"
soroban-token-sdk = "20.0.0"
```

**Contract Features:**
- Gas-optimized operations
- Upgradeable contract patterns
- Event emission for off-chain indexing
- Comprehensive error handling
- Built-in access control

### Frontend

**Core Framework:**
- **Next.js 14:** React framework with App Router for optimal performance
- **React 18:** Latest React with concurrent features
- **TypeScript:** Type-safe development experience

**UI Components:**
- **Tailwind CSS:** Utility-first styling
- **Shadcn/ui:** Accessible component library
- **Lucide React:** Modern icon set
- **Recharts:** Data visualization
- **Framer Motion:** Smooth animations

**Development Tools:**
- **ESLint:** Code quality enforcement
- **Prettier:** Code formatting
- **Husky:** Git hooks for pre-commit checks

### State Management

**Data Fetching:**
- **TanStack Query (React Query):** Server state management
  - Automatic caching and revalidation
  - Optimistic updates
  - Background refetching
  - Pagination and infinite scroll support

**Local State:**
- **React Context:** Global app state (theme, wallet connection)
- **Zustand:** Lightweight state management for complex UI state
- **React Hook Form:** Form state and validation

**Example Query:**
```typescript
const { data: proposals, isLoading } = useQuery({
  queryKey: ['proposals', 'active'],
  queryFn: () => fetchActiveProposals(),
  staleTime: 30000, // 30 seconds
  refetchInterval: 60000 // Refetch every minute
});
```

### Wallet Integration

**Supported Wallets:**
- **Freighter:** Browser extension wallet (primary)
- **Albedo:** Web-based wallet
- **Stellar Wallet Adapter:** Unified wallet interface

**Integration Features:**
- Auto-connect on page load
- Network switching (Testnet/Mainnet)
- Transaction signing with user confirmation
- Balance and account info fetching
- Multi-account support

**Connection Flow:**
```typescript
import { useWallet } from '@/hooks/useWallet';

const { connect, disconnect, address, isConnected } = useWallet();

// Connect wallet
await connect('freighter');

// Sign transaction
const result = await signTransaction(xdr);
```

### Data Layer

**Blockchain Data:**
- **Horizon API:** REST API for Stellar network data
  - Account information
  - Transaction history
  - Asset details
  - Network statistics

- **Soroban RPC:** Direct smart contract interaction
  - Contract invocation
  - State queries
  - Event streaming
  - Simulation before execution

**Off-Chain Storage:**
- **IPFS:** Decentralized storage for proposal content
  - Markdown documents
  - Supporting files
  - Images and media
  - Immutable content addressing

**Caching Strategy:**
- Browser localStorage for user preferences
- IndexedDB for large datasets
- Service Worker for offline support
- CDN caching for static assets
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
## 🚦 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

**Required Software:**
- **Node.js 18+** - [Download](https://nodejs.org/)
  ```bash
  node --version  # Should be v18.0.0 or higher
  ```

- **Rust (latest stable)** - [Install via rustup](https://rustup.rs/)
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  rustc --version
  ```

- **Stellar CLI** - [Installation Guide](https://developers.stellar.org/docs/tools/developer-tools)
  ```bash
  cargo install --locked stellar-cli
  stellar --version
  ```

- **Git** - Version control
  ```bash
  git --version
  ```

**Optional but Recommended:**
- **Docker** - For running local Stellar network
- **VS Code** - With Rust Analyzer extension
- **Freighter Wallet** - Browser extension for testing

**Accounts & Keys:**
- Stellar testnet account with XLM balance
- Get testnet XLM from [Friendbot](https://laboratory.stellar.org/#account-creator?network=test)

### Smart Contract Deployment

**Step 1: Clone the Repository**
```bash
git clone https://github.com/your-org/vora.git
cd vora
```

**Step 2: Build Contracts**
```bash
cd contracts

# Install dependencies
cargo build

# Build optimized WASM
stellar contract build

# Output: target/wasm32-unknown-unknown/release/vora_governance.wasm
```

**Step 3: Deploy to Testnet**
```bash
# Configure network
stellar network add \
  --global testnet \
  --rpc-url https://soroban-testnet.stellar.org:443 \
  --network-passphrase "Test SDF Network ; September 2015"

# Generate deployment identity
stellar keys generate deployer --network testnet

# Fund the account
curl "https://friendbot.stellar.org?addr=$(stellar keys address deployer)"

# Deploy governance contract
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/vora_governance.wasm \
  --source deployer \
  --network testnet

# Save the contract ID (output will be something like: CAXXX...)
export GOVERNANCE_CONTRACT_ID="<your-contract-id>"
```

**Step 4: Initialize Contract**
```bash
# Initialize with governance parameters
stellar contract invoke \
  --id $GOVERNANCE_CONTRACT_ID \
  --source deployer \
  --network testnet \
  -- \
  initialize \
  --admin $(stellar keys address deployer) \
  --token_address <YOUR_TOKEN_CONTRACT_ID> \
  --quorum_percentage 20 \
  --voting_period_days 7
```

**Step 5: Verify Deployment**
```bash
# Query contract state
stellar contract invoke \
  --id $GOVERNANCE_CONTRACT_ID \
  --source deployer \
  --network testnet \
  -- \
  get_config
```

### Frontend Installation

**Step 1: Navigate to Frontend Directory**
```bash
cd ../frontend
```

**Step 2: Install Dependencies**
```bash
npm install
# or
yarn install
# or
pnpm install
```

**Step 3: Configure Environment**
```bash
# Copy environment template
cp .env.example .env.local

# Edit .env.local with your values
```

**.env.local Configuration:**
```env
# Network Configuration
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_HORIZON_URL=https://horizon-testnet.stellar.org
NEXT_PUBLIC_SOROBAN_RPC_URL=https://soroban-testnet.stellar.org

# Contract Addresses
NEXT_PUBLIC_GOVERNANCE_CONTRACT_ID=CAXXX...
NEXT_PUBLIC_TOKEN_CONTRACT_ID=CBXXX...

# IPFS Configuration
NEXT_PUBLIC_IPFS_GATEWAY=https://ipfs.io/ipfs/
IPFS_API_KEY=your_api_key_here

# Optional: Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
```

**Step 4: Run Development Server**
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

**Step 5: Build for Production**
```bash
npm run build
npm start
```

**Troubleshooting:**
- If you encounter CORS issues, ensure your RPC URL is correct
- For wallet connection issues, check that Freighter is installed and unlocked
- Clear browser cache if you see stale data after contract updates


## 📂 Project Structure

```
vora/
├── contracts/                 # Soroban smart contracts
│   ├── governance/           # Main governance logic
│   ├── treasury/             # Treasury management
│   ├── token/                # Governance token (optional)
│   └── tests/                # Contract tests
├── frontend/                 # Next.js application
│   ├── app/                  # App router pages
│   ├── components/           # React components
│   ├── hooks/                # Custom React hooks
│   ├── lib/                  # Utilities and helpers
│   ├── public/               # Static assets
│   └── styles/               # Global styles
├── docs/                     # Documentation
│   ├── architecture.md       # System design
│   ├── api-reference.md      # API documentation
│   └── guides/               # User guides
├── scripts/                  # Deployment and utility scripts
└── README.md                 # This file
```

### Contracts Directory

**Structure:**
```
contracts/
├── governance/
│   ├── src/
│   │   ├── lib.rs           # Main contract entry point
│   │   ├── types.rs         # Data structures
│   │   ├── storage.rs       # State management
│   │   ├── voting.rs        # Voting logic
│   │   ├── proposals.rs     # Proposal management
│   │   └── delegation.rs    # Delegation system
│   ├── Cargo.toml           # Dependencies
│   └── README.md            # Contract documentation
├── treasury/
│   ├── src/
│   │   ├── lib.rs           # Treasury contract
│   │   └── execution.rs     # Proposal execution
│   └── Cargo.toml
└── tests/
    ├── integration_tests.rs  # End-to-end tests
    └── unit_tests.rs         # Unit tests
```

**Key Files:**
- `lib.rs`: Contract interface and public functions
- `types.rs`: Proposal, Vote, and Config structs
- `storage.rs`: Persistent storage helpers
- `voting.rs`: Vote casting and tallying logic
- `proposals.rs`: Proposal lifecycle management

### Frontend Directory

**Structure:**
```
frontend/
├── app/
│   ├── layout.tsx           # Root layout
│   ├── page.tsx             # Home page
│   ├── proposals/
│   │   ├── page.tsx         # Proposals list
│   │   ├── [id]/
│   │   │   └── page.tsx     # Proposal detail
│   │   └── create/
│   │       └── page.tsx     # Create proposal
│   ├── delegates/
│   │   └── page.tsx         # Delegate directory
│   └── profile/
│       └── page.tsx         # User profile
├── components/
│   ├── ui/                  # Shadcn components
│   ├── proposals/
│   │   ├── ProposalCard.tsx
│   │   ├── VoteButton.tsx
│   │   └── ProposalForm.tsx
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   └── Footer.tsx
│   └── wallet/
│       └── WalletConnect.tsx
├── hooks/
│   ├── useProposals.ts      # Proposal queries
│   ├── useVoting.ts         # Voting mutations
│   ├── useWallet.ts         # Wallet connection
│   └── useDelegation.ts     # Delegation logic
├── lib/
│   ├── stellar.ts           # Stellar SDK setup
│   ├── contracts.ts         # Contract interaction
│   ├── ipfs.ts              # IPFS helpers
│   └── utils.ts             # General utilities
└── styles/
    └── globals.css          # Global styles
```

**Component Organization:**
- `ui/`: Reusable UI primitives (buttons, cards, modals)
- `proposals/`: Proposal-specific components
- `layout/`: Page layout components
- `wallet/`: Wallet integration components

### Docs Directory

**Structure:**
```
docs/
├── architecture.md          # System architecture
├── api-reference.md         # API documentation
├── guides/
│   ├── getting-started.md   # Quick start guide
│   ├── creating-proposals.md
│   ├── voting-guide.md
│   └── delegation-guide.md
├── contracts/
│   ├── governance.md        # Governance contract spec
│   └── treasury.md          # Treasury contract spec
└── deployment/
    ├── testnet.md           # Testnet deployment
    └── mainnet.md           # Mainnet deployment
```

## 📜 Smart Contracts

### Governance Contract

The governance contract is the core of Vora, managing proposals, voting, and execution.

**Key Functions:**

**Proposal Management:**
```rust
// Create a new proposal
pub fn create_proposal(
    env: Env,
    proposer: Address,
    title: String,
    description: String,
    ipfs_hash: String,
    voting_period: u64,
    execution_data: Option<Bytes>
) -> u64;

// Get proposal details
pub fn get_proposal(env: Env, proposal_id: u64) -> Proposal;

// List all proposals
pub fn list_proposals(
    env: Env,
    status: Option<ProposalStatus>,
    offset: u32,
    limit: u32
) -> Vec<Proposal>;
```

**Voting Functions:**
```rust
// Cast a vote
pub fn vote(
    env: Env,
    voter: Address,
    proposal_id: u64,
    support: bool,
    reason: Option<String>
) -> Result<(), Error>;

// Get vote details
pub fn get_vote(
    env: Env,
    proposal_id: u64,
    voter: Address
) -> Option<Vote>;

// Get voting results
pub fn get_results(env: Env, proposal_id: u64) -> VoteResults;
```

**Delegation Functions:**
```rust
// Delegate voting power
pub fn delegate(
    env: Env,
    delegator: Address,
    delegatee: Address
) -> Result<(), Error>;

// Revoke delegation
pub fn revoke_delegation(env: Env, delegator: Address) -> Result<(), Error>;

// Get delegation info
pub fn get_delegation(env: Env, delegator: Address) -> Option<Address>;
```

**Execution Functions:**
```rust
// Execute a passed proposal
pub fn execute(env: Env, proposal_id: u64) -> Result<(), Error>;

// Queue proposal for execution (with timelock)
pub fn queue(env: Env, proposal_id: u64) -> Result<(), Error>;

// Cancel a proposal
pub fn cancel(env: Env, proposal_id: u64) -> Result<(), Error>;
```

**Data Structures:**
```rust
pub struct Proposal {
    pub id: u64,
    pub proposer: Address,
    pub title: String,
    pub ipfs_hash: String,
    pub snapshot_ledger: u32,
    pub start_time: u64,
    pub end_time: u64,
    pub for_votes: i128,
    pub against_votes: i128,
    pub abstain_votes: i128,
    pub status: ProposalStatus,
    pub execution_data: Option<Bytes>,
}

pub enum ProposalStatus {
    Pending,
    Active,
    Passed,
    Failed,
    Executed,
    Cancelled,
}

pub struct Vote {
    pub voter: Address,
    pub support: bool,
    pub weight: i128,
    pub reason: Option<String>,
    pub timestamp: u64,
}
```

**Events:**
```rust
// Emitted when a proposal is created
ProposalCreated {
    proposal_id: u64,
    proposer: Address,
    start_time: u64,
    end_time: u64,
}

// Emitted when a vote is cast
VoteCast {
    proposal_id: u64,
    voter: Address,
    support: bool,
    weight: i128,
}

// Emitted when a proposal is executed
ProposalExecuted {
    proposal_id: u64,
    executor: Address,
}
```

### Treasury Contract

The treasury contract manages community funds and executes approved proposals.

**Key Functions:**

**Treasury Management:**
```rust
// Deposit funds
pub fn deposit(env: Env, from: Address, amount: i128) -> Result<(), Error>;

// Get treasury balance
pub fn get_balance(env: Env) -> i128;

// Transfer funds (only via governance)
pub fn transfer(
    env: Env,
    to: Address,
    amount: i128,
    proposal_id: u64
) -> Result<(), Error>;
```

**Proposal Execution:**
```rust
// Execute treasury action
pub fn execute_proposal(
    env: Env,
    proposal_id: u64,
    actions: Vec<TreasuryAction>
) -> Result<(), Error>;

pub enum TreasuryAction {
    Transfer { to: Address, amount: i128 },
    ContractCall { contract: Address, function: String, args: Vec<Val> },
    UpdateConfig { key: String, value: Val },
}
```

**Access Control:**
```rust
// Set governance contract address
pub fn set_governance(env: Env, admin: Address, governance: Address);

// Only governance contract can execute actions
fn require_governance(env: &Env, caller: Address) -> Result<(), Error>;
```

### Testing

**Unit Tests:**
```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_create_proposal() {
        let env = Env::default();
        let contract_id = env.register_contract(None, GovernanceContract);
        let client = GovernanceContractClient::new(&env, &contract_id);
        
        let proposal_id = client.create_proposal(
            &proposer,
            &String::from_str(&env, "Test Proposal"),
            &String::from_str(&env, "Description"),
            &String::from_str(&env, "QmHash"),
            &7,
            &None
        );
        
        assert_eq!(proposal_id, 1);
    }

    #[test]
    fn test_voting() {
        // Test voting logic
    }

    #[test]
    fn test_delegation() {
        // Test delegation
    }
}
```

**Integration Tests:**
```bash
# Run all tests
cargo test

# Run specific test
cargo test test_create_proposal

# Run with output
cargo test -- --nocapture

# Run tests with coverage
cargo tarpaulin --out Html
```

**Test Coverage Goals:**
- Unit tests: >90% coverage
- Integration tests: All critical paths
- Edge cases: Overflow, underflow, access control
- Gas optimization: Benchmark critical functions

## 🎨 Frontend Application

### App Router

Vora uses Next.js 14 App Router for optimal performance and developer experience.

**Route Structure:**
```
app/
├── (marketing)/
│   ├── layout.tsx           # Marketing layout
│   └── page.tsx             # Landing page
├── (dashboard)/
│   ├── layout.tsx           # Dashboard layout
│   ├── proposals/
│   │   ├── page.tsx         # List view
│   │   ├── [id]/
│   │   │   ├── page.tsx     # Detail view
│   │   │   └── loading.tsx  # Loading state
│   │   └── create/
│   │       └── page.tsx     # Create form
│   ├── delegates/
│   │   ├── page.tsx         # Delegate directory
│   │   └── [address]/
│   │       └── page.tsx     # Delegate profile
│   └── profile/
│       └── page.tsx         # User profile
└── api/
    ├── proposals/
    │   └── route.ts         # Proposals API
    └── ipfs/
        └── route.ts         # IPFS upload
```

**Layout Features:**
- Nested layouts for different sections
- Shared components (header, sidebar)
- Loading and error states
- Metadata for SEO

**Example Page:**
```typescript
// app/proposals/[id]/page.tsx
import { ProposalDetail } from '@/components/proposals/ProposalDetail';
import { getProposal } from '@/lib/contracts';

export default async function ProposalPage({ 
  params 
}: { 
  params: { id: string } 
}) {
  const proposal = await getProposal(params.id);
  
  return <ProposalDetail proposal={proposal} />;
}

export async function generateMetadata({ params }: { params: { id: string } }) {
  const proposal = await getProposal(params.id);
  
  return {
    title: `${proposal.title} | Vora Governance`,
    description: proposal.description.slice(0, 160),
  };
}
```

### Components

**Component Architecture:**
- Atomic design principles
- Composition over inheritance
- Server and client components
- Accessibility first

**Key Components:**

**ProposalCard:**
```typescript
interface ProposalCardProps {
  proposal: Proposal;
  showVoteButton?: boolean;
  compact?: boolean;
}

export function ProposalCard({ 
  proposal, 
  showVoteButton = true,
  compact = false 
}: ProposalCardProps) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{proposal.title}</CardTitle>
        <ProposalStatus status={proposal.status} />
      </CardHeader>
      <CardContent>
        <ProposalProgress proposal={proposal} />
        {showVoteButton && <VoteButton proposalId={proposal.id} />}
      </CardContent>
    </Card>
  );
}
```

**VoteButton:**
```typescript
export function VoteButton({ proposalId }: { proposalId: string }) {
  const { mutate: vote, isPending } = useVote();
  const { address } = useWallet();
  
  const handleVote = (support: boolean) => {
    vote({ proposalId, support, voter: address });
  };
  
  return (
    <div className="flex gap-2">
      <Button 
        onClick={() => handleVote(true)}
        disabled={isPending}
      >
        Vote For
      </Button>
      <Button 
        onClick={() => handleVote(false)}
        variant="outline"
        disabled={isPending}
      >
        Vote Against
      </Button>
    </div>
  );
}
```

### Hooks

**Custom Hooks for Contract Interaction:**

**useProposals:**
```typescript
export function useProposals(status?: ProposalStatus) {
  return useQuery({
    queryKey: ['proposals', status],
    queryFn: async () => {
      const contract = getGovernanceContract();
      return await contract.listProposals({ status });
    },
    staleTime: 30000,
  });
}
```

**useVote:**
```typescript
export function useVote() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async ({ 
      proposalId, 
      support, 
      voter 
    }: VoteParams) => {
      const contract = getGovernanceContract();
      return await contract.vote({
        voter,
        proposal_id: proposalId,
        support,
      });
    },
    onSuccess: (_, variables) => {
      queryClient.invalidateQueries({ 
        queryKey: ['proposal', variables.proposalId] 
      });
    },
  });
}
```

**useWallet:**
```typescript
export function useWallet() {
  const [address, setAddress] = useState<string | null>(null);
  const [isConnected, setIsConnected] = useState(false);
  
  const connect = async (walletType: 'freighter' | 'albedo') => {
    if (walletType === 'freighter') {
      const { address } = await window.freighter.getAddress();
      setAddress(address);
      setIsConnected(true);
    }
  };
  
  const disconnect = () => {
    setAddress(null);
    setIsConnected(false);
  };
  
  return { address, isConnected, connect, disconnect };
}
```

### Utilities

**Contract Helpers:**
```typescript
// lib/contracts.ts
import { Contract, SorobanRpc } from '@stellar/stellar-sdk';

export function getGovernanceContract() {
  const rpc = new SorobanRpc.Server(process.env.NEXT_PUBLIC_SOROBAN_RPC_URL);
  const contractId = process.env.NEXT_PUBLIC_GOVERNANCE_CONTRACT_ID;
  
  return new Contract(contractId);
}

export async function simulateTransaction(xdr: string) {
  const rpc = new SorobanRpc.Server(process.env.NEXT_PUBLIC_SOROBAN_RPC_URL);
  return await rpc.simulateTransaction(xdr);
}
```

**IPFS Helpers:**
```typescript
// lib/ipfs.ts
export async function uploadToIPFS(content: string): Promise<string> {
  const response = await fetch('/api/ipfs', {
    method: 'POST',
    body: JSON.stringify({ content }),
  });
  
  const { hash } = await response.json();
  return hash;
}

export async function fetchFromIPFS(hash: string): Promise<string> {
  const response = await fetch(`${process.env.NEXT_PUBLIC_IPFS_GATEWAY}${hash}`);
  return await response.text();
}
```

## 🧪 Testing

**Frontend Testing Stack:**
- **Jest:** Unit testing framework
- **React Testing Library:** Component testing
- **Playwright:** End-to-end testing
- **MSW:** API mocking

**Unit Tests:**
```typescript
// __tests__/components/ProposalCard.test.tsx
import { render, screen } from '@testing-library/react';
import { ProposalCard } from '@/components/proposals/ProposalCard';

describe('ProposalCard', () => {
  it('renders proposal title', () => {
    const proposal = {
      id: '1',
      title: 'Test Proposal',
      status: 'active',
    };
    
    render(<ProposalCard proposal={proposal} />);
    expect(screen.getByText('Test Proposal')).toBeInTheDocument();
  });
});
```

**Integration Tests:**
```typescript
// __tests__/integration/voting.test.tsx
import { test, expect } from '@playwright/test';

test('user can vote on proposal', async ({ page }) => {
  await page.goto('/proposals/1');
  await page.click('button:has-text("Vote For")');
  await expect(page.locator('.vote-success')).toBeVisible();
});
```

**Run Tests:**
```bash
# Unit tests
npm test

# Watch mode
npm test -- --watch

# Coverage
npm test -- --coverage

# E2E tests
npm run test:e2e

# E2E with UI
npm run test:e2e:ui
```

## 🚀 Deployment

### Testnet Deployment

**Prerequisites:**
- Testnet account with XLM
- Environment variables configured
- Contracts built and tested

**Deploy Contracts:**
```bash
# Deploy governance contract
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/vora_governance.wasm \
  --source deployer \
  --network testnet

# Deploy treasury contract
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/vora_treasury.wasm \
  --source deployer \
  --network testnet

# Initialize contracts
./scripts/initialize-testnet.sh
```

**Deploy Frontend:**
```bash
# Build for production
npm run build

# Deploy to Vercel
vercel --prod

# Or deploy to Netlify
netlify deploy --prod
```

**Verify Deployment:**
```bash
# Test contract interaction
stellar contract invoke \
  --id $GOVERNANCE_CONTRACT_ID \
  --source deployer \
  --network testnet \
  -- \
  get_config

# Check frontend
curl https://your-app.vercel.app/api/health
```

### Mainnet Deployment

**Pre-Deployment Checklist:**
- [ ] All tests passing
- [ ] Security audit completed
- [ ] Gas optimization verified
- [ ] Frontend tested on testnet
- [ ] Documentation updated
- [ ] Monitoring setup
- [ ] Incident response plan ready

**Deploy Contracts:**
```bash
# Switch to mainnet
stellar network add \
  --global mainnet \
  --rpc-url https://soroban-rpc.mainnet.stellar.org:443 \
  --network-passphrase "Public Global Stellar Network ; September 2015"

# Deploy with production keys
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/vora_governance.wasm \
  --source production-deployer \
  --network mainnet

# Initialize with production parameters
./scripts/initialize-mainnet.sh
```

**Post-Deployment:**
- Monitor contract events
- Set up alerts for unusual activity
- Verify all integrations
- Announce to community

## ⚙️ Configuration

**Contract Configuration:**
```rust
pub struct Config {
    pub admin: Address,
    pub token_address: Address,
    pub quorum_percentage: u32,      // e.g., 20 = 20%
    pub passing_threshold: u32,      // e.g., 51 = 51%
    pub min_voting_period: u64,      // Minimum days
    pub max_voting_period: u64,      // Maximum days
    pub proposal_threshold: i128,    // Min tokens to propose
    pub timelock_delay: u64,         // Execution delay in seconds
    pub quadratic_voting: bool,      // Enable quadratic voting
}
```

**Update Configuration:**
```bash
stellar contract invoke \
  --id $GOVERNANCE_CONTRACT_ID \
  --source admin \
  --network mainnet \
  -- \
  update_config \
  --quorum_percentage 25 \
  --passing_threshold 60
```

**Frontend Configuration:**
```typescript
// config/governance.ts
export const governanceConfig = {
  network: process.env.NEXT_PUBLIC_STELLAR_NETWORK,
  contractId: process.env.NEXT_PUBLIC_GOVERNANCE_CONTRACT_ID,
  tokenContractId: process.env.NEXT_PUBLIC_TOKEN_CONTRACT_ID,
  
  // UI Configuration
  proposalsPerPage: 20,
  votingPowerDecimals: 7,
  refreshInterval: 30000, // 30 seconds
  
  // Feature Flags
  features: {
    delegation: true,
    quadraticVoting: false,
    proposalComments: true,
  },
};
```

## 📚 API Reference

**REST API Endpoints:**

**GET /api/proposals**
```typescript
// Query parameters
{
  status?: 'active' | 'passed' | 'failed' | 'executed',
  page?: number,
  limit?: number,
  sort?: 'created' | 'ending' | 'votes'
}

// Response
{
  proposals: Proposal[],
  total: number,
  page: number,
  hasMore: boolean
}
```

**GET /api/proposals/:id**
```typescript
// Response
{
  proposal: Proposal,
  votes: Vote[],
  results: VoteResults
}
```

**POST /api/proposals**
```typescript
// Request body
{
  title: string,
  description: string,
  votingPeriod: number,
  executionData?: string
}

// Response
{
  proposalId: string,
  transactionHash: string
}
```

**POST /api/vote**
```typescript
// Request body
{
  proposalId: string,
  support: boolean,
  reason?: string
}

// Response
{
  success: boolean,
  transactionHash: string
}
```

**Contract RPC Methods:**

Full contract interface documentation available at `/docs/api-reference.md`

## 🤝 Contributing

We welcome contributions from the community!

### Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what is best for the community
- Show empathy towards other community members

**Unacceptable Behavior:**
- Harassment or discriminatory language
- Trolling or insulting comments
- Publishing others' private information
- Other conduct which could reasonably be considered inappropriate

### Pull Requests

**Before Submitting:**
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass (`cargo test && npm test`)
6. Update documentation
7. Commit your changes (`git commit -m 'Add amazing feature'`)
8. Push to the branch (`git push origin feature/amazing-feature`)
9. Open a Pull Request

**PR Guidelines:**
- Clear description of changes
- Reference related issues
- Include screenshots for UI changes
- Ensure CI passes
- Request review from maintainers

**Code Style:**
- Rust: Follow `rustfmt` formatting
- TypeScript: Follow project ESLint rules
- Write clear commit messages
- Add comments for complex logic

**Review Process:**
1. Automated checks run (tests, linting, build)
2. Maintainer review (usually within 48 hours)
3. Address feedback
4. Approval and merge

## 🗺 Roadmap

### Phase 1: Foundation (Q1 2024) ✅

- [x] Core governance contract
- [x] Token-weighted voting
- [x] Balance snapshots
- [x] Basic frontend
- [x] Wallet integration
- [x] Testnet deployment

### Phase 2: Expansion (Q2-Q3 2024)

- [ ] Delegation system
- [ ] Quadratic voting
- [ ] Treasury integration
- [ ] IPFS integration
- [ ] Advanced analytics
- [ ] Mobile responsive design
- [ ] Multi-language support
- [ ] Security audit

### Phase 3: Maturation (Q4 2024 - Q1 2025)

- [ ] Mainnet launch
- [ ] Plugin system for custom governance modules
- [ ] Cross-chain governance (via bridges)
- [ ] AI-powered proposal analysis
- [ ] Reputation system
- [ ] Governance templates
- [ ] DAO-to-DAO integrations
- [ ] Advanced execution strategies

**Future Considerations:**
- Layer 2 scaling solutions
- Privacy-preserving voting (zk-SNARKs)
- Prediction markets for proposals
- Automated proposal generation
- Governance token staking

## ❓ FAQ

### What is Vora?

Vora is a decentralized governance protocol built on Stellar's Soroban smart contract platform. It enables communities to make collective decisions through token-weighted voting, where voting power is proportional to token holdings.

### How does token-weighted voting work?

In token-weighted voting, each token you hold represents one vote. If you hold 100 tokens, you have 100 votes. This system:
- Prevents Sybil attacks (creating multiple fake accounts)
- Aligns voting power with economic stake
- Ensures those most invested have proportional influence

Vora also supports quadratic voting to reduce whale dominance.

### What wallets are supported?

Currently supported wallets:
- **Freighter** (recommended): Browser extension wallet
- **Albedo**: Web-based wallet

Coming soon:
- Ledger hardware wallet support
- WalletConnect integration
- Mobile wallet support

### Is this audited?

**Testnet version:** Not yet audited (use at your own risk)

**Mainnet version:** Will undergo comprehensive security audit before launch

We recommend:
- Only use testnet for experimentation
- Review the code yourself
- Start with small amounts
- Report any security concerns to security@vora.io

### How much does it cost to vote?

Voting on Stellar is extremely affordable:
- Transaction fee: ~0.00001 XLM (less than $0.001)
- No gas wars or congestion
- Predictable costs

### Can I change my vote?

Yes! You can change your vote anytime during the active voting period. Your most recent vote is the one that counts.

### What happens if a proposal doesn't reach quorum?

If a proposal doesn't reach the required quorum (minimum participation threshold), it fails automatically when the voting period ends. The proposal status changes to "Failed" and cannot be executed.

### How do I create a proposal?

1. Connect your wallet
2. Ensure you meet the proposal threshold (minimum tokens required)
3. Navigate to "Create Proposal"
4. Fill in title, description, and parameters
5. Submit transaction
6. Share with community for voting

### Can I delegate my voting power?

Yes! Vora includes a delegation system where you can:
- Delegate to any address
- Retain full custody of your tokens
- Revoke delegation anytime
- Override delegate votes on specific proposals

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 Vora

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```


