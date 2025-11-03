# ARD Cross-Border Remittance - Quick Reference Guide
## Version 2.0 - Critical Updates Applied

**Last Updated:** 2025-11-03
**For:** Internal Team + Lightspark Review

---

## 🎯 System Overview

**What we're building:**
Cross-border remittance system for ARD APP using Lightspark's Lightning Network

**Core capabilities:**
1. Internal ARD transfers (zero-fee, instant)
2. Send money internationally (Mongolia → World)
3. Receive money internationally (World → Mongolia)
4. Cash out to banks (Mongolia + International)

---

## 🏢 System Roles

### ARD APP
- **User-facing** mobile/web application
- **Owns** BTC liquidity pool (10-20 BTC)
- **Manages** KYC, user data, funds
- **Provides** user information to Custody SaaS via API

### Custody SaaS
- **Backend ledger** platform (like exchange backend)
- **Tracks** all balances and transactions
- **Integrates** with Lightspark and idax
- **Does NOT** store KYC (gets from ARD APP)

### idax Exchange
- **Provides** trading API only
- **Executes** MNT/BTC conversions
- **Charges** ~0.2% trading fee
- **Minimal role** - just execution layer

### Lightspark
- **Provides** managed Lightning Network infrastructure
- **Handles** all routing, liquidity, compliance
- **Grid API** for international bank payouts
- **No Lightning node setup needed** on our side

---

## 🔄 Transaction Flows

### Type A: Internal ARD Transfer (Zero Fee)
```
Anhaa (Mongolia) → Bat (Mongolia)
Amount: 50,000 MNT

Flow:
1. Anhaa confirms in ARD APP
2. Custody SaaS updates ledger
   - Debit Anhaa: -50,000 MNT
   - Credit Bat: +50,000 MNT
3. Both users notified
4. DONE

Time: < 100ms
Fee: 0 MNT (FREE)
Network: None (ledger only)
```

### Type B: Outbound International
```
Anhaa (Mongolia) → Battulga (USA)
Amount: 100,000 MNT → ~$29 USD

Flow:
1. Anhaa requests transfer in ARD APP
2. Get quote (MNT→BTC→USD rates)
3. Anhaa confirms
4. Debit Anhaa: 101,000 MNT (including fee)
5. Use ARD's pre-funded BTC pool
6. Send via Lightning Network
7. Battulga receives $29 USD
8. DONE

Time: 5-10 seconds
Fee: 1,000 MNT (1%)
Cost to ARD: ~$0.05 Lightning fee
```

### Type C: Inbound International ⚠️ CRITICAL FLOW
```
Battulga (USA) → Anhaa (Mongolia)
Amount: $100 USD → ~66,334 MNT

Flow:
1. Battulga sends to $anhaa@ard.mn
2. Lightning payment arrives (0.00166 BTC)
3. 🚨 IMMEDIATELY execute idax sell order
4. Receive 66,334 MNT from idax
5. Credit Anhaa: +66,334 MNT
6. Update ARD's BTC pool ledger
7. DONE

Time: 3-6 seconds
Fee to user: Built into conversion rate
Cost to ARD: 0.1% idax fee (~$0.02)
Key: NO DAILY RECONCILIATION (immediate settlement)
```

**Why immediate execution?**
- ✅ Zero price risk
- ✅ Funds available immediately
- ✅ Simple operations
- ✅ Better user experience

### Type D: Bank Payout

**Mongolia Banks (Real-Time):**
```
Anhaa's ARD balance → Mongolia bank account
Amount: 100,000 MNT

Time: Instant to 2 hours
Fee: ~500 MNT
Method: Domestic bank transfer
```

**International Banks:**
```
Bold's ARD balance → US bank account
Amount: 100,000 MNT → ~$29 USD

Time: 1-2 business days (ACH)
Fee: ~2,000 MNT (2%)
Method: Lightspark Grid API → ACH/SEPA/PIX
```

---

## 💰 Fee Structure

| Transaction Type | Fee | Speed |
|-----------------|-----|-------|
| Internal ARD transfer | **0% (FREE)** | < 100ms |
| Outbound international | 0.5-1.5% | 5-10 sec |
| Inbound international | 0.5-1.0% (in rate) | 3-6 sec |
| Mongolia bank payout | ~500 MNT | Instant-2h |
| International payout | 1.5-2.5% | 1-2 days |

---

## 🎨 Branding

**Domain:** ard.mn
**App Name:** ARD APP
**UMA Address Format:** $username@ard.mn

**Example:**
- User "Anhaa" has UMA address: **$anhaa@ard.mn**
- Anyone globally can send to this address

---

## 🔑 Key Technical Points

### Pre-Funded BTC Pool
- **Owned by:** ARD APP
- **Size:** 10-20 BTC (adjustable based on volume)
- **Purpose:** Instant outbound transactions
- **Managed by:** ARD APP treasury team
- **Tracked by:** Custody SaaS ledger

### Immediate idax Execution (Inbound)
- **When:** Lightning payment received
- **Action:** Immediately sell BTC for MNT on idax
- **Why:** Eliminates price risk, better UX
- **Cost:** ~0.2% trading fee
- **Time:** 200-500ms execution

### Daily Reconciliation
- **For:** Outbound transactions only (using pre-funded pool)
- **Not for:** Inbound transactions (already settled immediately)
- **Frequency:** Once per day (00:00 UTC)
- **Purpose:** Replenish BTC pool from ARD APP
- **Cost:** Minimal (one blockchain transaction)

### No Lightning Node Required
- **Lightspark provides:** Fully managed Lightning nodes
- **We just:** Integrate via API
- **No need to:** Understand Lightning Network internals
- **No need to:** Manage channels, liquidity, routing

---

## 📊 Economics

### Cost Per Transaction (to ARD):

**Internal Transfer:**
- Cost: $0.00 (ledger only)
- Revenue: $0.00 (free to users)
- Strategy: Drive adoption

**Outbound International:**
- Cost: ~$0.05 (Lightning fee)
- Revenue: 1% of amount = ~$0.29 (on $29 TX)
- Profit: ~$0.24 per transaction

**Inbound International:**
- Cost: ~$0.07 (Lightning + idax fee)
- Revenue: 0.8% markup = ~$0.23 (on $29 TX)
- Profit: ~$0.16 per transaction

**Bank Payout:**
- Cost: ~$1.00 (Grid API + network)
- Revenue: 2% of amount = ~$0.58 (on $29 TX)
- Profit: Revenue covers cost (break-even to slight loss)
- Strategy: User retention (let them cash out easily)

### Break-Even:
- Monthly costs: ~$24,000
- Need: ~17,000 transactions/month
- Expected: Month 6

---

## 🕙 Implementation Timeline

**Total:** 18 weeks to launch

### Key Milestones:

**Week 4:** Internal ARD transfers live (dogfooding)
**Week 8:** Outbound international (beta - 100 users)
**Week 12:** Inbound international (full UMA support)
**Week 16:** Bank payouts (domestic + international)
**Week 18:** Public launch 🎉

---

## ⚡ Quick Wins

### Compared to Traditional Remittance:

| Feature | Traditional | ARD Remittance |
|---------|------------|----------------|
| **Cost** | 5-10% | 0.5-1.5% |
| **Speed** | 3-5 days | 5-10 seconds |
| **Availability** | Business hours | 24/7/365 |
| **User Experience** | Complex forms | Simple app |

**Result:** 80-95% cheaper + 1000x faster

### Compared to Running Own Lightning Node:

| Aspect | Own Node | Lightspark |
|--------|----------|-----------|
| **Setup** | Weeks of work | API integration |
| **Maintenance** | 24/7 monitoring | Fully managed |
| **Liquidity** | Manual management | Automatic |
| **Routing** | Complex | AI-optimized |
| **Cost** | High (staff + infra) | Pay-per-use |

**Result:** Focus on business, not infrastructure

---

## 🎯 Success Metrics

### Launch (Week 18):
- 200+ transactions/day
- 1,000+ active users
- >99.5% success rate
- 99.95% uptime

### Month 6:
- Break-even ($24K/month revenue)
- 10,000+ transactions/month
- 20,000+ active users

### Year 1:
- $381K revenue
- $93K profit
- Market leader in Mongolia

---

## 📞 Contact Points

### For Technical Questions:
- Lightspark Documentation: docs.lightspark.com
- Lightspark Support: support@lightspark.com

### For Implementation:
- CTO: Technical architecture oversight
- Backend Team: Custody SaaS development
- Frontend Team: ARD APP integration
- DevOps: Infrastructure setup

---

## ✅ Pre-Launch Checklist

### ARD APP:
- [ ] User registration and KYC flow
- [ ] BTC pool funded (initial 10 BTC)
- [ ] API endpoints for Custody SaaS
- [ ] Push notification setup

### Custody SaaS:
- [ ] Lightspark account and API keys
- [ ] idax trading API integration
- [ ] Ledger system implemented
- [ ] Webhook handlers set up

### Testing:
- [ ] Internal transfers (zero-fee)
- [ ] Outbound Lightning (testnet)
- [ ] Inbound Lightning (testnet)
- [ ] Bank payouts (sandbox)

### Compliance:
- [ ] KYC provider integrated (in ARD APP)
- [ ] AML screening process
- [ ] OFAC screening (via Lightspark)
- [ ] Travel Rule support

### Operations:
- [ ] Monitoring dashboards
- [ ] Alert system (PagerDuty)
- [ ] 24/7 on-call rotation
- [ ] Incident response procedures

---

## 🔴 Most Important Points

1. **Inbound Lightning = IMMEDIATE idax execution** (not daily reconciliation)
2. **ARD APP owns BTC pool** (Custody just tracks in ledger)
3. **idax is minimal role** (just trading API)
4. **No Lightning node needed** (Lightspark handles everything)
5. **Zero-fee internal transfers** (drive adoption)
6. **Mongolia banks are real-time** (not 1-2 days)

---

**This is the quick reference. For full details, see the complete documentation package.**

**Questions?** Review the full technical architecture document or contact the project team.

---

**Version:** 2.0
**Status:** Ready for review
**Share with:** Internal team + Lightspark
