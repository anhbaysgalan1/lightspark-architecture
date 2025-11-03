# ARD Financial Group - Business Operations & Treasury Management
## Cross-Border Remittance System

**Version:** 2.0
**Date:** 2025-11-03
**Status:** Ready for review
**Share with:** Internal ARD team + Lightspark

---

## Table of Contents

1. [Fee Structure & Revenue Model](#fee-structure--revenue-model)
2. [Treasury Management](#treasury-management)
3. [Cost Analysis](#cost-analysis)
4. [Risk Management](#risk-management)
5. [Operational Procedures](#operational-procedures)
6. [Financial Projections](#financial-projections)

---

## 1. Fee Structure & Revenue Model

### 1.1 Transaction Fee Structure

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    TRANSACTION FEE STRUCTURE                                     │
└─────────────────────────────────────────────────────────────────────────────────┘

TYPE A: INTERNAL ARD TRANSFERS (Mongolia ↔ Mongolia)
═══════════════════════════════════════════════════════════════════════════════════
Fee: 0% (FREE) ✅

Why zero fees?
• Encourages user adoption and stickiness
• Builds network effects within ARD ecosystem
• No external network costs (database only)
• Competitive advantage vs traditional banks
• Users keep money in ARD ecosystem

Revenue strategy:
• Monetize when users cash out internationally
• Increased transaction volume drives data
• Cross-sell other ARD financial products


TYPE B: OUTBOUND INTERNATIONAL (Mongolia → Other Countries)
═══════════════════════════════════════════════════════════════════════════════════
Fee Structure: 0.5% - 1.5% (tiered based on amount)

┌────────────────┬──────────────┬─────────────┬──────────────────┐
│ Amount (MNT)   │ Fee %        │ Min Fee     │ Max Fee          │
├────────────────┼──────────────┼─────────────┼──────────────────┤
│ 0 - 100,000    │ 1.5%         │ 1,000 MNT   │ 1,500 MNT        │
│ 100,001 - 1M   │ 1.0%         │ 1,500 MNT   │ 10,000 MNT       │
│ 1M - 10M       │ 0.75%        │ 10,000 MNT  │ 75,000 MNT       │
│ 10M+           │ 0.5%         │ 75,000 MNT  │ No max           │
└────────────────┴──────────────┴─────────────┴──────────────────┘

Examples:
• Send 50,000 MNT → Fee: 1,000 MNT (2%, but min applies)
• Send 100,000 MNT → Fee: 1,500 MNT (1.5%)
• Send 500,000 MNT → Fee: 5,000 MNT (1%)
• Send 5,000,000 MNT → Fee: 37,500 MNT (0.75%)

Comparison to Traditional Remittance:
Traditional (Western Union, MoneyGram): 5-10% + FX spread
ARD: 0.5-1.5% (80-95% cheaper!)


TYPE C: INBOUND INTERNATIONAL (Other Countries → Mongolia) 🚨 V2.0 UPDATE
═══════════════════════════════════════════════════════════════════════════════════
Fee Structure: 0.5% - 1.0% (absorbed from FX spread)

Fee charged to: Sender's VASP (transparent to ARD user)
ARD user receives: Full amount with small FX markup
FX Markup: ~0.5-0.8% on BTC/MNT conversion

🚨 V2.0: Immediate iDAX execution model
────────────────────────────────────────
When BTC arrives via Lightning:
1. IMMEDIATELY sell BTC for MNT on iDAX (200-500ms)
2. Credit user account with MNT
3. Transaction COMPLETED (no reconciliation needed)

Cost to ARD: 0.1% iDAX trading fee

Example:
• Battulga (USA) sends $100
• Conversion: $100 → 0.00166 BTC (via Lightning)
• iDAX execution: 0.00166 BTC → 66,334 MNT (immediate)
• iDAX fee: 0.1% = 66 MNT (~$0.02)
• Without markup: 67,000 MNT
• With markup (1.0%): 66,334 MNT
• ARD net revenue: 600 MNT (~$0.17) after iDAX fee


TYPE D: EXTERNAL BANK PAYOUTS
═══════════════════════════════════════════════════════════════════════════════════
Fee Structure: Varies by destination and speed

┌────────────────────────┬──────────────┬────────────────┬──────────────┐
│ Method                 │ ARD Fee      │ Network Fee    │ Delivery     │
├────────────────────────┼──────────────┼────────────────┼──────────────┤
│ Mongolia Banks         │ ~500 MNT     │ Minimal        │ Real-time    │
│ (Domestic - Realtime)  │              │                │ to 2 hours   │
├────────────────────────┼──────────────┼────────────────┼──────────────┤
│ ACH (USA)              │ 2.0%         │ ~$0.50         │ 1-2 days     │
│ FedNow (USA)           │ 2.5%         │ ~$1.50         │ Instant      │
│ SEPA (Europe)          │ 2.0%         │ ~€0.50         │ 1-2 days     │
│ SEPA Instant           │ 2.5%         │ ~€1.00         │ Instant      │
│ PIX (Brazil)           │ 2.5%         │ ~R$1.00        │ Instant      │
│ UPI (India)            │ 2.5%         │ ~₹5            │ Instant      │
│ SPEI (Mexico)          │ 2.5%         │ ~$10 MXN       │ Instant      │
└────────────────────────┴──────────────┴────────────────┴──────────────┘

Example (Mongolia bank - Real-time):
• Withdraw 100,000 MNT to Mongolia bank
• ARD fee: 500 MNT
• Network fee: Minimal
• Total cost: ~500 MNT
• Time: Instant to 2 hours ✅

Example (ACH to USA):
• Withdraw 100,000 MNT to US bank
• ARD fee: 2,000 MNT (2%)
• Network fee: ~500 MNT ($0.50)
• Total cost: 2,500 MNT
• User receives: ~$29 USD
• Time: 1-2 business days
```

### 1.2 Revenue Streams

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         REVENUE STREAM BREAKDOWN                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

PRIMARY REVENUE: Transaction Fees
═══════════════════════════════════════════════════════════════════════════════════
1. Outbound International Transfers
   • Volume: ~70% of international transactions
   • Average fee: 1.0%
   • Expected monthly volume: 10,000 transactions
   • Average amount: 500,000 MNT
   • Monthly revenue: 10,000 × 500,000 × 1% = 50,000,000 MNT (~$14,500/mo)

2. Inbound International Transfers 🚨 V2.0 UPDATED
   • Volume: ~20% of international transactions
   • Average markup: 0.8% (net of iDAX fee)
   • Expected monthly volume: 3,000 transactions
   • Average amount: 350,000 MNT equivalent
   • Monthly revenue: 3,000 × 350,000 × 0.8% = 8,400,000 MNT (~$2,450/mo)
   • Note: Already includes iDAX 0.2% fee deduction

3. External Bank Payouts
   • Volume: ~10% of international transactions
   • Average fee: 2.0%
   • Expected monthly volume: 1,500 transactions
   • Average amount: 400,000 MNT
   • Monthly revenue: 1,500 × 400,000 × 2% = 12,000,000 MNT (~$3,500/mo)

TOTAL PRIMARY REVENUE: ~70,400,000 MNT/month (~$20,450/month)
ANNUAL: ~844,800,000 MNT (~$245,000/year)


SECONDARY REVENUE: FX Spreads
═══════════════════════════════════════════════════════════════════════════════════
• iDAX provides wholesale rates
• ARD applies 0.1-0.3% retail markup (outbound)
• Additional revenue: ~10-20% of primary revenue
• Estimated: ~8,500,000 MNT/month (~$2,500/month)


TERTIARY REVENUE: Float Interest
═══════════════════════════════════════════════════════════════════════════════════
• MNT float: Average 500M MNT held
• Interest rate: ~8% APY (Mongolia rates)
• Annual interest: 40M MNT (~$11,600/year)
• Monthly: ~3.3M MNT (~$965/month)


TOTAL ESTIMATED REVENUE:
═══════════════════════════════════════════════════════════════════════════════════
Monthly: ~82,200,000 MNT (~$23,900/month)
Annual: ~986,400,000 MNT (~$287,000/year)

Note: Based on conservative projections (Year 1)
Growth potential: 5-10x in Years 2-3 with scale
```

---

## 2. Treasury Management

### 2.1 BTC Pool Management

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      BTC POOL MANAGEMENT STRATEGY                                │
│                      (OWNED BY ARD APP, TRACKED BY CUSTODY SAAS)                 │
└─────────────────────────────────────────────────────────────────────────────────┘

TARGET POOL SIZE: 10-20 BTC (Dynamic based on volume)
═══════════════════════════════════════════════════════════════════════════════════

Calculation Formula:
────────────────────
Target Pool = (Average Daily Outbound BTC × Safety Factor) + Buffer

Where:
• Average Daily Outbound BTC = Historical 7-day average
• Safety Factor = 1.5x (to handle spikes)
• Buffer = 2 BTC (minimum operational buffer)

Example:
• Average daily outbound: 5 BTC
• Target: (5 × 1.5) + 2 = 9.5 BTC
• Rounded up: 10 BTC


UTILIZATION THRESHOLDS & ACTIONS:
═══════════════════════════════════════════════════════════════════════════════════

┌─────────────────┬─────────────┬────────────────────────────────────────┐
│ Utilization     │ Status      │ Action                                 │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 0-35%           │ ✅ OPTIMAL  │ • No action                            │
│ (≥6.5 BTC avail)│             │ • Normal operations                    │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 35-50%          │ ⚠️ WATCH    │ • Monitor closely                      │
│ (5-6.5 BTC)     │             │ • Notify ARD APP treasury team (info)  │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 50-70%          │ ⚠️ WARNING  │ • Alert ARD APP treasury team          │
│ (3-5 BTC)       │             │ • Prepare replenishment                │
│                 │             │ • Increase monitoring frequency        │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 70-85%          │ 🔴 HIGH     │ • URGENT: Request replenishment        │
│ (1.5-3 BTC)     │             │ • Alert ARD APP senior management      │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 85-95%          │ 🚨 CRITICAL │ • EMERGENCY replenishment from ARD APP │
│ (0.5-1.5 BTC)   │             │ • Use emergency reserves               │
│                 │             │ • Consider pausing outbound TXs        │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ >95%            │ 🆘 DEPLETED │ • HALT outbound transactions           │
│ (<0.5 BTC)      │             │ • Emergency call with ARD APP execs    │
│                 │             │ • Fast-track BTC acquisition           │
└─────────────────┴─────────────┴────────────────────────────────────────┘


REPLENISHMENT PROCEDURES (OUTBOUND ONLY):
═══════════════════════════════════════════════════════════════════════════════════

Daily Reconciliation (Automated - 00:00 UTC):
──────────────────────────────────────────────
1. Calculate net BTC flow (OUTBOUND ONLY)
2. 🚨 NOTE: Inbound NOT included (already settled via immediate iDAX)
3. If deficit: Request BTC from ARD APP
4. ARD APP transfers BTC to Lightspark wallet
5. Verify pool restored to target
6. Generate reconciliation report

🚨 V2.0 CRITICAL CHANGE:
────────────────────────
Inbound Lightning transactions are NOT reconciled daily.
They are settled immediately upon receipt via iDAX execution.

Only outbound Lightning transactions consume from the pool and
require daily reconciliation.

Emergency Replenishment (Manual - When Critical):
──────────────────────────────────────────────────
1. Custody SaaS alerts ARD APP treasury
2. Calculate immediate BTC needed
3. ARD APP transfers from hot wallet (priority)
4. Monitor blockchain confirmation
5. Verify pool status every 5 minutes
6. Document incident for post-mortem

Target SLA:
• Daily reconciliation: Complete by 01:00 UTC (1 hour)
• Emergency replenishment: < 30 minutes

Transfer Methods:
• Lightning Network: Instant (preferred for small amounts)
• On-chain BTC: ~10-60 minutes (1-6 confirmations)
```

### 2.2 MNT Liquidity Management

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      MNT LIQUIDITY MANAGEMENT                                    │
└─────────────────────────────────────────────────────────────────────────────────┘

ARD INTERNAL LEDGER (MNT):
═══════════════════════════════════════════════════════════════════════════════════

Target MNT Balance: 500M - 1B MNT
═════════════════════════════════════

Purpose:
• Cover inbound international payments (users receive MNT)
• Maintain instant liquidity for internal transfers
• Buffer for operational needs

Sources of MNT:
1. Collected from outbound international transfers
   • Users send MNT abroad
   • We collect MNT, use pre-funded BTC pool
   • MNT retained for replenishment + float

2. Collected from bank payout requests
   • Users withdraw to banks
   • We debit their MNT balance
   • MNT available for other users

3. Top-up from ARD APP
   • Users deposit MNT via bank transfer
   • Users deposit via card payment
   • Direct funding to ARD accounts

Uses of MNT:
1. Credit users for inbound international payments 🚨 V2.0
   • Receive BTC via Lightning
   • IMMEDIATELY sell BTC for MNT on iDAX
   • Credit user account with MNT received from iDAX
   • MNT comes directly from iDAX sale

2. Internal transfers
   • User A → User B
   • No net change in total MNT liability
   • Just ledger updates

3. Daily settlement with ARD APP
   • Buy BTC with surplus MNT (for pool replenishment)
   • Coordinate with ARD APP treasury


MONITORING & ALERTS:
═══════════════════════════════════════════════════════════════════════════════════

┌─────────────────┬─────────────┬────────────────────────────────────────┐
│ MNT Balance     │ Status      │ Action                                 │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ >1B MNT         │ ✅ SURPLUS  │ • Coordinate with ARD APP treasury     │
│                 │             │ • Potential to buy BTC (proactive)     │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 500M-1B MNT     │ ✅ OPTIMAL  │ • Normal operations                    │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 250M-500M MNT   │ ⚠️ WATCH    │ • Monitor inbound volume               │
│                 │             │ • Prepare to coordinate with ARD APP   │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ 100M-250M MNT   │ ⚠️ WARNING  │ • Alert ARD APP treasury team          │
│                 │             │ • May need MNT injection               │
├─────────────────┼─────────────┼────────────────────────────────────────┤
│ <100M MNT       │ 🔴 CRITICAL │ • Emergency MNT from ARD APP           │
│                 │             │ • Consider credit line                 │
└─────────────────┴─────────────┴────────────────────────────────────────┘
```

### 2.3 Foreign Currency Management

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                  FOREIGN CURRENCY MANAGEMENT                                     │
└─────────────────────────────────────────────────────────────────────────────────┘

STRATEGY: No Foreign Currency Holdings (Pass-through Model)
═══════════════════════════════════════════════════════════════════════════════════

Why no holdings?
✓ Eliminates FX risk
✓ No capital tied up in foreign currencies
✓ Simplified treasury management
✓ Lightspark handles all FX conversions

How it works:
─────────────
Outbound:
1. User sends MNT
2. Use pre-funded BTC pool
3. BTC sent via Lightning
4. Lightspark/Recipient VASP converts BTC → Foreign Currency
5. Recipient receives foreign currency

Inbound 🚨 V2.0 UPDATE:
1. Sender sends foreign currency
2. Their VASP converts Foreign Currency → BTC
3. BTC sent via Lightning
4. We receive BTC
5. 🚨 IMMEDIATELY execute iDAX: BTC → MNT
6. User credited with MNT
7. Transaction COMPLETED (no reconciliation)

Result: We only touch MNT and BTC. Never hold USD, EUR, etc.


FX RATE MANAGEMENT:
═══════════════════════════════════════════════════════════════════════════════════

Rate Sources:
1. iDAX Exchange
   • MNT/BTC spot rates
   • Real-time updates
   • Wholesale rates

2. Lightspark Grid API
   • BTC/Foreign Currency rates
   • Real-time quotes
   • Valid for 60 seconds
   • Includes Lightspark's spread

Rate Markup Strategy:
─────────────────────
• Display rate = iDAX rate × Lightspark rate × (1 + our markup)
• Our markup: 0.1-0.3%
• Transparent disclosure to users
• Competitive vs banks (5-10% markup)
```

---

## 3. Cost Analysis

### 3.1 Operational Costs

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        MONTHLY OPERATIONAL COSTS                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

INFRASTRUCTURE COSTS:
═══════════════════════════════════════════════════════════════════════════════════

Lightspark Fees:
────────────────
• Lightning Network transactions: $0.05-0.10 per transaction
• Monthly volume: 10,000 outbound + 3,000 inbound = 13,000
• Cost: 13,000 × $0.075 = $975/month

• Grid API (bank payouts): 0.1-0.3% per transaction
• Monthly volume: 1,500 payouts × $29 avg = $43,500
• Cost: $43,500 × 0.2% = $87/month

• Total Lightspark: ~$1,060/month

Cloud Infrastructure:
────────────────────────────
• Servers, databases, cache, storage, networking
• Total: ~$820/month

Monitoring & Security:
──────────────────────
• Monitoring tools, error tracking, alerting
• Total: ~$380/month

TOTAL INFRASTRUCTURE: ~$2,260/month ($27,120/year)


THIRD-PARTY SERVICES:
═══════════════════════════════════════════════════════════════════════════════════

KYC/AML (managed by ARD APP):
──────────────────────────────
• Custody SaaS gets data via API
• Cost: $0 (ARD APP manages)

Compliance Services:
────────────────────
• Basic compliance tools
• Total: ~$500/month

Notifications:
──────────────
• Push, email, SMS services
• Total: ~$200/month

TOTAL THIRD-PARTY: ~$700/month ($8,400/year)


SETTLEMENT & TRANSACTION COSTS 🚨 V2.0 UPDATED:
═══════════════════════════════════════════════════════════════════════════════════

Daily BTC Pool Replenishment (Outbound only):
──────────────────────────────────────────────
• Daily transfers from ARD APP: 30 per month
• Blockchain fee per transfer: ~$2
• Total per settlement: ~$2
• Monthly: 30 × $2 = $60/month

iDAX Trading Fees (Inbound - IMMEDIATE execution):
────────────────────────────────────────────────────
• 🚨 V2.0: Immediate execution for ALL inbound Lightning
• Volume: 3,000 inbound transactions/month
• Average amount: $20 USD equivalent
• Total volume: $60,000/month
• iDAX fee: 0.1% = $60/month

TOTAL SETTLEMENT: ~$120/month ($1,440/year)


PERSONNEL (Custody SaaS Team):
═══════════════════════════════════════════════════════════════════════════════════
• Backend developers (2): $8,000/month
• DevOps engineer (1): $4,500/month
• Compliance officer (1): $4,000/month
• Customer support (2): $3,000/month
• Total: ~$19,500/month ($234,000/year)


GRAND TOTAL MONTHLY COSTS:
═══════════════════════════════════════════════════════════════════════════════════
Infrastructure:      $2,260
Third-party:         $700
Settlement:          $120
Personnel:           $19,500
────────────────────────────
TOTAL:               $22,580/month ($271,000/year)

🚨 V2.0 NOTE: Costs are LOWER than V1.0 due to:
• Reduced settlement costs (immediate execution simpler)
• No reconciliation tracking for inbound transactions
• Lower iDAX fees (only 0.1% on inbound, vs potential price risk)
```

### 3.2 Cost Savings Analysis

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│            COST SAVINGS: PRE-FUNDED POOL vs REAL-TIME SETTLEMENT                 │
└─────────────────────────────────────────────────────────────────────────────────┘

SCENARIO: 10,000 outbound international transactions per month

OPTION A: REAL-TIME SETTLEMENT (Don't do this! ❌)
═══════════════════════════════════════════════════════════════════════════════════
Every transaction requires:
1. iDAX API call
2. MNT → BTC conversion on iDAX
3. BTC withdrawal from iDAX
4. BTC transfer to Lightspark
5. Lightning payment

Costs per transaction:
• iDAX trading fee: 0.1% × $29 avg = $0.029
• iDAX withdrawal: 0.0001 BTC = $6
• Blockchain fee: $2-10
• Processing time: 10-60 minutes
• Total per TX: ~$8-16

Monthly cost:
• 10,000 TX × $10 (average) = $100,000/month
• Annual: $1,200,000/year


OPTION B: PRE-FUNDED BTC POOL (Our approach ✅)
═══════════════════════════════════════════════════════════════════════════════════
Transactions use pre-funded pool (owned by ARD APP):
1. Instant BTC availability
2. Single Lightning payment
3. Daily batch replenishment
4. One transfer per day

Costs:
• Lightning fees: 10,000 × $0.075 = $750/month
• Daily replenishment: 30 × $2 = $60/month
• Total: $810/month
• Annual: $9,720/year

SAVINGS:
═══════════════════════════════════════════════════════════════════════════════════
Monthly: $100,000 - $810 = $99,190 saved
Annual: $1,200,000 - $9,720 = $1,190,280 saved

Cost reduction: 99.2% 💰

Additional benefits:
✓ Instant transactions (5-10 sec vs 10-60 min)
✓ Better user experience
✓ No failed TXs due to exchange issues
✓ Simplified operations
✓ Lower technical complexity


🚨 V2.0 INBOUND COST ANALYSIS:
═══════════════════════════════════════════════════════════════════════════════════

IMMEDIATE iDAX EXECUTION vs DAILY RECONCILIATION:

OPTION A: Daily Reconciliation (Old model ❌)
──────────────────────────────────────────────
• Hold BTC for 12-24 hours
• Price risk: BTC can move 1-5% in 24 hours
• Example: $60,000 inbound volume × 3% drop = $1,800 loss
• Complexity: Tracking, reconciliation overhead

OPTION B: Immediate iDAX Execution (V2.0 ✅)
────────────────────────────────────────────
• Execute within 200-500ms of Lightning receipt
• Price risk: ZERO ✅
• Cost: 0.1% iDAX fee = $60/month on $60K volume
• Complexity: Simple, immediate settlement

WINNER: Immediate execution
• Lower risk ($0 vs potential $1,800/month loss)
• Better UX (user credited immediately)
• Simpler operations
• Cost: Only $60/month (trivial)
```

### 3.3 Break-Even Analysis

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          BREAK-EVEN ANALYSIS                                     │
└─────────────────────────────────────────────────────────────────────────────────┘

MONTHLY COSTS (V2.0): $22,580
MONTHLY REVENUE NEEDED TO BREAK EVEN: $22,580

REVENUE PER TRANSACTION (Average):
═══════════════════════════════════════════════════════════════════════════════════
• Outbound international: 500,000 MNT × 1% = 5,000 MNT (~$1.45)
• Inbound international: 350,000 MNT × 0.8% = 2,800 MNT (~$0.80)
• Bank payouts: 400,000 MNT × 2% = 8,000 MNT (~$2.35)

Weighted average (70% outbound, 20% inbound, 10% payout):
= (0.70 × $1.45) + (0.20 × $0.80) + (0.10 × $2.35)
= $1.015 + $0.16 + $0.235
= $1.41 per transaction


BREAK-EVEN VOLUME:
═══════════════════════════════════════════════════════════════════════════════════
Monthly transactions needed = $22,580 ÷ $1.41 = 16,000 transactions/month

Daily: 16,000 ÷ 30 = ~533 transactions/day

TIMELINE TO BREAK-EVEN (Projected Growth):
═══════════════════════════════════════════════════════════════════════════════════
Month 1: 2,000 TX/month (soft launch)
Month 2: 4,000 TX/month
Month 3: 7,000 TX/month
Month 4: 11,000 TX/month
Month 5: 15,000 TX/month
Month 6: 18,000 TX/month ← BREAK-EVEN ✓

Conclusion: Expect break-even in Month 6 (conservative estimate)
With aggressive marketing: Possible in Month 4

🚨 V2.0 NOTE: Lower costs = earlier break-even potential
```

---

## 4. Risk Management

### 4.1 Financial Risks

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           FINANCIAL RISK MATRIX                                  │
└─────────────────────────────────────────────────────────────────────────────────┘

RISK 1: BTC PRICE VOLATILITY
═══════════════════════════════════════════════════════════════════════════════════
Risk Level: VERY LOW (V2.0 improvement ✅)

Exposure:
• Outbound: BTC held for 5-10 seconds only
• Inbound: 🚨 BTC held for <1 second (immediate iDAX execution)
• Pre-funded pool: 10-20 BTC (owned by ARD APP, managed risk)

🚨 V2.0 IMPROVEMENT:
────────────────────
OLD: Inbound BTC held 12-24h = significant price risk
NEW: Inbound BTC sold in <500ms = ZERO price risk ✅

Mitigation Strategies:
✓ Immediate iDAX execution for inbound (V2.0)
✓ Minimize holding time (5-10 seconds per outbound TX)
✓ Daily reconciliation to rebalance pool
✓ Natural hedging (pool managed by ARD APP)
✓ Monitor BTC price movements (>5% = alert)

Example Impact (V2.0):
• Inbound: NO EXPOSURE (immediate execution)
• Outbound: Minimal exposure (5-10 sec avg)
• Pool: Managed by ARD APP treasury
• Actual risk: Near zero ✅


RISK 2: MNT LIQUIDITY SHORTAGE
═══════════════════════════════════════════════════════════════════════════════════
Risk Level: LOW

Scenario:
• Sudden spike in inbound international payments
• Need to credit users with MNT
• MNT balance insufficient

Mitigation:
✓ iDAX provides MNT immediately (inbound execution)
✓ Maintain 500M-1B MNT float
✓ Real-time monitoring with alerts
✓ Emergency support from ARD APP (parent company)
✓ Can coordinate with ARD APP treasury within minutes

🚨 V2.0 IMPROVEMENT:
────────────────────
Immediate iDAX execution ensures MNT is received
instantly from each inbound transaction, reducing
the need for large MNT reserves.


RISK 3: SETTLEMENT FAILURE WITH IDAX
═══════════════════════════════════════════════════════════════════════════════════
Risk Level: LOW

Scenario:
• iDAX offline or technical issue
• Can't execute immediate trades (inbound)
• Can't replenish BTC pool (outbound)

Mitigation:
✓ iDAX owned by ARD Financial Group (internal)
✓ Direct API access and priority support
✓ SLA with iDAX: 99.9% uptime
✓ Automatic retries with exponential backoff
✓ Alternative exchange backup (Binance, etc.)
✓ Can pause inbound processing if extended outage

Impact:
• Inbound: Can queue for retry (resume in minutes)
• Outbound: Can operate 1-2 days with existing pool
• Emergency: ARD APP can provide alternative solutions


RISK 4: REGULATORY CHANGES
═══════════════════════════════════════════════════════════════════════════════════
Risk Level: MEDIUM (long-term)

Scenarios:
• Mongolia restricts cryptocurrency
• New KYC/AML requirements
• Capital controls on remittances
• Licensing requirements

Mitigation:
✓ Engage with regulators proactively
✓ Join industry associations
✓ Built compliant from day 1
✓ ARD APP handles all KYC/AML
✓ Legal counsel on retainer
✓ Monitor regulatory landscape monthly
```

### 4.2 Operational Risks

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        OPERATIONAL RISK MATRIX                                   │
└─────────────────────────────────────────────────────────────────────────────────┘

RISK 1: SYSTEM DOWNTIME
═══════════════════════════════════════════════════════════════════════════════════
Impact: HIGH  |  Probability: LOW

Mitigation:
• Multi-AZ deployment (99.99% uptime SLA)
• Load balancing across multiple servers
• Database replication
• Automatic failover (< 30 seconds)
• Health monitoring
• 24/7 on-call rotation
• Disaster recovery plan

Target: 99.95% uptime


RISK 2: SECURITY BREACH
═══════════════════════════════════════════════════════════════════════════════════
Impact: CRITICAL  |  Probability: LOW

Mitigation:
• Penetration testing (quarterly)
• Security audits (annual)
• WAF + DDoS protection
• HSM for private keys (ARD APP infrastructure)
• Multi-sig for treasury operations
• Incident response plan
• Cyber insurance


RISK 3: FRAUD / MONEY LAUNDERING
═══════════════════════════════════════════════════════════════════════════════════
Impact: HIGH  |  Probability: MEDIUM

Mitigation:
• KYC verification (via ARD APP)
• AML screening
• OFAC screening (Lightspark)
• Transaction monitoring (real-time)
• Velocity limits
• Behavioral analysis
• Manual review for high-risk
```

---

## 5. Operational Procedures

### 5.1 Daily Operations Checklist

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       DAILY OPERATIONS CHECKLIST                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

MORNING (08:00-09:00):
═══════════════════════════════════════════════════════════════════════════════════
[ ] Review overnight alerts and incidents
[ ] Check daily reconciliation report (from 00:00 UTC run)
[ ] Verify BTC pool balance matches expected
[ ] Check MNT float balance
[ ] Review failed transactions from previous 24h
[ ] Check compliance queue (pending reviews)
[ ] Review system health dashboard
[ ] Check Lightspark service status
[ ] Verify iDAX connectivity and rates
[ ] Brief team on any issues


MID-DAY (12:00-13:00):
═══════════════════════════════════════════════════════════════════════════════════
[ ] Review morning transaction volume
[ ] Check BTC pool utilization trend
[ ] Monitor MNT balance trend
[ ] Review customer support escalations
[ ] Check compliance alerts
[ ] Verify no pending issues


END-OF-DAY (18:00-19:00):
═══════════════════════════════════════════════════════════════════════════════════
[ ] Final check on BTC pool status
[ ] Review day's financial summary
[ ] Check for unresolved incidents
[ ] Brief on-call engineer
[ ] Document any issues or learnings


AUTOMATED (No Manual Action Required):
═══════════════════════════════════════════════════════════════════════════════════
✓ Daily reconciliation (00:00 UTC) - Outbound only
✓ BTC pool monitoring (every 10 min)
✓ Transaction processing (real-time)
✓ 🚨 Inbound iDAX execution (immediate, automatic)
✓ Webhook handling (real-time)
✓ Alerts and notifications (real-time)
✓ Backups (every 6 hours)
```

---

## 6. Financial Projections

### 6.1 3-Year Revenue Forecast

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         3-YEAR REVENUE FORECAST                                  │
└─────────────────────────────────────────────────────────────────────────────────┘

YEAR 1 (Conservative Launch):
═══════════════════════════════════════════════════════════════════════════════════

Month 1-6 (Growth to Break-even):
──────────────────────────────────
• Users: 1,000 → 10,000
• Monthly TXs: 2,000 → 20,000
• Revenue/month: $2,800 → $28,000
• Break-even: Month 6 ✓

Month 7-12 (Scale):
───────────────────
• Users: 10,000 → 20,000
• Monthly TXs: 20,000 → 50,000
• Revenue/month: $28,000 → $70,000

YEAR 1 TOTAL:
═════════════
• Revenue: $381,000
• Costs: $271,000 (V2.0 lower costs)
• Net Profit: $110,000 ✓
• ROI: 41%


YEAR 2 (Growth & Expansion):
═══════════════════════════════════════════════════════════════════════════════════
• Users: 100,000
• Monthly TXs: 150,000
• Monthly revenue: $210,000
• Annual revenue: $2,520,000
• Annual costs: $440,000
• Net Profit: $2,080,000 ✓
• ROI: 473%


YEAR 3 (Maturity):
═══════════════════════════════════════════════════════════════════════════════════
• Users: 200,000
• Monthly TXs: 400,000
• Monthly revenue: $560,000
• Annual revenue: $6,720,000
• Annual costs: $850,000
• Net Profit: $5,870,000 ✓
• ROI: 690%


3-YEAR SUMMARY:
═══════════════════════════════════════════════════════════════════════════════════
• Total Revenue: $9,621,000
• Total Costs: $1,561,000
• Total Net Profit: $8,060,000
• Average Annual ROI: 516%

Market Share (Year 3):
• 200K users out of 500K target market = 40% market share
• Dominant player in Mongolia cross-border remittance
```

---

**Document Version:** 2.0
**Last Updated:** 2025-11-03
**Status:** Ready for review
**Share with:** Internal ARD team + Lightspark

---

## Summary

This business operations document provides:

1. ✅ Detailed fee structure (zero fees for internal, competitive for international)
2. ✅ Treasury management strategy (pre-funded BTC pool owned by ARD APP)
3. ✅ 🚨 V2.0: Immediate iDAX execution for inbound (eliminates price risk)
4. ✅ Comprehensive cost analysis (break-even in Month 6)
5. ✅ Risk management framework (VERY LOW risk with V2.0 improvements)
6. ✅ Daily operational procedures
7. ✅ 3-year financial projections ($8M+ profit over 3 years)

**Key V2.0 Improvements:**

1. **Inbound Lightning = Immediate iDAX Execution**
   - Zero price risk (BTC sold in <500ms)
   - Better user experience (immediate crediting)
   - Simpler operations (no reconciliation tracking)
   - Cost: Only 0.1% iDAX fee (~$60/month)

2. **Lower Operating Costs**
   - V1.0: $288,000/year
   - V2.0: $271,000/year
   - Savings: $17,000/year from simplified operations

3. **Higher Profit**
   - Year 1 profit: $110,000 (vs $93,000 in V1.0)
   - Improvement: $17,000 more profit in Year 1

**Key Takeaway:** The pre-funded BTC pool model (owned by ARD APP) combined with immediate iDAX execution for inbound transactions creates the optimal balance of cost efficiency, zero price risk, and excellent user experience.
