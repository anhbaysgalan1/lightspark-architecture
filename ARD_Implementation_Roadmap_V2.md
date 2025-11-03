# ARD Financial Group - Implementation Roadmap
## Cross-Border Remittance System

**Version:** 2.0
**Date:** 2025-11-03
**Timeline:** 18 weeks (4.5 months) to production launch
**Status:** Ready for review
**Share with:** Internal ARD team + Lightspark

---

## Executive Summary

This roadmap outlines the implementation of ARD's cross-border remittance system using Lightspark's Lightning Network infrastructure. The system will enable instant, low-cost money transfers between Mongolia and 140+ countries.

**Key Milestones:**
- Week 4: Internal ARD transfers (zero-fee, instant)
- Week 8: Outbound international transfers (Mongolia → World)
- Week 12: Inbound international transfers (World → Mongolia) 🚨 **with immediate idax execution**
- Week 16: External bank payouts (ACH, SEPA, PIX, etc.)
- Week 18: Production launch

**Resource Requirements:**
- Backend developers: 2 FTE
- Frontend developer: 1 FTE
- DevOps engineer: 1 FTE
- Compliance officer: 0.5 FTE (coordinates with ARD APP)
- CTO oversight
- Total team: 4.5 FTE

**Key V2.0 Updates:**
- 🚨 **Immediate idax execution** for inbound Lightning transactions (Week 10)
- Daily reconciliation for **outbound only** (Week 11)
- ARD APP owns BTC pool, Custody SaaS tracks in ledger
- Simplified technical approach (no complex code examples)

---

## Phase 1: Foundation & Setup (Weeks 1-2)

### Week 1: Project Kickoff & Infrastructure Setup

**Objectives:**
- Set up development environment
- Access Lightspark platform
- Establish project structure
- Initial team onboarding

**Tasks:**

**Day 1-2: Lightspark Account Setup**
- [ ] Sign up for Lightspark account at app.lightspark.com
- [ ] Complete onboarding and verification
- [ ] Access test environment (REGTEST mode)
- [ ] Generate API credentials (test mode)
- [ ] Review Lightspark documentation
- [ ] Join Lightspark community Slack

**Day 2-3: Development Environment**
- [ ] Set up Git repository
- [ ] Initialize backend project structure
- [ ] Configure environment variables
- [ ] Set up code quality tools
- [ ] Get production idax API credentials (direct integration)

**Day 4-5: Cloud Infrastructure**
- [ ] Set up cloud account
- [ ] Create VPC and subnets
- [ ] Set up managed database (test instance)
- [ ] Set up managed cache (test instance)
- [ ] Configure security groups
- [ ] Set up compute instances
- [ ] Install monitoring
- [ ] Set up CI/CD pipeline

**Deliverables:**
- ✅ Lightspark test account active
- ✅ Local development environment running
- ✅ Cloud infrastructure provisioned
- ✅ CI/CD pipeline configured
- ✅ Team onboarded with access

---

### Week 2: Database & Core Backend

**Objectives:**
- Design and implement database schema
- Set up core backend services
- Implement authentication

**Tasks:**

**Day 1-2: Database Design**
- [ ] Design database schema
- [ ] Create migration files
- [ ] Implement data models:
  - Users
  - Balances
  - Transactions
  - UMA addresses
  - Compliance logs
  - BTC pool balance (owned by ARD APP, tracked in ledger)
  - Reconciliation records
- [ ] Seed test data
- [ ] Write database access layer
- [ ] Implement connection pooling

**Day 3-4: Core Backend Services**
- [ ] Set up API server
- [ ] Implement API structure (RESTful)
- [ ] Create base service classes:
  - AuthService
  - AccountService
  - TransferService
  - LightsparkService (SDK wrapper)
  - iDAXService (for immediate execution + reconciliation)
- [ ] Implement error handling middleware
- [ ] Set up logging
- [ ] Configure CORS
- [ ] Implement rate limiting

**Day 5: Authentication & Authorization**
- [ ] Implement user registration
- [ ] Implement login (JWT tokens)
- [ ] Set up refresh tokens
- [ ] Implement password hashing
- [ ] Create auth middleware
- [ ] Implement role-based access control
- [ ] Write unit tests for auth

**Deliverables:**
- ✅ Database schema implemented
- ✅ Core backend services running
- ✅ Authentication working
- ✅ API endpoints documented

---

## Phase 2: Internal Transfers (Weeks 3-4)

### Week 3: Internal Transfer Implementation

**Objectives:**
- Implement zero-fee internal transfers
- Build transaction processing engine
- Implement balance management

**Tasks:**

**Day 1-2: Internal Transfer Logic**
- [ ] Implement internal transfer processing
- [ ] Implement atomic transactions (database level)
- [ ] Implement balance locking
- [ ] Implement balance validation
- [ ] Implement transaction state machine
- [ ] Handle concurrent transactions
- [ ] Write comprehensive unit tests

**Day 3-4: API Endpoints**
- [ ] POST /api/v1/transfers/internal
  - Request validation
  - User authentication
  - Balance check
  - Execute transfer
  - Return response
- [ ] GET /api/v1/account/balance
  - Query user balances
  - Return all currencies
- [ ] GET /api/v1/account/transactions
  - Query transaction history
  - Pagination
  - Filtering (type, status, date)
- [ ] GET /api/v1/transfers/:id/status
  - Real-time status updates

**Day 5: Testing & Integration**
- [ ] Write integration tests
- [ ] Test concurrent transfers
- [ ] Test error scenarios
- [ ] Performance testing (1000 TPS target)
- [ ] Load testing
- [ ] Fix bugs

**Deliverables:**
- ✅ Internal transfers working
- ✅ Transaction API complete
- ✅ Unit tests passing (>80% coverage)
- ✅ Integration tests passing

---

### Week 4: Frontend Integration & Testing

**Objectives:**
- Build mobile app UI for internal transfers
- Integrate with backend API
- User testing

**Tasks:**

**Day 1-2: Mobile App UI**
- [ ] Create "Send Money" screen
  - Recipient selection (UMA address search)
  - Amount input
  - Note/memo field
  - Preview screen
- [ ] Create "Transaction History" screen
  - List of transactions
  - Filter/search
  - Transaction details
- [ ] Create "Balance" widget
  - Display current balance
  - Refresh on pull
- [ ] Implement push notifications

**Day 3-4: API Integration**
- [ ] Integrate authentication API
- [ ] Integrate balance API
- [ ] Integrate transfer API
- [ ] Integrate transaction history API
- [ ] Implement error handling
- [ ] Implement loading states
- [ ] Implement optimistic updates

**Day 5: Testing & Polish**
- [ ] Internal team testing (dogfooding)
- [ ] Fix UI/UX issues
- [ ] Performance optimization
- [ ] Analytics integration
- [ ] Crash reporting

**Deliverables:**
- ✅ Mobile app with internal transfers
- ✅ Team using the app daily (dogfooding)
- ✅ Zero-fee instant transfers working
- ✅ Ready for Phase 3

**🎉 Milestone 1: Internal Transfers Live!**

---

## Phase 3: Outbound International (Weeks 5-8)

### Week 5: Lightspark Integration

**Objectives:**
- Integrate Lightspark SDK
- Implement BTC pool management
- Test Lightning payments

**Tasks:**

**Day 1-2: Lightspark SDK Integration**
- [ ] Install and configure Lightspark SDK
- [ ] Implement LightsparkService class
- [ ] Implement wallet management:
  - Get account balance
  - Get Lightning node info
  - Query payment status
- [ ] Implement payment methods:
  - Pay Lightning invoice
  - Create Lightning invoice
  - Decode invoice
- [ ] Implement webhook handling:
  - Set up webhook endpoint
  - Verify webhook signatures
  - Handle payment events
- [ ] Write unit tests

**Day 3-4: BTC Pool Management**
- [ ] Implement BTCPoolMonitor class
- [ ] Implement pool balance tracking (owned by ARD APP)
- [ ] Implement utilization calculation
- [ ] Implement alert system:
  - 50% utilization → Warning
  - 70% utilization → Alert to ARD APP
  - 85% utilization → Critical alert to ARD APP
- [ ] Integrate with alerting system
- [ ] Implement manual replenishment API
- [ ] Write tests

**Day 5: Testing Lightning Payments**
- [ ] Test Lightning invoice creation
- [ ] Test Lightning payment execution
- [ ] Test payment status queries
- [ ] Test webhook reception
- [ ] Test error handling (no route, timeout, etc.)
- [ ] Document findings

**Deliverables:**
- ✅ Lightspark SDK integrated
- ✅ BTC pool monitoring working
- ✅ Lightning payments tested successfully

---

### Week 6: idax Integration & Quote Engine

**Objectives:**
- Integrate with idax exchange
- Implement quote generation
- Test MNT/BTC conversion rates

**Tasks:**

**Day 1-2: idax API Integration**
- [ ] Implement iDAXService class
- [ ] Implement rate queries:
  - Get MNT/BTC spot rate
  - Get real-time quotes
  - Cache rates (60 second TTL)
- [ ] Implement trading methods:
  - 🚨 **Create market order (for immediate inbound execution)**
  - Query order status
  - Get account balance
- [ ] Implement withdrawal/deposit tracking (for daily reconciliation)
- [ ] Handle API errors and retries
- [ ] Write unit tests

**Day 3-4: Quote Generation Engine**
- [ ] Implement QuoteService class
- [ ] Implement multi-step quote generation:
  1. Query idax for MNT/BTC rate
  2. Query Lightspark for BTC/Foreign Currency rate
  3. Calculate total cost (amount + fees)
  4. Generate quote ID
  5. Set expiry (60 seconds)
  6. Store in cache
- [ ] Implement quote validation
- [ ] Implement quote expiry handling
- [ ] Calculate fees (tiered structure)
- [ ] Write tests

**Day 5: Testing & Optimization**
- [ ] Test quote generation end-to-end
- [ ] Test quote expiry
- [ ] Test rate updates
- [ ] Performance testing (target: <200ms)
- [ ] Load testing (1000 quotes/sec)
- [ ] Optimize caching strategy

**Deliverables:**
- ✅ idax integration complete
- ✅ Quote engine working
- ✅ Rate caching optimized

---

### Week 7: Outbound Transaction Implementation

**Objectives:**
- Implement outbound international transfers
- Use pre-funded BTC pool (owned by ARD APP)
- Handle transaction lifecycle

**Tasks:**

**Day 1-2: Transaction Orchestration**
- [ ] Implement OutboundTransferService class
- [ ] Implement transaction flow:
  1. Get quote (validated, not expired)
  2. Debit user MNT balance
  3. Reserve BTC from pre-funded pool
  4. Generate Lightning invoice (if UMA)
  5. Execute Lightning payment
  6. Monitor payment status
  7. Update transaction status
  8. Release/return BTC from pool
  9. Send notifications
- [ ] Implement error handling and rollback
- [ ] Implement timeout handling (5 min)
- [ ] Write comprehensive tests

**Day 3-4: UMA Protocol Support**
- [ ] Implement UMA address validation
- [ ] Implement LNURLP lookup
- [ ] Parse LNURLP response
- [ ] Extract supported currencies
- [ ] Handle compliance data
- [ ] Verify signatures
- [ ] Create pay request with compliance data
- [ ] Write tests

**Day 5: Integration & Testing**
- [ ] End-to-end testing (testnet)
- [ ] Test with real UMA addresses (Wallet of Satoshi testnet)
- [ ] Test error scenarios
- [ ] Test refunds
- [ ] Performance testing
- [ ] Fix bugs

**Deliverables:**
- ✅ Outbound transfers working
- ✅ UMA protocol implemented
- ✅ End-to-end tests passing

---

### Week 8: Outbound Frontend & Launch

**Objectives:**
- Build mobile UI for international transfers
- Launch outbound international feature (beta)
- Monitor and optimize

**Tasks:**

**Day 1-2: Mobile UI**
- [ ] Create "Send Internationally" screen
  - Country selection
  - Recipient UMA address or details
  - Amount input (MNT)
  - Preview with quote
  - Fee breakdown
  - Estimated delivery time
- [ ] Implement quote request
- [ ] Display real-time rates
- [ ] Show quote expiry countdown
- [ ] Implement confirmation flow
- [ ] Add transaction tracking

**Day 3: Testing & Beta Launch**
- [ ] Internal testing
- [ ] Fix critical bugs
- [ ] Deploy to production
- [ ] Enable for beta users (100 users)
- [ ] Monitor transactions closely
- [ ] Set up alerts

**Day 4-5: Monitoring & Optimization**
- [ ] Monitor success rates
- [ ] Monitor BTC pool utilization
- [ ] Monitor Lightning payment times
- [ ] Collect user feedback
- [ ] Fix issues
- [ ] Optimize user experience
- [ ] Prepare for wider rollout

**Deliverables:**
- ✅ Mobile app with outbound international
- ✅ Beta launch complete (100 users)
- ✅ Monitoring dashboards set up
- ✅ Initial feedback collected

**🎉 Milestone 2: Outbound International Live!**

---

## Phase 4: Inbound International (Weeks 9-12) 

### Week 9: UMA Server Implementation

**Objectives:**
- Implement UMA endpoints (receiving side)
- Set up .well-known/lnurlp endpoint
- Handle incoming Lightning payments

**Tasks:**

**Day 1-2: UMA Endpoint Setup**
- [ ] Implement LNURLP endpoint:
  ```
  GET /.well-known/lnurlp/:username
  ```
- [ ] Return LNURLP response with:
  - Callback URL
  - Supported currencies (MNT, USD, BTC)
  - Min/max amounts
  - Compliance data
  - KYC status (from ARD APP API)
  - Signature
- [ ] Implement signature generation
- [ ] Configure DNS for ard.mn
- [ ] Set up SSL certificate
- [ ] Test endpoint publicly

**Day 3-4: Pay Request Handler**
- [ ] Implement pay request endpoint:
  ```
  POST /api/v1/uma/payreq
  ```
- [ ] Parse pay request
- [ ] Validate signature
- [ ] Extract compliance data
- [ ] Perform OFAC screening (via Lightspark)
- [ ] Get KYC data from ARD APP API
- [ ] Calculate amount in BTC
- [ ] Create Lightning invoice
- [ ] Return invoice in response
- [ ] Write tests

**Day 5: Testing**
- [ ] Test with external UMA wallets
- [ ] Test from Wallet of Satoshi
- [ ] Test from Strike
- [ ] Test compliance data exchange
- [ ] Test various amounts and currencies
- [ ] Fix bugs

**Deliverables:**
- ✅ UMA server endpoints live
- ✅ Can receive payments from external wallets
- ✅ Compliance data exchange working

---

### Week 10: Inbound Payment Processing 

**Objectives:**
- Handle incoming Lightning payments
- 🚨 **IMMEDIATELY execute idax trade to convert BTC → MNT**
- Credit user accounts with MNT
- Implement webhook processing

**Tasks:**

**Day 1-2: Webhook Handler + Immediate idax Execution 🚨**
- [ ] Implement webhook endpoint for Lightspark
- [ ] Verify webhook signatures
- [ ] Handle payment received events
- [ ] Extract payment details (amount, sender, etc.)
- [ ] 🚨 **IMMEDIATELY call idax trading API:**
  - Execute market sell order (BTC → MNT)
  - Amount: Exact BTC received via Lightning
  - Type: MARKET (immediate execution)
  - Expected time: 200-500ms
- [ ] Receive MNT from idax trade
- [ ] Update ARD APP's BTC pool ledger
- [ ] Write tests

**Day 2-3: User Crediting Logic**
- [ ] Implement InboundTransferService class
- [ ] Receive MNT amount from idax execution
- [ ] Credit user MNT balance (atomic transaction)
- [ ] Create transaction record (status: COMPLETED)
- [ ] Send push notification to user
- [ ] Handle errors gracefully (retry idax call if needed)
- [ ] Write tests

**Day 4-5: Testing & Integration**
- [ ] End-to-end testing
- [ ] Send test payments from external wallets
- [ ] Verify user receives MNT correctly
- [ ] Verify idax execution happens immediately (<1 second)
- [ ] Test different amounts
- [ ] Test error scenarios (idax API down, etc.)
- [ ] Performance testing
- [ ] Fix bugs

**🚨 V2.0 KEY DIFFERENCE:**
```
OLD (V1.0): Receive BTC → Mark for daily reconciliation → Settle next day
            ❌ Price risk (12-24 hours)
            ❌ Delayed user crediting

NEW (V2.0): Receive BTC → IMMEDIATE idax execution (200-500ms) → Credit user
            ✅ Zero price risk
            ✅ Instant user crediting
            ✅ Simple operations
            ✅ Cost: Only 0.2% idax fee
```

**Deliverables:**
- ✅ Inbound payments working end-to-end
- ✅ Users credited with MNT automatically and IMMEDIATELY
- ✅ idax execution happening in <1 second
- ✅ Zero price risk achieved
- ✅ Notifications sent

---

### Week 11: Daily Reconciliation 🚨 V2.0 UPDATE

**Objectives:**
- Implement automated daily reconciliation
- 🚨 **For OUTBOUND transactions only** (not inbound)
- Settle with ARD APP
- Balance BTC pool

**Tasks:**

**Day 1-2: Reconciliation Service**
- [ ] Implement DailyReconciliationService class
- [ ] 🚨 Calculate yesterday's BTC flows (OUTBOUND ONLY):
  - Outbound Lightning payments (used from pool)
  - Bank payouts (used from pool)
  - Net BTC consumed from pool
  - **Note:** Inbound NOT included (already settled via immediate idax)
- [ ] Calculate yesterday's MNT flows:
  - MNT collected from users (outbound)
  - Track for reporting only
- [ ] Determine settlement needed
- [ ] Write tests

**Day 3-4: ARD APP Settlement Integration**
- [ ] Implement settlement coordination with ARD APP
- [ ] Request BTC replenishment from ARD APP (if pool deficit)
- [ ] Monitor blockchain confirmation
- [ ] Verify pool balance after settlement
- [ ] Handle settlement failures
- [ ] Retry logic
- [ ] Alert on discrepancies
- [ ] Write tests

**Day 5: Scheduling & Testing**
- [ ] Set up cron job (00:00 UTC daily)
- [ ] Test reconciliation with historical data
- [ ] Simulate various scenarios
- [ ] Test alert system
- [ ] Generate reconciliation reports
- [ ] Document procedures

**🚨 V2.0 KEY DIFFERENCE:**
```
OLD (V1.0): Reconcile both inbound and outbound daily
NEW (V2.0): Reconcile OUTBOUND ONLY
            - Inbound already settled immediately via idax
            - Simpler operations
            - Less tracking overhead
```

**Deliverables:**
- ✅ Daily reconciliation automated (outbound only)
- ✅ Settlement with ARD APP working
- ✅ Reports generated automatically

---

### Week 12: Inbound Frontend & Launch

**Objectives:**
- Display UMA address in app
- Add "Receive Money" feature
- Launch inbound international

**Tasks:**

**Day 1-2: Mobile UI**
- [ ] Create "Receive Money" screen
  - Display user's UMA address ($user@ard.mn)
  - QR code for UMA address
  - Share button
  - Instructions for senders
- [ ] Add "Incoming Transactions" section to history
- [ ] Add notifications for received payments
- [ ] Implement deep linking (UMA protocol)

**Day 3: Testing & Launch**
- [ ] Internal testing
- [ ] Test receiving from various wallets
- [ ] Verify immediate crediting works
- [ ] Fix critical bugs
- [ ] Deploy to production
- [ ] Enable for all users
- [ ] Monitor incoming transactions

**Day 4-5: Monitoring & Optimization**
- [ ] Monitor inbound success rates
- [ ] Monitor idax execution times (should be <1 second)
- [ ] Monitor MNT balance
- [ ] Verify immediate settlement working
- [ ] Collect user feedback
- [ ] Optimize

**Deliverables:**
- ✅ Users can receive international payments
- ✅ UMA addresses working globally
- ✅ Inbound international live with immediate execution
- ✅ Zero price risk achieved

**🎉 Milestone 3: Inbound International Live! (with V2.0 improvements)**

---

## Phase 5: Bank Payouts (Weeks 13-16)

### Week 13: Lightspark Grid API Integration

**Objectives:**
- Integrate Lightspark Grid API for bank payouts
- Implement off-ramp functionality
- Test ACH/SEPA/PIX

**Tasks:**

**Day 1-2: Grid API Setup**
- [ ] Access Lightspark Grid API documentation
- [ ] Generate Grid API credentials
- [ ] Implement GridService class
- [ ] Implement quote generation for off-ramps
- [ ] Implement quote validation
- [ ] Cache quotes
- [ ] Write tests

**Day 3-4: Off-Ramp Execution**
- [ ] Implement executeOffRamp()
- [ ] Handle payment tracking
- [ ] Implement webhook handling for Grid events
- [ ] Handle status updates:
  - Processing
  - Sent to bank
  - Completed
  - Failed
- [ ] Implement refund logic for failures
- [ ] Write tests

**Day 5: Testing**
- [ ] Test ACH (USA) payouts
- [ ] Test SEPA (Europe) payouts
- [ ] Test PIX (Brazil) payouts
- [ ] Test Mongolia domestic banks (real-time)
- [ ] Test various amounts
- [ ] Test error scenarios
- [ ] Document findings

**Deliverables:**
- ✅ Grid API integrated
- ✅ Off-ramp execution working
- ✅ Multiple payment methods tested

---

### Week 14: Bank Account Linking

**Objectives:**
- Implement bank account linking
- Store bank account details securely
- Verify bank accounts

**Tasks:**

**Day 1-2: Bank Linking Integration**
- [ ] Implement bank linking flow in mobile app
- [ ] Handle account verification
- [ ] Store account details encrypted
- [ ] Implement bank account CRUD APIs
- [ ] Write tests

**Day 3-4: Bank Account Management**
- [ ] Create "Linked Accounts" screen in app
- [ ] Allow users to add bank accounts
- [ ] Display linked accounts
- [ ] Allow users to remove accounts
- [ ] Verify accounts
- [ ] Handle errors
- [ ] Security review

**Day 5: Testing**
- [ ] Test bank linking flow
- [ ] Test with various banks
- [ ] Test Mongolia domestic banks
- [ ] Test error scenarios
- [ ] Security testing
- [ ] Fix bugs

**Deliverables:**
- ✅ Bank account linking working
- ✅ Accounts stored securely
- ✅ Verification working

---

### Week 15: Payout Transaction Implementation

**Objectives:**
- Implement full bank payout flow
- Handle multi-day processing (international)
- Handle real-time processing (Mongolia domestic)
- Status tracking

**Tasks:**

**Day 1-2: Payout Service**
- [ ] Implement BankPayoutService class
- [ ] Implement payout flow:
  1. Get quote from Grid API (or Mongolia bank API)
  2. Debit user MNT balance
  3. Reserve BTC from pool (if international)
  4. Execute payout
  5. Monitor status via webhooks
  6. Update transaction status
  7. Release BTC from pool (when confirmed)
  8. Send notifications
- [ ] Handle multi-day processing (international)
- [ ] Handle real-time processing (Mongolia domestic)
- [ ] Implement timeout handling
- [ ] Write tests

**Day 3-4: Status Tracking**
- [ ] Implement status polling (fallback)
- [ ] Update user on status changes
- [ ] Handle ACH delays (1-2 business days)
- [ ] Handle instant payment methods (FedNow, PIX)
- [ ] Handle Mongolia real-time transfers (instant to 2 hours)
- [ ] Notify user when funds delivered
- [ ] Write tests

**Day 5: Testing**
- [ ] End-to-end payout testing
- [ ] Test Mongolia domestic (real-time)
- [ ] Test ACH (2-day wait)
- [ ] Test instant methods
- [ ] Test failures and refunds
- [ ] Performance testing
- [ ] Fix bugs

**Deliverables:**
- ✅ Bank payouts working end-to-end
- ✅ Status tracking implemented
- ✅ Mongolia real-time transfers working
- ✅ Tests passing

---

### Week 16: Payout Frontend & Launch

**Objectives:**
- Build payout UI
- Launch bank payout feature
- Monitor and optimize

**Tasks:**

**Day 1-2: Mobile UI**
- [ ] Create "Withdraw to Bank" screen
  - Select linked bank account
  - Enter amount (MNT)
  - Select destination currency
  - Show quote (amount to receive)
  - Show fees and delivery time
  - Confirmation screen
- [ ] Add payout tracking to transaction history
- [ ] Show expected delivery date
- [ ] Add status updates

**Day 3: Testing & Launch**
- [ ] Internal testing
- [ ] Fix critical bugs
- [ ] Deploy to production
- [ ] Enable for beta users (50 users)
- [ ] Monitor closely

**Day 4-5: Monitoring & Optimization**
- [ ] Monitor payout success rates
- [ ] Monitor delivery times
- [ ] Track user satisfaction
- [ ] Collect feedback
- [ ] Optimize UX
- [ ] Fix issues

**Deliverables:**
- ✅ Bank payout UI complete
- ✅ Beta launch successful
- ✅ Ready for full launch

**🎉 Milestone 4: Bank Payouts Live!**

---

## Phase 6: Polish & Production Launch (Weeks 17-18)

### Week 17: Compliance & Security Audit

**Objectives:**
- Complete compliance review
- Security audit
- Performance optimization
- Bug fixes

**Tasks:**

**Day 1-2: Compliance Review**
- [ ] Review all KYC/AML procedures (coordinated with ARD APP)
- [ ] Test Travel Rule implementation
- [ ] Test OFAC screening
- [ ] Review audit logs
- [ ] Ensure all transactions logged
- [ ] Generate compliance reports
- [ ] Document procedures
- [ ] Get sign-off from compliance officer

**Day 3-4: Security Audit**
- [ ] Run penetration testing
- [ ] Code security review
- [ ] Dependency vulnerability scan
- [ ] API security review
- [ ] Encryption verification
- [ ] Access control audit
- [ ] Fix all critical and high issues
- [ ] Document findings

**Day 5: Performance Optimization**
- [ ] Database query optimization
- [ ] API response time optimization
- [ ] Caching strategy review
- [ ] Load testing (1000 concurrent users)
- [ ] Identify bottlenecks
- [ ] Fix performance issues
- [ ] Re-test

**Deliverables:**
- ✅ Compliance audit passed
- ✅ Security audit completed
- ✅ Performance optimized

---

### Week 18: Production Launch

**Objectives:**
- Final testing
- Production deployment
- Public launch
- Monitor and support

**Tasks:**

**Day 1: Final Testing**
- [ ] End-to-end testing (all features)
- [ ] User acceptance testing
- [ ] Smoke tests in staging
- [ ] Data migration (if needed)
- [ ] Backup verification
- [ ] Disaster recovery test

**Day 2: Production Deployment**
- [ ] Deploy backend to production
- [ ] Deploy mobile app to stores (App Store, Play Store)
- [ ] Enable production Lightspark credentials
- [ ] Coordinate with ARD APP for BTC pool setup
- [ ] Enable production idax API
- [ ] Enable production payment networks
- [ ] Verify all systems operational
- [ ] Enable monitoring and alerts

**Day 3: Soft Launch**
- [ ] Enable for 1,000 users (gradual rollout)
- [ ] Monitor closely for first 24 hours
- [ ] Fix any critical issues immediately
- [ ] Collect feedback
- [ ] Prepare for wider rollout

**Day 4: Public Launch**
- [ ] Enable for all ARD APP users
- [ ] Press release
- [ ] Social media announcement
- [ ] Email campaign to users
- [ ] In-app banners and tutorials
- [ ] Monitor transaction volume
- [ ] Customer support ready

**Day 5: Post-Launch**
- [ ] Monitor KPIs:
  - Transaction volume
  - Success rates
  - User feedback
  - System performance
  - idax execution times (inbound)
  - BTC pool utilization
- [ ] Address any issues
- [ ] Collect user feedback
- [ ] Plan improvements
- [ ] Celebrate! 🎉

**Deliverables:**
- ✅ Production deployment successful
- ✅ Public launch complete
- ✅ Users actively using the system
- ✅ All monitoring in place
- ✅ V2.0 immediate execution model working perfectly

**🎉🎉🎉 LAUNCH COMPLETE! 🎉🎉🎉**

---

## Post-Launch: Continuous Improvement (Ongoing)

### Month 1 Post-Launch

- [ ] Daily monitoring of all KPIs
- [ ] Verify immediate idax execution working smoothly
- [ ] Monitor price risk (should be zero)
- [ ] Collect and analyze user feedback
- [ ] Fix bugs reported by users
- [ ] Optimize based on real usage patterns
- [ ] Add requested features (prioritize)
- [ ] Marketing campaigns to drive adoption
- [ ] Monitor break-even progress

### Month 2-3 Post-Launch

- [ ] Reach break-even (target: Month 6)
- [ ] Expand marketing efforts
- [ ] Add new currencies/corridors
- [ ] Improve conversion rates
- [ ] Reduce transaction failures
- [ ] Enhance user experience
- [ ] Build partnerships with other VASPs

### Month 4-6 Post-Launch

- [ ] Scale infrastructure for growth
- [ ] Expand team if needed
- [ ] Add advanced features:
  - Recurring payments
  - Payment requests
  - Bill payments
  - Business accounts
- [ ] Explore new markets
- [ ] Consider licensing/partnerships

---

## Resource Allocation

### Team Structure

**Backend Team:**
- Lead Backend Developer (1 FTE) - Weeks 1-18
- Backend Developer (1 FTE) - Weeks 1-18

**Frontend Team:**
- Mobile Developer (1 FTE) - Weeks 4-18

**DevOps:**
- DevOps Engineer (1 FTE) - Weeks 1-18

**Compliance:**
- Compliance Officer (0.5 FTE) - Weeks 9-18
- Coordinates with ARD APP's KYC/AML team

**Leadership:**
- CTO - Oversight & architecture - Weeks 1-18

**Total: 4.5 FTE**

### Budget Estimate

**Development Costs (18 weeks):**
- Backend developers: $36,000
- Frontend developer: $13,500
- DevOps engineer: $18,000
- Compliance officer: $9,000
- **Total Personnel: $76,500**

**Infrastructure (4.5 months):**
- Cloud infrastructure: ~$10,000
- Third-party services: ~$3,000
- **Total Infrastructure: ~$13,000**

**One-time Costs:**
- Security audit: $5,000
- Legal review: $3,000
- **Total One-time: $8,000**

**TOTAL DEVELOPMENT BUDGET: ~$97,500**

### ROI Timeline (V2.0 Improved)

- Month 6: Break-even ($22,580/month revenue - lower costs in V2.0)
- Month 12: $70K/month revenue
- Development cost recovered in Month 7
- Year 1 profit: $110,000 (V2.0 improvement: +$17K vs V1.0)
- Year 2 profit: $2,080,000
- Year 3 profit: $5,870,000

**3-year ROI: 8,262%** 🚀

---

## Risk Mitigation

### Technical Risks

1. **Lightspark API issues**
   - Mitigation: Start testing early (Week 5)
   - Fallback: Queue transactions for retry
   - Support: Lightspark SLA and support channels

2. **idax integration issues** 🚨 V2.0 CRITICAL
   - Mitigation: Test immediate execution early with production API
   - Since ARD owns idax: Direct communication channel and production access
   - Monitor: idax API uptime and execution times
   - Backup: Alternative exchange (Binance, etc.) if needed

3. **Lightning routing failures**
   - Mitigation: Lightspark handles routing automatically
   - Success rate: >99.5%
   - Fallback: Automatic refund

### Business Risks

1. **Low adoption**
   - Mitigation: Zero fees for internal transfers
   - Marketing: In-app promotions, referral program
   - Advantage: ARD APP existing user base

2. **Regulatory changes**
   - Mitigation: Build compliant from day 1
   - Engage regulators early
   - Flexible architecture to adapt

3. **Competition**
   - Mitigation: First-mover advantage in Mongolia
   - Competitive pricing (80-95% cheaper than Western Union)
   - Superior UX (instant vs days)

---

## Success Criteria

### Week 4 (Phase 1 Complete)
- ✅ 100+ internal transfers per day
- ✅ Zero transaction failures
- ✅ <100ms average transaction time
- ✅ Team actively using the app

### Week 8 (Phase 2 Complete)
- ✅ 50+ outbound international transfers per day
- ✅ >99% success rate
- ✅ <10 second average transaction time
- ✅ BTC pool management working smoothly

### Week 12 (Phase 3 Complete) 🚨 V2.0 ENHANCED
- ✅ 20+ inbound international transfers per day
- ✅ Daily reconciliation working (outbound only)
- ✅ idax execution happening in <1 second for inbound
- ✅ Zero price risk achieved
- ✅ Users receiving money from abroad successfully

### Week 16 (Phase 4 Complete)
- ✅ 10+ bank payouts per day
- ✅ Multiple payment methods working (ACH, SEPA, PIX)
- ✅ Mongolia real-time transfers working
- ✅ Users successfully cashing out to banks

### Week 18 (Launch)
- ✅ 200+ transactions per day (all types)
- ✅ >99.5% overall success rate
- ✅ 99.95% system uptime
- ✅ 1,000+ active users
- ✅ Positive user reviews (>4.5 stars)

### Month 6 Post-Launch
- ✅ Break-even achieved
- ✅ 10,000+ transactions per month
- ✅ 20,000+ active users
- ✅ Market leader in Mongolia cross-border remittance

---

## Conclusion

This 18-week roadmap provides a detailed, actionable plan to launch ARD's cross-border remittance system. By following this phased approach, we ensure:

1. ✅ **Incremental delivery** - Each phase delivers value
2. ✅ **Risk mitigation** - Test and validate before moving forward
3. ✅ **Team efficiency** - Clear tasks and responsibilities
4. ✅ **Quality assurance** - Testing built into every phase
5. ✅ **Business success** - Break-even by Month 6, profitable Year 1

### Key V2.0 Innovations

**The immediate idax execution model (Week 10) is the critical improvement that enables:**
- ✅ **Zero price risk** - BTC sold in <1 second, not held for 12-24 hours
- ✅ **Better UX** - Users credited immediately upon receipt
- ✅ **Simpler operations** - No inbound reconciliation tracking needed
- ✅ **Lower costs** - Only 0.2% idax fee vs potential 1-5% price volatility exposure
- ✅ **Higher profit** - $110K Year 1 profit (vs $93K in old model)

**The pre-funded BTC pool model (owned by ARD APP) enables:**
- ✅ 99.2% cost reduction vs real-time settlement
- ✅ Instant user experience (5-10 seconds)
- ✅ Simple operations (daily reconciliation for outbound only)
- ✅ Scalability to millions of transactions

**Let's build the future of money movement in Mongolia! 🚀**

---

**Document Version:** 2.0
**Last Updated:** 2025-11-03
**Status:** Ready for review
**Share with:** Internal ARD team + Lightspark
**Next Review:** Weekly during implementation
