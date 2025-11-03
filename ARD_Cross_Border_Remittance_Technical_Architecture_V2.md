# ARD Financial Group - Cross-Border Remittance System
## Technical Architecture Document

**Version:** 2.0
**Date:** 2025-11-03
**Prepared for:** ARD Financial Group + Lightspark Review
**System:** Cross-Border Remittance using Lightspark Lightning Network

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [System Overview](#system-overview)
3. [Architecture Components](#architecture-components)
4. [Pre-Funded Lightning Model](#pre-funded-lightning-model)
5. [Transaction Types](#transaction-types)
6. [Integration Points](#integration-points)
7. [Security & Compliance](#security--compliance)

---

## 1. Executive Summary

### Business Context

ARD Financial Group ecosystem includes:
- **ARD APP**: Consumer financial services platform
- **Custody SaaS**: Blockchain wallet and ledger platform (like exchange backend)
- **iDAX Exchange**: Cryptocurrency exchange (idax.exchange) - provides trading API

### New Product: Cross-Border Remittance

A Lightning Network-powered remittance service enabling:
- **Mongolia → International**: Send MNT to 140+ countries
- **International → Mongolia**: Receive money from anywhere to MNT
- **Internal ARD Transfers**: Instant, zero-fee transfers between ARD users
- **Multi-currency support**: 120+ currencies via UMA protocol
- **Settlement time**: < 5 seconds for international, instant for internal
- **Cost**: 85-95% cheaper than traditional remittance

### Key Innovation: Pre-Funded Pool + Immediate Execution

**Outbound transactions** (Mongolia → World):
- **Pre-funded BTC pool** owned by ARD APP (10-20 BTC)
- **Daily batch reconciliation** with iDAX exchange
- **Optimal cost efficiency** while maintaining instant user experience
- **No Lightning node operation required** - fully managed by Lightspark

**Inbound transactions** (World → Mongolia): 🚨 **CRITICAL V2.0 UPDATE**
- **IMMEDIATE iDAX execution** when BTC arrives via Lightning
- **Zero price risk** - BTC sold for MNT instantly
- **Better user experience** - funds available immediately
- **Simple operations** - no reconciliation needed

---

## 2. System Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARD FINANCIAL GROUP ECOSYSTEM                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────┐      ┌────────────┐      ┌────────────┐        │
│  │    ARD     │      │  Custody   │      │    iDAX    │        │
│  │    APP     │◄────▶│    SaaS    │◄────▶│  Exchange  │        │
│  │  (UI/UX)   │      │  (Ledger)  │      │ (Trading)  │        │
│  └────────────┘      └────────────┘      └────────────┘        │
│       │                     │                     │              │
│  OWNS BTC POOL         TRACKS POOL         TRADING API          │
│  Manages funds         Ledger only         Minimal role         │
│                             │                     │              │
│                             ▼                     │              │
│                      ┌─────────────────────────────────┐        │
│                      │   Pre-Funded BTC Pool           │        │
│                      │   (Owned by ARD APP)            │        │
│                      │   - Daily reconciliation        │        │
│                      │   - 10-20 BTC float            │        │
│                      └─────────────────────────────────┘        │
└─────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────┐
│              LIGHTSPARK MANAGED INFRASTRUCTURE                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  • Managed Lightning Nodes (no setup required)             │ │
│  │  • AI-Powered Routing                                      │ │
│  │  • Channel Liquidity Management                            │ │
│  │  • UMA Protocol Support                                    │ │
│  │  • Grid API (140+ countries, 25+ payment networks)         │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │   Global Recipients      │
                    │   • Lightning Network    │
                    │   • UMA addresses        │
                    │   • Bank accounts (ACH)  │
                    │   • 120+ currencies      │
                    └──────────────────────────┘
```

### Component Responsibilities

#### ARD APP (Frontend + Fund Management)
- User interface for sending/receiving money
- **Owns and manages BTC liquidity pool** (10-20 BTC)
- **Manages user KYC/AML data**
- Account management and user data
- Push notifications
- Multi-language support (Mongolian, English, etc.)
- Provides user information to Custody SaaS via API

#### Custody SaaS Platform (Backend Ledger + Blockchain Accounting)
- **Function**: Like an exchange backend - tracks balances and transactions
- User balance ledger (internal accounting)
- Transaction orchestration
- Lightspark API integration
- UMA protocol implementation
- **Immediate iDAX execution** for inbound Lightning (V2.0)
- Mongolia banking integration
- Daily reconciliation with ARD APP for outbound pool usage
- **Does NOT store KYC** - gets from ARD APP via API
- **Does NOT own BTC pool** - only tracks it in ledger

#### iDAX Exchange (Trading API Provider - Minimal Role)
- **Provides**: Trading API for MNT/BTC conversions
- **Charges**: Trading fees (~0.1%)
- **Does NOT**: Manage funds, treasury, or reconciliation
- **Role**: Execution layer only

#### Lightspark (Infrastructure Provider)
- Managed Lightning Network nodes
- International payment routing
- UMA protocol infrastructure
- Grid API for fiat on/off ramps
- Compliance infrastructure (Travel Rule, OFAC)

---

## 3. Architecture Components

### 3.1 Core System Functions

**User Management:**
- KYC verification and status tracking
- UMA address assignment ($user@ard.mn)
- Balance management (MNT and BTC)
- Transaction limits and compliance
- Linked bank accounts

**Transaction Processing:**
- Quote generation with rate locking (60 seconds)
- User approval workflow
- Multi-currency conversion
- Lightning Network routing
- Settlement and confirmation
- Automatic refunds for failed transactions

---

## 4. Pre-Funded Lightning Model

### 4.1 Concept

**For Outbound Transactions** (Mongolia → World):
Instead of settling each transaction between ARD APP and Lightspark in real-time:

1. **Pre-fund** a BTC pool on Lightspark (owned by ARD APP)
2. **Use** this pool for all outbound international transactions
3. **Replenish** daily via batch settlement from ARD APP
4. **Minimize** transaction costs and reconciliation overhead

**For Inbound Transactions** (World → Mongolia): 🚨 **V2.0 CRITICAL UPDATE**
1. **Receive** BTC via Lightning Network
2. **IMMEDIATELY** execute iDAX market sell order
3. **Credit** user account with MNT
4. **Update** ARD APP's BTC pool ledger
5. **NO daily reconciliation needed** - already settled!

### 4.2 BTC Pool Management (Outbound Only)

```
┌─────────────────────────────────────────────────────────────────┐
│                    BTC POOL LIFECYCLE                            │
└─────────────────────────────────────────────────────────────────┘

Day 1 - Initial Setup:
─────────────────────────────────────────────────────────────
  ARD APP                          Lightspark Lightning Node
   │                                      │
   │  1. Transfer 10 BTC (initial pool)  │
   │─────────────────────────────────────▶│
   │                                      │
   │                                      │  Pool: 10 BTC
   │                                      │  Available: 10 BTC
   │                                      │

Day 1 - Operations (Throughout the day):
─────────────────────────────────────────────────────────────
Outbound transactions consume from pool:
  - TX1: -0.001 BTC (Mongolia → USA)
  - TX2: -0.002 BTC (Mongolia → Korea)
  - TX3: -0.0015 BTC (Mongolia → Australia)
  ...
  - TX100: -0.003 BTC

Inbound transactions: IMMEDIATE iDAX execution (no pool impact)
  - RX1: +0.00166 BTC received → INSTANTLY sold for MNT
  - RX2: +0.002 BTC received → INSTANTLY sold for MNT

End of Day 1:
  Pool: 10 BTC
  Available: 7.5 BTC (consumed by outbound)
  Used: 2.5 BTC (outbound only)

Day 2 - Morning Reconciliation (Automated):
─────────────────────────────────────────────────────────────
  ARD APP                          Custody SaaS
   │                                 │
   │  1. Reconciliation report        │
   │◄────────────────────────────────│
   │  "Yesterday consumed: 2.5 BTC"  │
   │  "Net MNT collected: 10M MNT"   │
   │                                 │
   │  2. Settlement transfer          │
   │  Transfer 2.5 BTC to Lightspark │
   │─────────────────────────────────▶ Lightspark
   │                                      │
   │                                      │  Pool: 7.5 → 10 BTC
   │                                      │  (Replenished)

Note: Inbound transactions NOT included in reconciliation
      (already settled via immediate iDAX execution)

Day 2 - Operations continue...
  - Normal operations resume
  - Pool maintained at optimal level
```

### 4.3 Pool Size Calculation

Optimal pool size formula:

```
optimal_size = daily_outbound_volume_btc × safety_factor

Where:
  - daily_outbound_volume_btc: Average daily outbound BTC volume
  - safety_factor: 1.5x (50% buffer)
  - Minimum: 2 BTC
  - Maximum: 50 BTC (adjustable)

Example calculations:
  - Daily volume: 5 BTC → Optimal pool: 7.5 BTC
  - Daily volume: 10 BTC → Optimal pool: 15 BTC
  - Daily volume: 50 BTC → Optimal pool: 50 BTC (capped)
```

### 4.4 Cost Comparison: Real-time vs Pre-funded

```
┌─────────────────────────────────────────────────────────────────┐
│              COST ANALYSIS: REAL-TIME vs PRE-FUNDED             │
└─────────────────────────────────────────────────────────────────┘

Assumptions:
- Daily volume: 1000 outbound transactions
- Average transaction: 0.001 BTC
- Total daily volume: 1 BTC

OPTION A: REAL-TIME SETTLEMENT (NOT RECOMMENDED)
─────────────────────────────────────────────────────────────
1000 transactions × Real-time settlement costs:
  - Network fee per TX: ~$2 × 1000 = $2,000
  - Processing overhead: ~$1 × 1000 = $1,000

  TOTAL DAILY COST: ~$3,000/day
  MONTHLY COST: ~$90,000

OPTION B: PRE-FUNDED POOL (RECOMMENDED) ✅
─────────────────────────────────────────────────────────────
1 batch settlement per day:
  - Network fee: ~$2 × 1 = $2
  - Processing overhead: ~$1 × 1 = $1

  TOTAL DAILY COST: ~$3/day
  MONTHLY COST: ~$90

SAVINGS: $89,910/month (99.9% reduction!) 💰
```

---

## 5. Transaction Types

### 5.1 Type A: Internal ARD Transfers (Zero Fee, Instant)

**Use Case:** ARD user sends MNT to another ARD user within the app

```
┌─────────────────────────────────────────────────────────────────┐
│         INTERNAL ARD TRANSFER (INSTANT, ZERO FEE)               │
└─────────────────────────────────────────────────────────────────┘

Anhaa (ARD User)                     Bat (ARD User)
  Balance: 500,000 MNT                  Balance: 100,000 MNT
       │                                      │
       │  1. Send 50,000 MNT to Bat          │
       │     via ARD app                     │
       ▼                                     │
┌─────────────────────────────────────┐     │
│  CUSTODY SAAS INTERNAL LEDGER       │     │
│                                     │     │
│  Transaction Type: INTERNAL         │     │
│  Processing:                        │     │
│    1. Check Anhaa balance ✓         │     │
│    2. Check Bat exists ✓            │     │
│    3. Atomic ledger update:         │     │
│       - Debit Anhaa: -50,000 MNT    │     │
│       - Credit Bat: +50,000 MNT     │     │
│    4. No external network!          │     │
│    5. No blockchain fees!           │     │
│    6. No conversion needed!         │     │
│                                     │     │
│  Time: < 100ms (database only)      │     │
│  Fee: 0 MNT (internal transfer)     │     │
│  Status: COMPLETED ✓                │     │
└─────────────────────────────────────┘     │
       │                                     │
       │  2. Instant notification            │
       ▼                                     ▼
  Balance: 450,000 MNT            Balance: 150,000 MNT
  (Updated instantly)             (Updated instantly)

┌─────────────────────────────────────────────────────────────────┐
│  ADVANTAGES OF INTERNAL TRANSFERS:                               │
│  ✓ Instant settlement (< 100ms)                                 │
│  ✓ Zero fees (no network costs)                                 │
│  ✓ No blockchain involved                                       │
│  ✓ No currency conversion                                       │
│  ✓ Simple database transaction                                  │
│  ✓ Can batch thousands per second                               │
│  ✓ Minimal compliance overhead (same platform)                  │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Type B: Outbound International (Mongolia → Other Countries)

**Use Case:** ARD user in Mongolia sends money to recipient abroad

```
┌─────────────────────────────────────────────────────────────────┐
│      OUTBOUND INTERNATIONAL TRANSFER (Mongolia → USA)            │
└─────────────────────────────────────────────────────────────────┘

Step 1: User Initiation
────────────────────────────────────────────────────────────
Anhaa (Mongolia)                 ARD APP
  │                                   │
  │  "Send 100,000 MNT to             │
  │   $battulga@usawallet.com"        │
  │───────────────────────────────────▶│
  │                                   │
  │                                   │  Forward to Custody SaaS
  │                                   │
                                      ▼
                          ┌────────────────────────┐
                          │   CUSTODY SAAS         │
                          │  1. Validate UMA       │
                          │     address            │
                          │  2. Get user KYC       │
                          │     from ARD APP API   │
                          │  3. Check limits       │
                          └────────────────────────┘

Step 2: Quote Generation
────────────────────────────────────────────────────────────
                          CUSTODY SAAS
                                │
              ┌─────────────────┼─────────────────┐
              ▼                 ▼                 ▼
        ┌──────────┐      ┌──────────┐     ┌──────────┐
        │ UMA      │      │ iDAX     │     │Lightspark│
        │ Lookup   │      │ Rate     │     │ Grid     │
        │          │      │ Query    │     │ Quote    │
        └──────────┘      └──────────┘     └──────────┘
              │                 │                 │
              │  Battulga OK    │  MNT/BTC rate  │  BTC/USD rate
              │  USD accepted   │  60,000        │  755,000
              │  Travel Rule: Y │  MNT/BTC       │  USD/BTC
              └─────────────────┴─────────────────┘
                                │
                                ▼
                          Quote Generated:
                          - Send: 100,000 MNT
                          - Fee: 1,000 MNT (1%)
                          - Battulga receives: ~$29 USD
                          - Rate: 1 MNT ≈ 0.00029 USD
                          - Valid: 60 seconds

Step 3: User Approval & Execution
────────────────────────────────────────────────────────────
Anhaa                           CUSTODY SAAS
  │                                   │
  │  "Approve"                        │
  │───────────────────────────────────▶│
  │                                   │
  │                              1. Debit Anhaa's
  │                                 MNT balance
  │                                 (101,000 MNT)
  │                                   │
  │                              2. Mark transaction
  │                                 as FUNDING
  │                                   │
                                      ▼

Step 4: BTC Pool Usage (NO iDAX real-time call!)
────────────────────────────────────────────────────────────
                          CUSTODY SAAS
                                │
                                │  Calculate BTC needed:
                                │  100,000 MNT ÷ 60,000 = 0.00166 BTC
                                │
                                ▼
                    ┌────────────────────────┐
                    │  PRE-FUNDED BTC POOL   │
                    │  (Owned by ARD APP)    │
                    │  Current: 10 BTC       │
                    │  Reserve: 0.00166 BTC  │
                    │  Available: 9.99834 BTC│
                    └────────────────────────┘
                                │
                                │  NO iDAX call needed!
                                │  Use pre-funded pool!
                                │
                                ▼

Step 5: Lightning Network Payment
────────────────────────────────────────────────────────────
                          CUSTODY SAAS
                                │
                                │  Create Lightning invoice
                                │  for 0.00166 BTC
                                ▼
                          LIGHTSPARK API
                          payInvoice(0.00166 BTC)
                                │
                                │  Route through Lightning
                                │  Network (3-5 seconds)
                                ▼
                          Lightning Network
                          (Global routing)
                                │
                                │  Payment successful
                                ▼
                    ┌────────────────────────┐
                    │  USA WALLET (VASP)     │
                    │  - Receive 0.00166 BTC │
                    │  - Convert to ~$29 USD │
                    │  - Credit Battulga     │
                    └────────────────────────┘

Step 6: Confirmation & Ledger Update
────────────────────────────────────────────────────────────
CUSTODY SAAS
  │
  │  1. Mark transaction COMPLETED
  │  2. Update internal ledger:
  │     - Track 0.00166 BTC used from pool
  │     - Record for daily reconciliation
  │  3. Send notifications
  │
  ├─────────▶ Anhaa: "Sent successfully!"
  └─────────▶ Battulga: "Received $29 USD"

TOTAL TIME: 5-10 seconds
FEE TO USER: 1,000 MNT (1%)
NETWORK COST TO ARD: ~$0.05 (Lightning fee only)
NO REAL-TIME IDAX SETTLEMENT COST! ✅
```

**Daily Reconciliation (Next Day):**

```
Morning of Day 2:
────────────────────────────────────────────────────────────
CUSTODY SAAS                    ARD APP
      │                               │
      │  Reconciliation Report        │
      │───────────────────────────────▶│
      │                               │
      │  "Yesterday we used:          │
      │   - Total outbound: 5 BTC     │
      │   - From MNT collection:      │
      │     200M MNT collected        │
      │   - Need replenishment:       │
      │     5 BTC"                    │
      │                               │
      │                               │  ARD APP treasury:
      │                               │  1. Transfer 5 BTC to
      │                               │     Lightspark wallet
      │                               │
      │  Confirmation: 5 BTC sent     │
      │◄───────────────────────────────│
      │                               │
      │  Update pool balance          │
      │                               │

COST: One settlement per day instead of 1000+ per day!
SAVINGS: 99.9% reduction in transaction costs!
```

### 5.3 Type C: Inbound International (Other Countries → Mongolia)

🚨 **CRITICAL V2.0 UPDATE: IMMEDIATE iDAX EXECUTION**

**Use Case:** Someone abroad sends money to ARD user in Mongolia

```
┌─────────────────────────────────────────────────────────────────┐
│      INBOUND INTERNATIONAL TRANSFER (USA → Mongolia)             │
│      🚨 V2.0: IMMEDIATE IDAX EXECUTION (NO RECONCILIATION)      │
└─────────────────────────────────────────────────────────────────┘

Step 1: UMA Address Lookup
────────────────────────────────────────────────────────────
Battulga (USA Sender)        USA Bank/Wallet
     │                             │
     │  "Send $100 to              │
     │   $anhaa@ard.mn"            │
     │─────────────────────────────▶│
                                   │
                                   │  LNURLP Lookup:
                                   │  GET https://ard.mn/
                                   │      .well-known/lnurlp/anhaa
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────┐
│  CUSTODY SAAS (UMA Endpoint)                             │
│                                                          │
│  Response:                                               │
│  {                                                       │
│    "callback": "https://api.ard.mn/uma/payreq",         │
│    "minSendable": 1000000,  // millisats                │
│    "maxSendable": 100000000000,                         │
│    "currencies": [                                      │
│      {                                                  │
│        "code": "MNT",                                   │
│        "name": "Mongolian Tugrik",                      │
│        "symbol": "₮",                                   │
│        "decimals": 2,                                   │
│        "multiplier": 850  // millisats per MNT         │
│      },                                                 │
│      { "code": "USD", ... }                            │
│    ],                                                   │
│    "compliance": {                                      │
│      "kycStatus": "verified",                          │
│      "isSubjectToTravelRule": true,                    │
│      "receiverIdentifier": "$anhaa@ard.mn"             │
│    }                                                    │
│  }                                                       │
└──────────────────────────────────────────────────────────┘

Step 2: Payment Request with Compliance
────────────────────────────────────────────────────────────
USA Bank/Wallet                CUSTODY SAAS
     │                              │
     │  POST /uma/payreq            │
     │──────────────────────────────▶│
     │  {                            │
     │    "amount": 100,             │  Process payment request:
     │    "currency": "USD",         │  $100 → ~66,334 MNT
     │    "payerData": {             │  (example rate)
     │      "identifier": "$battulga@usa",
     │      "compliance": {...}      │  1. Validate compliance
     │    }                          │  2. Check Anhaa account
     │  }                            │  3. Generate Lightning
     │                              │     invoice
     │                              │
     │  Lightning Invoice            │
     │◄──────────────────────────────│
     │  {                            │
     │    "pr": "lnbc...",           │
     │    "routes": []               │
     │  }                            │
     │                              │

Step 3: Lightning Payment Execution
────────────────────────────────────────────────────────────
USA Bank/Wallet                LIGHTSPARK
     │                              │
     │  Pay Lightning Invoice        │
     │  (~0.00166 BTC for $100)     │
     │──────────────────────────────▶│
     │                              │
     │                              │  Route through
     │                              │  Lightning Network
     │                              │  (2-3 minutes)
     │                              │
     │                              │  Deliver to ARD's
     │                              │  Lightning wallet
     │                              │
                                    ▼
                          CUSTODY SAAS
                          Lightning Wallet
                                │
                                │  Webhook received:
                                │  Payment successful!
                                │  Received: 0.00166 BTC
                                │
                                ▼

Step 4: 🚨 IMMEDIATE iDAX EXECUTION (V2.0 CRITICAL CHANGE)
────────────────────────────────────────────────────────────
CUSTODY SAAS
     │
     │  🚨 DO NOT WAIT! EXECUTE IMMEDIATELY!
     │
     │  1. BTC received: 0.00166 BTC
     │  2. IMMEDIATELY call iDAX API
     ▼
┌──────────────────────────────────────────────┐
│  iDAX TRADING API                            │
│                                              │
│  POST /api/v1/trade/execute                  │
│  {                                           │
│    "action": "sell",                         │
│    "asset": "BTC",                           │
│    "amount": 0.00166,                        │
│    "for": "MNT",                             │
│    "type": "market"  // Immediate execution  │
│  }                                           │
│                                              │
│  Response (200-500ms):                       │
│  {                                           │
│    "executed": true,                         │
│    "btc_sold": 0.00166,                      │
│    "mnt_received": 66334,                    │
│    "rate": 39,960 MNT/BTC,                   │
│    "fee": 66 MNT (0.1%)                      │
│  }                                           │
└──────────────────────────────────────────────┘
     │
     │  3. Received 66,334 MNT from iDAX
     │  4. Credit Anhaa's account: +66,334 MNT
     │  5. Update ARD APP's BTC pool ledger
     │  6. Transaction COMPLETED ✅
     │
     ▼

Step 5: User Notification
────────────────────────────────────────────────────────────
CUSTODY SAAS                   ARD APP
     │                              │
     │  Push Notification           │
     │──────────────────────────────▶│
     │  "You received 66,334 MNT    │
     │   from $battulga@usa.com"    │
     │                              │
     │                              │  Show in app:
     │                              │  ✓ New balance
     │                              │  ✓ Transaction details
     │                              │
                                    ▼
                               Anhaa sees:
                               "Received 66,334 MNT"
                               "From: Battulga (USA)"

TOTAL TIME: 3-6 seconds (end-to-end)
FEE TO USER: Built into conversion rate (~0.5-1%)
COST TO ARD: 0.1% iDAX fee (~$0.10 on $100)
PRICE RISK: ZERO ✅ (immediate conversion)

🚨 NO DAILY RECONCILIATION NEEDED
   (Already settled immediately!)

WHY IMMEDIATE EXECUTION?
────────────────────────────────────────────────────────────
✅ ZERO price risk (BTC can move 1-5% in 24 hours)
✅ Funds available to user immediately
✅ Simple, clean operations
✅ Better user experience
✅ Lower operational risk

Cost: Only 0.1% iDAX fee (~$0.02 per $20 transaction)
      Much cheaper than 24h BTC price exposure risk!
```

### 5.4 Type D: External Payout (ARD Balance → Bank Account)

**Use Case:** ARD user wants to cash out their balance to a bank account

```
┌─────────────────────────────────────────────────────────────────┐
│    EXTERNAL PAYOUT (ARD Balance → Bank Account)                  │
└─────────────────────────────────────────────────────────────────┘

OPTION 1: Mongolia Bank (Real-Time)
────────────────────────────────────────────────────────────
Anhaa's ARD balance → Mongolia bank account
Amount: 100,000 MNT

Time: Instant to 2 hours (real-time domestic transfer)
Fee: ~500 MNT
Method: Domestic bank transfer via Mongolia banking system

OPTION 2: International Bank (via Lightspark Grid)
────────────────────────────────────────────────────────────
Bold's ARD balance → US bank account
Amount: 100,000 MNT → ~$29 USD

Time: 1-2 business days (ACH)
Fee: ~2,000 MNT (2%)
Method: Lightspark Grid API → ACH/SEPA/PIX/etc.

Process Flow:
────────────────────────────────────────────────────────────
Bold (ARD User)                CUSTODY SAAS
  │                                 │
  │  "Withdraw 100,000 MNT to       │
  │   my US bank account"           │
  │─────────────────────────────────▶│
  │                                 │
  │                              Query rates:
  │                              - MNT/BTC from iDAX
  │                              - BTC/USD from Lightspark
  │                                 │
  │  Quote:                         │
  │◄─────────────────────────────────│
  │  - Send: 100,000 MNT            │
  │  - Receive: ~$29 USD            │
  │  - Fee: 2,000 MNT               │
  │  - Processing: 1-2 business days│
  │                                 │
  │  "Approve"                      │
  │─────────────────────────────────▶│
                                    │
                                    │  1. Debit Bold: 102,000 MNT
                                    │  2. Use pre-funded BTC pool
                                    │  3. Call Lightspark Grid API
                                    │
                                    ▼
                          LIGHTSPARK GRID API
                          createOffRampQuote()
                          executeOffRamp()
                                    │
                                    │  Lightspark handles:
                                    │  - BTC → USD conversion
                                    │  - ACH initiation
                                    │  - Compliance (OFAC)
                                    │  - Delivery to bank
                                    │
                                    ▼
                              US Banking System
                              (1-2 business days)
                                    │
                                    ▼
                              Bold's Bank Account
                              +$29.00 USD

SUPPORTED PAYOUT METHODS:
────────────────────────────────────────────────────────────
Via Lightspark Grid:

✓ ACH (USA) - 1-2 business days
✓ FedNow (USA) - Instant
✓ SEPA (Europe) - 1-2 business days
✓ Faster Payments (UK) - Instant
✓ PIX (Brazil) - Instant
✓ UPI (India) - Instant
✓ SPEI (Mexico) - Instant
✓ PromptPay (Thailand) - Instant
✓ 20+ other domestic payment networks
```

---

## 6. Integration Points

### 6.1 Key APIs

**ARD APP → Custody SaaS:**
- User data API (KYC information)
- Balance queries
- Transaction status updates
- Webhook notifications

**Custody SaaS → Lightspark:**
- Lightning payments (send/receive)
- UMA protocol endpoints
- Grid API (bank payouts)
- Webhook handlers

**Custody SaaS → iDAX:**
- 🚨 **IMMEDIATE trade execution** (inbound BTC → MNT)
- Rate queries (for quotes)
- Daily reconciliation (outbound pool replenishment)

### 6.2 Lightspark Integration

Key integration points:
- **Managed Lightning Node**: No setup required
- **Payment APIs**: Send/receive via Lightning
- **UMA Protocol**: Email-like addresses ($user@ard.mn)
- **Grid API**: International bank payouts
- **Webhooks**: Payment notifications
- **Compliance**: Built-in Travel Rule, OFAC screening

### 6.3 Daily Reconciliation Process (Outbound Only)

Scheduled for 00:00 UTC daily:

```
1. Calculate outbound BTC pool usage (yesterday)
2. Calculate MNT collected from users (yesterday)
3. ARD APP replenishes pool with exact amount used
4. Record reconciliation for audit trail

Note: Inbound transactions NOT included
      (already settled via immediate iDAX execution)
```

---

## 7. Security & Compliance

### 7.1 KYC/AML Requirements

**Managed by ARD APP:**
- User KYC verification
- AML screening
- Risk scoring
- Document verification

**Provided to Custody SaaS via API:**
- User verification status
- Risk scores
- Travel Rule data (when needed)

### 7.2 Compliance Checks

**Per Transaction:**
- KYC status verification
- AML screening (high-value transactions)
- OFAC sanctions screening (via Lightspark)
- Travel Rule compliance (transactions >$1000 USD)

### 7.3 Security Measures

- **API Authentication**: OAuth 2.0 + JWT tokens
- **Webhook Verification**: HMAC signatures
- **Remote Signing**: Optional - keep keys in ARD APP's HSM
- **Rate Limiting**: Prevent abuse and DDoS
- **Encryption**: TLS 1.3 for all API calls
- **2FA/MFA**: Required for high-value transactions
- **Audit Logging**: Complete audit trail for compliance

---

## 8. Conclusion

This technical architecture provides a comprehensive solution for ARD Financial Group's cross-border remittance product using Lightspark's Lightning Network infrastructure.

### Key Benefits:

1. **Cost Efficiency**: 99.9% reduction in settlement costs via pre-funded BTC pool
2. **Fast Settlement**: < 5 seconds for international, instant for internal transfers
3. **Zero Fees for Internal**: Free transfers between ARD users
4. **Scalable**: Lightspark's managed infrastructure handles all complexity
5. **Compliant**: Built-in Travel Rule, OFAC, KYC/AML support
6. **Global Reach**: 140+ countries, 120+ currencies
7. **No Lightning Node Required**: Fully managed by Lightspark
8. **Zero Price Risk**: Immediate iDAX execution for inbound transactions ✅

### Implementation Priority:

**Phase 1** (Months 1-2): Internal ARD transfers (zero fee, instant)
**Phase 2** (Months 2-3): Outbound international (Mongolia → World)
**Phase 3** (Months 3-4): Inbound international with immediate iDAX execution
**Phase 4** (Months 4-5): External bank payouts (ACH, SEPA, etc.)

---

**Document Version:** 2.0
**Last Updated:** 2025-11-03
**Status:** Ready for review
**Share with:** Internal ARD team + Lightspark
