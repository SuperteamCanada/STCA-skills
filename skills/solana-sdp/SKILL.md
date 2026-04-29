---
name: solana-sdp
description: Build on the Solana Developer Platform (SDP) for enterprise payments, tokenization, and trading. Triggers on "SDP", "Solana Developer Platform", "tokenized deposits", "stablecoin issuance", "enterprise Solana", "fiat on-ramp".
---

# Solana Developer Platform (SDP) Starter

Build enterprise-grade payment, tokenization, and trading products using the [Solana Developer Platform](https://solana.com/solutions/sdp) — the Solana Foundation's API-first platform that aggregates 20+ infrastructure providers into a single unified interface.

## Why this matters now

SDP launched March 24, 2026 with Mastercard, Worldpay, and Western Union as early adopters. It's designed to work out-of-the-box with AI coding platforms like Claude Code and OpenAI Codex. Three modules cover the core fintech use cases on Solana:

> **⚠️ Early-stage platform.** SDP's public API documentation is limited as of April 2026. The workflow below is a guide based on announced capabilities — verify endpoints and SDK availability against [platform.solana.com](https://platform.solana.com) before building. Some steps may require waitlist access or partner onboarding.

| Module | What it does | Use cases |
|--------|-------------|-----------|
| **Issuance** | Create and manage tokenized deposits, stablecoins, and RWAs | Stablecoin launch, tokenized treasury, RWA platform |
| **Payments** | Fiat/stablecoin orchestration with on/off-ramp partners | Payment gateway, remittances, payroll, merchant checkout |
| **Trading** | Swaps, vaults, and on-chain FX | DEX frontend, portfolio management, FX conversion |

## When to use this skill

- "Build a payment product on Solana"
- "I need fiat on/off-ramp integration"
- "Create a stablecoin issuance flow"
- "Enterprise Solana integration"
- "Tokenize real-world assets"
- Any fintech/payments product targeting Solana

## Build mode workflow

### Step 1 — Define the use case

Ask the founder:
1. **Which SDP module?** (Issuance, Payments, Trading, or combination)
2. **What's the end-user flow?** (merchant checkout, P2P transfer, token creation, swap)
3. **Compliance requirements?** (KYC/AML, geographic restrictions, GENIUS Act compliance)
4. **Target chains?** (Solana mainnet, with potential multi-chain via SDP's unified API)
5. **Volume expectations?** (affects infrastructure partner selection and rate negotiation)

### Step 2 — Scaffold the project

```
solana-sdp-app/
├── src/
│   ├── sdp/
│   │   ├── client.ts         # SDP API client configuration
│   │   ├── issuance.ts       # Token issuance flows
│   │   ├── payments.ts       # Payment orchestration
│   │   └── trading.ts        # Swap/FX operations
│   ├── compliance/
│   │   └── kyc.ts            # KYC/AML partner integration
│   ├── webhooks/
│   │   └── handler.ts        # SDP event webhooks
│   └── index.ts
├── .env.example              # SDP API keys, partner configs
└── package.json
```

### Step 3 — Connect to SDP sandbox

> **⚠️ No public SDK yet.** As of April 2026, SDP does not publish a standalone npm client or SDK. Access is via REST API with key-based auth from the developer portal. The steps below reflect the announced flow — confirm current availability at [platform.solana.com](https://platform.solana.com).

1. **Register** at [platform.solana.com](https://platform.solana.com) for sandbox API credentials
2. **Configure** the SDP client with sandbox (devnet) environment
3. **Select infrastructure partners** — SDP's unified interface lets you swap between Helius, QuickNode, Alchemy without code changes

Until a published SDK exists, wrap the REST API yourself:

```typescript
// src/sdp/client.ts — minimal SDP API wrapper (adapt to actual endpoints)
const SDP_BASE = process.env.SDP_API_URL ?? "https://api.platform.solana.com/v1";
const SDP_KEY = process.env.SDP_API_KEY;

async function sdpRequest(path: string, body?: unknown) {
  const res = await fetch(`${SDP_BASE}${path}`, {
    method: body ? "POST" : "GET",
    headers: {
      Authorization: `Bearer ${SDP_KEY}`,
      "Content-Type": "application/json",
    },
    body: body ? JSON.stringify(body) : undefined,
  });
  if (!res.ok) throw new Error(`SDP ${path}: ${res.status} ${await res.text()}`);
  return res.json();
}
```

### Step 4 — Implement the module

> **Note:** The examples below describe the intended integration patterns based on SDP's announced module capabilities. No runnable code samples are available from the Solana Foundation yet — treat these as architectural guidance, not copy-paste implementations.

**Issuance — stablecoin creation:**
- Configure token parameters (name, symbol, decimals, supply controls)
- Set up mint/burn authority with compliance partner
- Implement Token-2022 extensions (transfer hooks, permanent delegation, confidential transfers)
- Test issuance flow on sandbox

**Payments — merchant checkout:**
- Set up fiat on-ramp via SDP Payments API
- Build checkout UI that accepts fiat → converts to stablecoin → settles on-chain
- Configure off-ramp for merchant fiat withdrawal
- Handle webhooks for payment status updates

**Trading — swap interface:**
- Connect to SDP Trading API for best-rate aggregation
- Build swap UI with quote preview and slippage controls
- Implement vault management for portfolio products
- Handle on-chain FX conversion

### Step 5 — Compliance integration

- Integrate KYC/AML partner via SDP's compliance module
- Implement geographic restrictions based on regulatory requirements
- Set up transaction monitoring and reporting
- For stablecoin issuance: ensure GENIUS Act compliance for US-targeted products

### Fallback — build without SDP

If SDP sandbox access is unavailable or waitlisted, the same product categories can be built directly:

| SDP Module | Direct Alternative |
|---|---|
| Issuance | `@solana/spl-token` + Token-2022 extensions directly |
| Payments | [Helio / MoonPay Commerce](https://www.hel.io/) or [Sphere](https://spherepay.co/) for on/off-ramp |
| Trading | [Jupiter Swap V2 API](https://developers.jup.ag/docs/api-reference) for swaps |

This loses the "single API" abstraction but lets you ship without waiting on SDP onboarding.

## Hard rules

1. **Always start on sandbox.** SDP provides a full devnet sandbox. Never send real funds during development.
2. **KYC/AML is not optional for payments.** If you're moving fiat or issuing tokens, compliance integration is required before mainnet launch.
3. **Use SDP's unified API, not direct provider APIs.** The whole point of SDP is provider abstraction — switching from Helius to QuickNode should be a config change, not a code change.
4. **Handle webhook idempotency.** SDP may deliver webhooks multiple times. Use idempotency keys for all payment state transitions.
5. **Token-2022 extensions for compliance.** Use transfer hooks and confidential transfers for regulated token issuance — vanilla SPL tokens lack the controls regulators expect.

## Reference links

- [Solana Developer Platform](https://solana.com/solutions/sdp)
- [SDP launch announcement](https://solana.com/news/solana-developer-platform)
- [CoinDesk: Solana Foundation taps Mastercard, Western Union](https://www.coindesk.com/tech/2026/03/24/solana-foundation-taps-mastercard-western-union-worldpay-for-institutional-developer-platform)
- [Token-2022 extensions](https://spl.solana.com/token-2022)
