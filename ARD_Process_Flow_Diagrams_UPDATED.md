# ARD Financial Group - Process Flow Diagrams
## Cross-Border Remittance System

**Version:** 2.0 (Updated)
**Date:** 2025-11-03

---

## Transaction Type Overview

```
┌────────────────────────────────────────────────────────────────────┐
│             ARD REMITTANCE TRANSACTION TYPES                       │
└────────────────────────────────────────────────────────────────────┘

TYPE A: INTERNAL ARD TRANSFERS
═══════════════════════════════════════════════════════════════════
User A (Mongolia) ──────▶ User B (Mongolia)
                 [ARD Internal Ledger Only]

Characteristics:
✓ Instant settlement (< 100ms)
✓ Zero fees
✓ Ledger transaction only
✓ No blockchain involved
✓ Same currency (MNT)

Use Cases:
• Friend-to-friend payments
• Bill splitting
• Merchant payments within ARD ecosystem
• Family transfers


TYPE B: OUTBOUND INTERNATIONAL
═══════════════════════════════════════════════════════════════════
User (Mongolia) ──────▶ Recipient (International)
        [MNT] ──▶ [BTC via Lightning] ──▶ [USD/Other Currency]

Characteristics:
• 5-10 second settlement
• Low fees (0.5-1.5%)
• Uses ARD APP's pre-funded BTC pool
• Pool managed by ARD APP (not custody platform)
• 140+ countries supported

Use Cases:
• Sending money to family abroad
• International shopping
• Cross-border business payments
• Freelancer payments


TYPE C: INBOUND INTERNATIONAL ⚠️ CRITICAL FLOW
═══════════════════════════════════════════════════════════════════
Sender (International) ──────▶ User (Mongolia)
    [USD/Other Currency] ──▶ [BTC via Lightning] ──▶ [MNT]

**IMPORTANT:** BTC is IMMEDIATELY sold on iDAX for MNT when received
(NOT marked for daily reconciliation)

Characteristics:
• 5-10 second settlement
• Immediate iDAX execution upon receipt
• Low fees (0.5-1.5%)
• No price risk (instant conversion)
• Receive from 140+ countries

Use Cases:
• Receiving money from family abroad
• Freelancer income
• International business payments
• Remittance receipts


TYPE D: EXTERNAL BANK PAYOUTS
═══════════════════════════════════════════════════════════════════
User (ARD Balance) ──────▶ Bank Account (International or Mongolia)

**Mongolia Banks:** Real-time (instant to a few hours)
**International Banks:** 1-3 business days via ACH/SEPA/etc.

Characteristics:
• Mongolia: Real-time domestic transfer
• International: 1-3 business days
• Moderate fees (1-2%)
• Uses Lightspark Grid for international
• Direct bank transfers

Use Cases:
• Cash out to personal bank account
• Withdraw to savings
• Pay bills in foreign country
• Salary disbursement
```

---

## Type A: Internal ARD Transfers (Zero-Fee, Instant)

### Happy Path Flow

```
┌────────────────────────────────────────────────────────────────────┐
│           INTERNAL ARD TRANSFER - DETAILED FLOW                    │
└────────────────────────────────────────────────────────────────────┘

ACTORS:
• Anhaa: Sender (ARD APP User in Mongolia)
• Bat: Receiver (ARD APP User in Mongolia)
• ARD APP: Mobile/Web application (manages users, KYC, funds)
• Custody SaaS: Backend ledger platform

┌──────────────┐
│ Anhaa        │
│ (Sender)     │
└──────┬───────┘
       │
       │ (1) Opens ARD APP
       │     Navigates to "Send Money"
       │     Selects "To ARD User"
       │
       ▼
┌──────────────────────────────────────┐
│ ARD APP (UI)                         │
├──────────────────────────────────────┤
│ [Search Recipients]                  │
│  • Search by UMA address             │
│  • Search by phone number            │
│  • Search by name                    │
│  • Select from contacts              │
└──────┬───────────────────────────────┘
       │
       │ (2) Anhaa enters:
       │     • Recipient: $bat@ard.mn
       │     • Amount: 50,000 MNT
       │     • Note: "Dinner payment"
       │
       ▼
┌──────────────────────────────────────┐
│ ARD APP                              │
│ • Validates input                    │
│ • Shows preview:                     │
│   ┌────────────────────────────┐    │
│   │ Send to: Bat                │    │
│   │ Amount: 50,000 MNT          │    │
│   │ Fee: FREE                   │    │
│   │ Bat receives: 50,000 MNT    │    │
│   │ Instant delivery            │    │
│   │                             │    │
│   │ [Confirm] [Cancel]          │    │
│   └────────────────────────────┘    │
└──────┬───────────────────────────────┘
       │
       │ (3) Anhaa confirms
       │     API Call to Custody SaaS
       │
       ▼
┌────────────────────────────────────────────────────────────────┐
│ CUSTODY SAAS BACKEND (Ledger Platform)                        │
│                                                                │
│ STEP 1: Validate Request                                      │
│ ───────────────────────                                       │
│   ✓ Check authentication token                                │
│   ✓ Verify sender is KYC'd (via ARD APP API)                  │
│   ✓ Validate recipient UMA exists                             │
│   ✓ Check amount > 0                                          │
│   ✓ Check amount <= daily limit                               │
│                                                                │
│ STEP 2: Execute Transfer (Ledger Transaction)                 │
│ ────────────────────────────────────────                       │
│   (a) Lock sender's balance                                    │
│       Available: 500,000 MNT ✓                                 │
│                                                                │
│   (b) Validate sufficient balance                             │
│       500,000 >= 50,000 ✓                                      │
│                                                                │
│   (c) Validate recipient exists                                │
│       Bat (user_id = 'bat') ✓                                  │
│                                                                │
│   (d) Create transaction record                                │
│       tx_id: 'tx_abc123'                                       │
│       type: 'internal'                                         │
│       status: 'completed'                                      │
│       fee: 0 MNT                                               │
│                                                                │
│   (e) Update ledger atomically:                                │
│       - Debit Anhaa: -50,000 MNT                               │
│       - Credit Bat: +50,000 MNT                                │
│                                                                │
│   ✓ Transaction completed                                      │
│                                                                │
│ STEP 3: Notify Users                                          │
│ ──────────────────                                            │
│   • Send push notification to Anhaa                            │
│   • Send push notification to Bat                              │
│                                                                │
│ TOTAL PROCESSING TIME: 50-150ms                                │
└────────────────────────────────────────────────────────────────┘
       │
       ├──────────────────────────────────────┐
       │                                      │
       ▼                                      ▼
┌──────────────────┐                  ┌──────────────────┐
│ Anhaa's Phone    │                  │ Bat's Phone      │
│                  │                  │                  │
│ Push:            │                  │ Push:            │
│ ✓ Sent 50,000    │                  │ ✓ Received       │
│   MNT to Bat     │                  │   50,000 MNT     │
│                  │                  │   from Anhaa     │
│ New balance:     │                  │                  │
│ 450,000 MNT      │                  │ New balance:     │
│                  │                  │ 150,000 MNT      │
│ [View Details]   │                  │                  │
└──────────────────┘                  │ [View Details]   │
                                      └──────────────────┘

SUMMARY:
═══════════════════════════════════════════════════════════════
✓ No external network calls (Lightning, iDAX, etc.)
✓ Simple ledger transaction with atomic guarantees
✓ Instant settlement (< 100ms)
✓ Zero fees (internal transfer)
✓ Both users see updated balances immediately
✓ No currency conversion needed
✓ No blockchain fees
✓ Can handle thousands per second
```

---

## Type C: Inbound International (CRITICAL UPDATED FLOW)

### 🚨 IMPORTANT: Immediate iDAX Execution

**OLD FLOW (WRONG):**
```
Lightning arrives → Mark for daily reconciliation → Next day settle
```

**NEW FLOW (CORRECT):**
```
Lightning arrives → IMMEDIATELY execute iDAX sell order → Credit user → Complete
```

**Why immediate execution?**
- Avoids 2-3 minute price risk during Lightning settlement
- Avoids additional delays waiting for daily reconciliation
- Better user experience (funds available immediately)
- Reduces fee exposure

### Updated Flow: USA → Mongolia

```
┌────────────────────────────────────────────────────────────────────┐
│   INBOUND INTERNATIONAL - USA TO MONGOLIA (IMMEDIATE EXECUTION)    │
└────────────────────────────────────────────────────────────────────┘

ACTORS:
• Battulga: Sender in USA
• Anhaa: Receiver in Mongolia (ARD APP User)
• Sender's VASP: US wallet/bank
• Custody SaaS: ARD backend ledger platform
• ARD APP: Manages user data, KYC, liquidity
• iDAX: Provides trading API
• Lightspark: Lightning Network infrastructure

═══════════════════════════════════════════════════════════════════
PHASE 1: UMA ADDRESS DISCOVERY
═══════════════════════════════════════════════════════════════════

Battulga (USA)          US Wallet App
  │                          │
  │ (1) "Send $100 to        │
  │     $anhaa@ard.mn"       │
  │──────────────────────────▶│
  │                          │
  │                          │ (2) Parse UMA address:
  │                          │     username: anhaa
  │                          │     domain: ard.mn
  │                          │
  │                          │ (3) LNURLP Lookup:
  │                          │     GET https://ard.mn/
  │                          │         .well-known/lnurlp/anhaa
  │                          │
  │                          ▼
  │              ┌────────────────────────────┐
  │              │ CUSTODY SAAS               │
  │              │ (UMA Endpoint)             │
  │              │                            │
  │              │ (4) Query ARD APP for:     │
  │              │     • Anhaa's KYC status   │
  │              │     • Account active?      │
  │              │     • Can receive?         │
  │              │                            │
  │              │ Response from ARD APP:     │
  │              │ • KYC: verified ✓          │
  │              │ • Status: active ✓         │
  │              │ • Can receive ✓            │
  │              │                            │
  │              │ (5) Query iDAX for rates:  │
  │              │     BTC → MNT rate         │
  │              │     1 BTC = 40,000,000 MNT │
  │              │                            │
  │              │ (6) Generate LNURLP        │
  │              │     response               │
  │              └────────────┬───────────────┘
  │                          │
  │  (7) LNURLP Response     │
  │◄──────────────────────────┤
  │                          │
  │  • Anhaa verified ✓      │
  │  • Accepts MNT, USD, BTC │
  │  • Conversion rates      │
  │                          │
  │  (8) Battulga confirms   │
  │──────────────────────────▶│

═══════════════════════════════════════════════════════════════════
PHASE 2: PAYMENT EXECUTION
═══════════════════════════════════════════════════════════════════

US Wallet                  LIGHTSPARK
     │                          │
     │ (9) Pay Lightning Invoice│
     │      ($100 = 0.00166 BTC)│
     │──────────────────────────▶│
     │                          │
     │                          │ Routing...
     │                          │ (2-5 seconds)
     │                          │
     │                          ▼
     │                  ┌────────────────┐
     │                  │ LIGHTSPARK     │
     │                  │ (ARD's node)   │
     │                  │                │
     │                  │ Payment        │
     │                  │ received! ✓    │
     │                  │                │
     │                  │ Amount:        │
     │                  │ 0.00166 BTC    │
     │                  └────────┬───────┘
     │                          │
     │                          │ (10) Webhook:
     │                          │      payment_received
     │                          │
     │                          ▼
     │                  ┌────────────────────┐
     │                  │ CUSTODY SAAS       │
     │                  │                    │
     │                  │ WebhookHandler     │
     │                  └────────┬───────────┘
     │                          │
     │                          │ ⚠️ CRITICAL: IMMEDIATE EXECUTION
     │                          │
     │                          ▼

═══════════════════════════════════════════════════════════════════
PHASE 3: 🚨 IMMEDIATE IDAX EXECUTION (CRITICAL CHANGE)
═══════════════════════════════════════════════════════════════════

CUSTODY SAAS                    iDAX API
     │                              │
     │ (11) IMMEDIATE API call      │
     │      to iDAX                 │
     │──────────────────────────────▶│
     │                              │
     │  Request:                    │
     │  {                           │
     │    "action": "sell",         │
     │    "asset": "BTC",           │
     │    "amount": 0.00166,        │
     │    "for": "MNT",             │
     │    "type": "market",         │
     │    "priority": "immediate"   │
     │  }                           │
     │                              │
     │                              │ (12) Execute immediately:
     │                              │      • Market sell order
     │                              │      • 0.00166 BTC → MNT
     │                              │      • Rate: 40M MNT/BTC
     │                              │      • Result: 66,400 MNT
     │                              │      • Fee: 0.1% = 66 MNT
     │                              │      • Net: 66,334 MNT
     │                              │
     │                              │ (13) Execution complete
     │  Response:                   │
     │◄──────────────────────────────│
     │  {                           │
     │    "status": "executed",     │
     │    "btc_sold": 0.00166,      │
     │    "mnt_received": 66,334,   │
     │    "rate": 40,000,000,       │
     │    "fee": 66,                │
     │    "execution_time": "250ms" │
     │  }                           │
     │                              │
     ▼

═══════════════════════════════════════════════════════════════════
PHASE 4: CREDIT USER & COMPLETE
═══════════════════════════════════════════════════════════════════

CUSTODY SAAS
     │
     │ (14) Update Ledger:
     │ ─────────────────
     │  • Received: 0.00166 BTC (from Lightning)
     │  • Sold for: 66,334 MNT (from iDAX)
     │  • Credit Anhaa: +66,334 MNT
     │
     │ (15) Update ARD APP's BTC pool:
     │ ──────────────────────────────
     │  • BTC pool owned by ARD APP
     │  • Pool increased by: +0.00166 BTC
     │  • (Custody just tracks in ledger)
     │
     │ (16) Transaction Record:
     │ ──────────────────────
     │  • Type: inbound_international
     │  • Status: completed
     │  • BTC received: 0.00166
     │  • MNT credited: 66,334
     │  • iDAX fee: 66 MNT
     │  • Executed immediately: YES ✓
     │
     │ (17) Notify user
     │
     ▼
┌──────────────┐
│Anhaa's Phone │
│              │
│ Push:        │
│ ✓ Received!  │
│              │
│ Received     │
│ 66,334 MNT   │
│ from         │
│ Battulga     │
│ (USA)        │
│              │
│ Available    │
│ immediately! │
│              │
│ [View]       │
│              │
└──────────────┘

TIMING BREAKDOWN:
═══════════════════════════════════════════════════════════════════
• Lightning routing: 2-5 seconds
• Webhook processing: <100ms
• iDAX sell execution: 200-500ms
• Ledger update: <100ms
• User notification: <100ms

TOTAL TIME: ~3-6 seconds (end-to-end)

KEY BENEFITS:
═══════════════════════════════════════════════════════════════════
✓ NO price risk (immediate conversion)
✓ NO waiting for daily reconciliation
✓ Funds available to user immediately
✓ Simple flow (receive → sell → credit)
✓ Better user experience
✓ Lower operational risk

COST PER TRANSACTION:
═══════════════════════════════════════════════════════════════════
• Lightning fee: ~$0.05
• iDAX trading fee: 0.2% = 132 MNT (~$0.035)
• Total cost: ~$0.085 per transaction

vs OLD APPROACH (daily reconciliation):
• Would accumulate BTC exposure
• Price risk during holding period
• More complex operations
• Delayed user crediting
```

---

## Type D: External Bank Payouts

### Mongolia Bank Withdrawals (Real-Time)

```
┌────────────────────────────────────────────────────────────────────┐
│    EXTERNAL PAYOUT - MONGOLIA BANK (REAL-TIME) ✅                  │
└────────────────────────────────────────────────────────────────────┘

Anhaa has 500,000 MNT in ARD APP balance
Wants to withdraw to Mongolia bank account

FLOW:
═════════════════════════════════════════════════════════════════════

Anhaa                       ARD APP
  │                              │
  │ (1) "Withdraw to Bank"       │
  │     Amount: 100,000 MNT      │
  │     To: Khan Bank account    │
  │─────────────────────────────▶│
  │                              │
  │                              │ (2) Process:
  │                              │     • Verify account
  │                              │     • Check balance
  │                              │     • Fee: 500 MNT
  │                              │
  │  (3) Show confirmation       │
  │◄─────────────────────────────│
  │  You withdraw: 100,500 MNT   │
  │  Bank receives: 100,000 MNT  │
  │  Fee: 500 MNT                │
  │  Time: Instant - 2 hours     │
  │                              │
  │  (4) Confirm                 │
  │─────────────────────────────▶│
  │                              │
  │                              ▼
  │                  ┌────────────────────────┐
  │                  │ CUSTODY SAAS           │
  │                  │                        │
  │                  │ • Debit: 100,500 MNT   │
  │                  │ • Create payout record │
  │                  └────────┬───────────────┘
  │                          │
  │                          │ (5) Initiate bank transfer
  │                          │     (Mongolia domestic)
  │                          │
  │                          ▼
  │                  ┌────────────────────────┐
  │                  │ MONGOLIA BANKING       │
  │                  │ SYSTEM                 │
  │                  │                        │
  │                  │ Khan Bank              │
  │                  │ Account: XXXX1234      │
  │                  │                        │
  │                  │ Credit: +100,000 MNT   │
  │                  │                        │
  │                  │ REAL-TIME:             │
  │                  │ • Instant to 2 hours   │
  │                  │ • Domestic transfer    │
  │                  └────────┬───────────────┘
  │                          │
  │  (6) Confirmation         │
  │◄──────────────────────────┤
  │                          │
  │  ✓ Transfer completed!   │
  │  100,000 MNT in your     │
  │  Khan Bank account       │

TIMING: Instant to 2 hours (Mongolia domestic banking)
COST: ~500 MNT (~$0.15)
STATUS: Real-time capability ✅
```

### International Bank Payouts (ACH/SEPA Example: USA)

```
┌────────────────────────────────────────────────────────────────────┐
│    EXTERNAL PAYOUT - USA BANK ACCOUNT (ACH)                        │
└────────────────────────────────────────────────────────────────────┘

Bold has 500,000 MNT in ARD APP
Wants to send to his US bank account

FLOW:
═════════════════════════════════════════════════════════════════════

Bold                        ARD APP
  │                              │
  │ (1) "Withdraw to Bank"       │
  │     Amount: 100,000 MNT      │
  │     To: US bank (ACH)        │
  │     Currency: USD            │
  │─────────────────────────────▶│
  │                              │
  │                              │ (2) Get quote:
  │                              │     • Query iDAX: MNT/BTC
  │                              │     • Query Lightspark: BTC/USD
  │                              │     • Calculate total
  │                              │
  │  (3) Show quote              │
  │◄─────────────────────────────│
  │  Send: 102,000 MNT           │
  │  Receive: ~$29 USD           │
  │  Fee: 2,000 MNT (2%)         │
  │  Time: 1-2 business days     │
  │                              │
  │  (4) Confirm                 │
  │─────────────────────────────▶│
  │                              │
  │                              ▼
  │                  ┌────────────────────────┐
  │                  │ CUSTODY SAAS           │
  │                  │                        │
  │                  │ • Debit: 102,000 MNT   │
  │                  │ • Use ARD's BTC pool   │
  │                  │ • Reserve 0.00048 BTC  │
  │                  └────────┬───────────────┘
  │                          │
  │                          │ (5) Call Lightspark Grid API
  │                          │
  │                          ▼
  │                  ┌────────────────────────┐
  │                  │ LIGHTSPARK GRID        │
  │                  │                        │
  │                  │ • Convert BTC → USD    │
  │                  │ • Initiate ACH         │
  │                  │ • $29 to US bank       │
  │                  └────────┬───────────────┘
  │                          │
  │                          │ (6) ACH Processing
  │                          │     (1-2 business days)
  │                          │
  │                          ▼
  │                  ┌────────────────────────┐
  │                  │ US BANK                │
  │                  │                        │
  │                  │ Bold's Account         │
  │                  │ Credit: +$29.00        │
  │                  └────────┬───────────────┘
  │                          │
  │  (7) Confirmation (Day 2-3)│
  │◄──────────────────────────┤
  │                          │
  │  ✓ Transfer completed!   │
  │  $29.00 in your bank     │

TIMING: 1-2 business days (US ACH)
COST: ~2,000 MNT (2%) + network fees
```

---

## Daily Reconciliation (Simplified)

**Note:** With immediate iDAX execution on inbound transactions, daily reconciliation is primarily for:
1. Outbound Lightning transactions (using pre-funded pool)
2. Bank payouts
3. Pool balance verification

```
┌────────────────────────────────────────────────────────────────────┐
│              DAILY RECONCILIATION (SIMPLIFIED)                      │
└────────────────────────────────────────────────────────────────────┘

00:00 UTC (Daily)
     │
     │ Automated Job Runs
     │
     ▼
┌──────────────────────────┐
│ CUSTODY SAAS             │
│                          │
│ Calculate yesterday:     │
│ ─────────────────────   │
│                          │
│ OUTBOUND:                │
│ • Lightning payments     │
│   sent from pool         │
│ • Total: 2.5 BTC used    │
│                          │
│ INBOUND:                 │
│ • Already settled!       │
│   (immediate iDAX exec)  │
│ • Just verify records    │
│                          │
│ PAYOUTS:                 │
│ • Bank withdrawals       │
│ • Total: 0.5 BTC used    │
│                          │
│ NET POOL USAGE:          │
│ • Used: 3 BTC            │
│ • Pool needs refill      │
└──────────┬───────────────┘
          │
          │ Request from ARD APP
          │
          ▼
┌──────────────────────────┐
│ ARD APP                  │
│ (Liquidity Manager)      │
│                          │
│ • Transfer 3 BTC         │
│   to Lightspark wallet   │
│ • Replenish pool         │
│ • Pool: 10 BTC restored  │
└──────────────────────────┘

FREQUENCY: Once daily
COST: Minimal (one blockchain TX)
COMPLEXITY: Low (mainly outbound tracking)
```


**Document Version:** 2.0
**Last Updated:** 2025-11-03 (Critical flow corrections)
**Next Review:** Before sharing with Lightspark
