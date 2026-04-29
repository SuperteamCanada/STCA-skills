---
name: solana-compress
description: Launch ZK compressed tokens on Solana using Light Protocol for 99% cost reduction. Triggers on "compressed tokens", "ZK compression", "Light Protocol", "cheap airdrop", "mass token distribution", "compressed mint".
---

# ZK Compressed Token Launcher

Build with [ZK Compression](https://www.zkcompression.com/) on Solana using [Light Protocol](https://github.com/Lightprotocol/light-protocol) to reduce on-chain storage costs by 99%. The Light Token SDK drops Solana Associated Token Account fees to **$0.001** for all SPL tokens, with Token-2022 extension support.

## Why this matters now

Light Token SDK is live on mainnet. Any operation that creates many token accounts — airdrops, loyalty programs, gaming assets, agent micropayments — goes from economically impossible to trivially cheap. Token-2022 extensions (transfer hooks, permanent delegation, transfer fees) now work with compression.

## When to use this skill

- "Airdrop tokens to 100k wallets cheaply"
- "Compressed token mint"
- "Reduce token account costs"
- "ZK compression on Solana"
- "Mass NFT distribution"
- "Gaming assets at scale"
- Any project creating many token accounts

## Core concept

Standard Solana token accounts cost ~$0.002 each in rent. At scale:
- 10k accounts = ~$20
- 100k accounts = ~$200
- 1M accounts = ~$2,000

With ZK Compression:
- 10k accounts = ~$0.20
- 100k accounts = ~$2
- 1M accounts = ~$20

The compression works by storing account data in a Merkle tree and posting only the root hash on-chain. ZK proofs verify state transitions without storing full account data.

## Build mode workflow

### Step 1 — Define the use case

Ask the founder:
1. **What tokens?** (fungible SPL, Token-2022 with extensions, NFTs/cNFTs)
2. **Scale?** (hundreds, thousands, millions of accounts)
3. **Token-2022 extensions needed?** (transfer hooks, transfer fees, permanent delegation, confidential transfers)
4. **Distribution method?** (airdrop, claim page, in-app earning, game rewards)
5. **Composability needs?** (do compressed tokens need to interact with existing DeFi protocols?)

### Step 2 — Scaffold the project

```
solana-compress/
├── src/
│   ├── token/
│   │   ├── create.ts          # Create compressed token mint
│   │   ├── mint.ts            # Mint compressed tokens
│   │   └── transfer.ts        # Transfer compressed tokens
│   ├── airdrop/
│   │   ├── distribute.ts      # Batch distribution script
│   │   └── merkle.ts          # Merkle tree management
│   ├── utils/
│   │   └── cost-calculator.ts # Compare compressed vs uncompressed costs
│   └── index.ts
├── .env.example
└── package.json
```

### Step 3 — Set up Light Protocol SDK

```bash
npm install @lightprotocol/stateless.js @lightprotocol/compressed-token @solana/web3.js
```

**Dependencies:**
- `@lightprotocol/stateless.js` — Core RPC, proof generation, compression primitives
- `@lightprotocol/compressed-token` — Compressed token operations (mint, transfer)
- A compression-compatible RPC — Helius has native support; alternatively use Light's own indexer + prover

**RPC setup** — compressed tokens require three endpoints (Solana RPC, Photon Indexer, Light Prover):
```typescript
import { Rpc, createRpc } from "@lightprotocol/stateless.js";

// Helius (single URL, routes internally)
const rpc: Rpc = createRpc(process.env.HELIUS_RPC_URL);

// Or explicit endpoints (devnet)
const rpc: Rpc = createRpc(
  "https://api.devnet.solana.com",
  "https://indexer.devnet.lightprotocol.com",
  "https://prover.devnet.lightprotocol.com",
  { commitment: "confirmed" },
);
```

### Step 4 — Implement compressed token operations

**Create a compressed token mint:**
```typescript
import { createMintInterface } from "@lightprotocol/compressed-token";
import { Keypair } from "@solana/web3.js";

const mintSigner = Keypair.generate();
const mintAuthority = Keypair.generate();

const { mint, transactionSignature } = await createMintInterface(
  rpc,
  payer,                    // Signer — fee payer
  mintAuthority.publicKey,  // mint authority
  null,                     // freeze authority (optional)
  9,                        // decimals
  mintSigner,               // mint keypair
);
await rpc.confirmTransaction(transactionSignature, "confirmed");
```

**Mint compressed tokens (also used for batch airdrops):**
```typescript
import { mintToCompressed } from "@lightprotocol/compressed-token";
import { PublicKey } from "@solana/web3.js";

// Supports multiple recipients in a single transaction
const recipients = [
  { recipient: new PublicKey("..."), amount: 1_000_000_000n },
  { recipient: new PublicKey("..."), amount: 5_000_000_000n },
];

const txId = await mintToCompressed(
  rpc,
  payer,         // Signer — fee payer
  mint,          // PublicKey — the compressed mint
  mintAuthority, // Signer — mint authority
  recipients,    // Array<{ recipient: PublicKey; amount: bigint }>
);
await rpc.confirmTransaction(txId, "confirmed");
```

**Transfer compressed tokens:**
```typescript
import { transfer } from "@lightprotocol/compressed-token";

const txId = await transfer(
  rpc,
  payer,      // Signer — fee payer
  mint,       // PublicKey — token mint
  250n,       // amount
  owner,      // Signer — token owner
  recipient,  // PublicKey — destination
  { commitment: "confirmed" },
);
await rpc.confirmTransaction(txId, "confirmed");
```

### Step 5 — Cost comparison

Before deploying, run the cost calculator to show the founder the savings:

| Operation | Standard | Compressed | Savings |
|-----------|----------|------------|---------|
| 1 token account | ~$0.002 | ~$0.00002 | 99% |
| 10k airdrop | ~$20 | ~$0.20 | 99% |
| 100k airdrop | ~$200 | ~$2 | 99% |
| 1M accounts | ~$2,000 | ~$20 | 99% |

### Step 6 — Token-2022 extensions (if needed)

Compressed tokens support Token-2022 extensions:
- **Transfer hooks** — Execute custom logic on every transfer (royalties, compliance checks)
- **Transfer fees** — Automatic fee collection on transfers
- **Permanent delegation** — Allow a delegate to transfer tokens at any time (escrow, vesting)
- **Confidential transfers** — Hide transfer amounts using ZK proofs

## Hard rules

1. **Use a compression-compatible RPC.** Not all RPCs support ZK Compression. Helius has native support. Check provider compatibility before starting.
2. **Understand the composability tradeoff.** Compressed tokens can't directly interact with standard DeFi protocols (AMMs, lending) that expect regular token accounts. Decompress first if needed.
3. **Merkle tree sizing matters.** Choose tree depth based on expected account count. Too small = runs out of leaves. Too large = higher creation cost.
4. **Test decompression flows.** Users may need to decompress tokens to use them in existing DeFi. Make sure this path works smoothly.
5. **Monitor tree utilization.** When a Merkle tree fills up, you need a new one. Build monitoring for tree capacity.

## Reference links

- [ZK Compression documentation](https://www.zkcompression.com/home)
- [Light Protocol GitHub](https://github.com/Lightprotocol/light-protocol)
- [Light Token SDK examples](https://github.com/Lightprotocol/examples-light-token)
- [Helius ZK Compression support](https://www.helius.dev/docs)
- [Token-2022 extensions](https://spl.solana.com/token-2022)
