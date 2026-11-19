
# 20 · FINTECH & PAYMENTS

## 20.1 Ecosystem

### Ecosystem Fundamentals

**The Five Players:**

| Player | Role | Moves | Revenue |
|--------|------|-------|---------|
| **Issuer** | Bank/fintech that gave you the card; authorizes transactions, manages credit risk | Credit | Interchange fees, interest, FX spreads |
| **Acquirer** | Bank/payment company enabling merchant to accept cards; provides merchant accounts, processes auth, deposits funds | Funds | Processing & settlement fees, risk tools |
| **Card Network** | Visa/Mastercard/Amex; connects issuers and acquirers, defines rules, carries authorization messages | Trust | Scheme fees, cross-border charges, dispute assessments |
| **Payment Gateway** | Secure portal routing data to acquirer; encrypts, checks fraud, ensures PCI/3DS compliance | Data | Platform & per-transaction fees |
| **Payment Aggregator** | Handles everything under one umbrella for smaller merchants; acts as Merchant of Record | Access | Higher transaction margins |

**Flow of a card transaction:**
1. Cardholder presents card at merchant
2. Merchant POS sends to acquirer (via gateway/processor)
3. Acquirer sends to card network
4. Network routes to issuer
5. Issuer approves/declines
6. Response returns via same path
7. Settlement happens later (T+1 or T+2)

**Recall:** *Issuer moves credit, Acquirer moves funds, Network moves trust, Gateway moves data.*

**SOTA:** Aggregators like Stripe, Paddle, PayU blurring lines; embedded finance collapsing the stack.

### Card Networks

**Three-Party:** Amex (issuer + network + acquirer combined).
**Four-Party:** Visa/Mastercard (issuer, acquirer, network, cardholder/merchant).

**Interchange fees:** Paid by acquirer to issuer. Varies by card type, merchant category, CP vs CNP, region.

**Scheme fees:** Paid to network.

**Assessment fees:** Cross-border, dispute.

**Recall:** *Network doesn't lend or store money — it moves trust and data.*

### Mastercard Certification

**Why it matters:** Every switch, POS, and acquiring system must be certified before live traffic.

**Requirements:**
- ISO 8583 understanding
- EMV cryptograms
- MACing (Message Authentication Code)
- Clearing/settlement procedures
- MIP (Mastercard Interface Processor) connectivity

**Process:** Test cases → simulator → live certification.

**Recall:** *Certify before live traffic.*

---

## 20.2 Standards & Protocols

### ISO 8583

**Definition:** Near-universal messaging format for card payments.

**Structure:**
- **MTI (Message Type Indicator):** 4 digits
  - Position 1: Version (0 = 1987, 2 = 1993, 3 = 2003)
  - Position 2: Message class (1 = auth, 2 = financial, 4 = reversal, 8 = network)
  - Position 3: Function (0 = request, 1 = response, 2 = advice)
  - Position 4: Origin (0 = acquirer, 1 = acquirer repeat, 2 = issuer, 4 = other)
- **Bitmaps:** Primary (fields 1–64), secondary (65–128), tertiary (129–192)
- **Data elements:** Actual fields

**Common MTIs:**
| MTI | Meaning |
|-----|---------|
| 0100 | Authorization request |
| 0110 | Authorization response |
| 0200 | Financial request |
| 0210 | Financial response |
| 0400 | Reversal request |
| 0410 | Reversal response |
| 0800 | Network management request |
| 0810 | Network management response |

**Key data elements:**
| Field | Name | Description |
|-------|------|-------------|
| 2 | PAN | Primary Account Number |
| 3 | Processing code | Transaction type |
| 4 | Amount | Transaction amount |
| 7 | Transmission date/time | MMDDhhmmss |
| 11 | STAN | System Trace Audit Number |
| 12 | Local time | hhmmss |
| 13 | Local date | MMDD |
| 22 | POS entry mode | 05=ICC, 07=contactless, 02=magstripe |
| 32 | Acquiring institution ID | |
| 37 | RRN | Retrieval Reference Number |
| 39 | Response code | 00=approved |
| 41 | Terminal ID | |
| 42 | Merchant ID | |
| 49 | Currency code | ISO 4217 |
| 52 | PIN block | Encrypted PIN |

**Response codes:**
| Code | Meaning |
|------|---------|
| 00 | Approved |
| 01 | Refer to card issuer |
| 05 | Do Not Honor |
| 12 | Invalid transaction |
| 13 | Invalid amount |
| 14 | Invalid card number |
| 41 | Lost card |
| 43 | Stolen card |
| 51 | Insufficient Funds |
| 54 | Expired Card |
| 55 | Incorrect PIN |
| 57 | Transaction not permitted |
| 61 | Exceeds withdrawal limit |
| 65 | Exceeds withdrawal frequency |
| 91 | Issuer unavailable |

**Recall:** *MTI + Bitmaps + Data Elements.*

### ISO 20022

**Definition:** XML-based messaging standard; richer data than ISO 8583.

**Messages:**
- **pacs.008:** Customer credit transfer
- **pacs.002:** Payment status report
- **camt.053:** Bank statement
- **camt.054:** Debit/credit notification

**Migration:** SWIFT MT → ISO 20022; domestic schemes adopting.

**Benefits:** Structured data, richer remittance info, easier compliance.

**Recall:** *Large-value settlement path; XML-based, richer data.*

### Key Data Elements

**Bit 2 (PAN):** Primary Account Number; 12–19 digits.

**Bit 3 (Processing code):** 6 digits; transaction type + account type.

**Bit 4 (Amount):** Transaction amount; 12 digits.

**Service Entry Mode:**
| Code | Meaning |
|------|---------|
| 02 | Magnetic stripe |
| 05 | ICC (EMV chip) |
| 07 | Contactless EMV |
| 80 | Fallback |
| 81 | E-commerce |

**Recall:** *Bit 2 (PAN), Bit 3 (processing code), Bit 4 (amount).*

### Response Codes

**00:** Approved.
**05:** Do Not Honor.
**51:** Insufficient Funds.
**54:** Expired Card.
**55:** Incorrect PIN.

**Recall:** *00 Approved, 05 Do Not Honor, 51 Insufficient Funds, 54 Expired, 55 Incorrect PIN.*

### Dual vs Single Message

**Dual message:** Credit cards, signature debit (Visa, Mastercard).
- Authorization (x1xx) → real-time
- Clearing (x2xx) → batch files
- Settlement later

**Single message (SMS):** PIN debit, ATM (Maestro, Pulse).
- Transaction (x2xx) only
- All real-time

**Recall:** *Dual = auth then clear; Single = one shot.*

---

## 20.3 Card Technology

### EMV Chip

**Definition:** Global standard for secure card payments (1996).

**How it works:**
1. Card inserted into POS → terminal communicates with chip
2. Authentication method used:
   - **SDA (Static Data Authentication):** Digital signature verifies data unchanged; vulnerable to cloning
   - **DDA (Dynamic Data Authentication):** Unique cryptographic value per transaction; harder to clone
   - **CDA (Combined Data Authentication):** DDA + transaction data signing; most secure
3. Cardholder verification (PIN or signature)
4. Chip generates **single-use, unique cryptogram** sent to issuer
5. Issuer verifies cryptogram → approve/decline

**Why it matters:** Magnetic stripe data is static, unencrypted, skimmable. EMV data is dynamic, unique per transaction, encrypted.

**AID:** Application Identifier; identifies payment application.

**CVM:** Cardholder Verification Method; PIN, signature, no CVM.

**Recall:** *EMV = dynamic cryptogram per transaction, not static stripe data.*

**SOTA:** Contactless EMV (NFC), mobile wallets (Apple Pay, Google Pay) use tokenized EMV with device-specific cryptograms.

### POS & Acceptance

**Terminal types:**
- **Countertop:** Fixed; retail
- **Mobile:** Portable; restaurant
- **Integrated:** Built into POS system
- **SoftPOS:** Smartphone/tablet as terminal (NFC)
- **Virtual terminal:** Online; manual entry

**Acceptance flow:**
```
POS → Acquirer → Network → Issuer → (back)
```

**Card Present (CP) vs Card Not Present (CNP):**
- **CP:** Lower fraud risk, lower interchange
- **CNP:** Higher fraud risk, higher interchange, 3DS authentication required

**Recall:** *CP = chip/PIN/contactless; CNP = e-commerce, 3DS.*

### Tokenization

**Definition:** Replace PAN with surrogate value (token).

**Process:**
1. PSP tokenizes card details during checkout
2. Token stored instead of PAN
3. Detokenization at network level during authorization

**Benefits:** Reduced PCI scope, secure storage, mobile wallets.

**Network tokenization:** Visa Token Service, Mastercard Digital Enablement Service.

**Recall:** *PAN → surrogate token.*

---

## 20.4 Processing & Routing

### Payment Switches

**Definition:** Real-time routing system; "traffic controller" of payment transactions.

**What a switch does:**
- Transaction routing
- Message formatting and translation
- Connectivity with networks and issuers
- High-speed, real-time processing

**What a switch does NOT do:**
- Decide approvals/declines
- Hold balances or accounts
- Define card rules or limits

**Core components:**
| Component | Function |
|-----------|----------|
| Inbound Interface | Accepts payment requests from gateway/POS/mobile |
| Rules Engine | Evaluates card type, currency, country, amount, processor performance |
| Connector Modules | Manages integrations with each processor/network |
| Message Translator | Re-formats fields between different systems |
| Security Layer | Encryption, tokenization, PCI DSS compliance, key management |
| Monitoring | Real-time dashboards, approval rates, latency, decline codes |

**Routing logic factors:**
- Payment method, card network, BIN, issuer, acquirer
- Merchant, country, currency, transaction type/amount
- Terminal/channel, provider availability, configured rules

**Least-cost routing:** Routes payments through local partners to be treated as domestic, lowering interchange fees and speeding settlement. Smart routing can raise approval rates from 85–95% global average to higher.

**Recall:** *Switch = routing layer; gateway = connection layer; processor = technical processing layer.*

**SOTA:** Intelligent routing with real-time performance data, automatic failover when processors timeout, AI-driven cost optimization.

### Issuer Decisioning

**Issuer Processors:** Provide connectivity with networks, card management, and payment processing. Remove the need for issuers to build card capabilities or connect to networks directly.

**Decisioning checks:**

| Category | Question | Example Checks |
|----------|----------|----------------|
| **Card Payment** | Is request legitimate? | Card expiry match, CVV match, PIN match, cryptographic match, 3DS, fraud decisioning |
| **Card** | Is card usable? | Card existence, block, expiration, state, token state, controls |
| **Account** | Is account usable? | Block, controls, balance check |
| **Product** | Does product allow use? | Product-level controls |
| **Other** | Additional checks | SKU checks, velocity limits |

**Risk checks:** Velocity, geolocation, device fingerprinting, ML scoring, rules.

**Recall:** *Issuer decisioning = card + account + product + payment legitimacy.*

---

## 20.5 Rails & Settlement

### Real-Time Rails

**SEPA Instant (EU):** 10-second hard limit; bank-centric; pan-regional consistency.
**FedNow (US):** Domestic resilience; multiple rails; different settlement models.
**RTP (US):** Real-time payments network.
**Pix (Brazil):** Highly centralised; alias directory; QR-first; mass adoption.
**UPI (India):** Clear separation of rail and overlays; push + pull; mandates and QR above rail; open participation.

**EU Instant Payments Regulation (IPR):** Payer PSP, payee PSP, and clearing/settlement mechanism have combined 9 seconds to credit payee's account — inside the 10-second hard limit.

**Cross-border gap:** Domestic instant schemes are "fast islands" with no native bridges. Swift connects 11,000+ institutions across 200+ countries; ISO 20022 migration harmonized messaging — but real-time settlement across that reach is still missing.

**Trust deficit:** Less than half of consumers feel real-time payments are safer than credit cards. EBA reports fraud risk for instant transfers up to 10× higher than standard transactions due to shortened settlement cycles and irrevocability.

**Recall:** *Domestic = solved speed; cross-border = unsolved trust + settlement.*

### Cross-Border

**Swift:** Global messaging network; 11,000+ institutions; 200+ countries. ISO 20022 migration. gpi transparency.

**Alternatives:**
- **Visa B2B Connect:** 190+ countries; 160 currencies
- **Mastercard Move:** 180+ countries; 150+ currencies
- **RippleNet:** Fintech multi-currency network
- **Stablecoin rails:** 24/7 settlement; capital efficiency
- **CIPS:** China's RMB cross-border system

**Selection framework:**
1. Speed sensitivity — instant vs multi-day
2. Corridor and currency — single country vs many markets
3. Capital efficiency — prefunded accounts vs on-demand liquidity
4. Compliance burden — own licenses vs partner

**Recall:** *No single system replaces Swift — most businesses route across multiple rails by corridor.*

---

## 20.6 Security & Compliance

### Security & Compliance

**PCI DSS:** Payment Card Industry Data Security Standard; mandatory for any entity storing, processing, or transmitting card data.

**3D Secure (3DS):** Additional authentication layer for CNP transactions. 3DS2 uses risk-based authentication and biometrics.

**Tokenization:** Replaces PAN with surrogate value.

**Encryption:** PIN blocks, key management (HSM, KMS), TLS for transmission.

**Fraud prevention:**
- Velocity checks
- Geolocation
- Device fingerprinting
- Machine learning scoring
- Rules engine

**Recall:** *PCI DSS + 3DS + tokenization = card data protection triad.*

---

## 20.7 Emerging Domains

### Emerging Domains

**Open Banking:** APIs allowing third-party access to bank data (PSD2 in EU, CMA in UK). Enables A2A payments, account aggregation, lending.

**Embedded Finance:** Financial services integrated into non-financial platforms (Shopify payments, Uber wallets).

**BNPL (Buy Now Pay Later):** Installment payments at checkout (Klarna, Afterpay). Credit risk shifted to BNPL provider.

**CBDCs (Central Bank Digital Currencies):** Government-issued digital currency. Retail vs wholesale. Examples: China's e-CNY, Sweden's e-Krona.

**Stablecoins:** Crypto pegged to fiat (USDC, USDT). Used for 24/7 settlement and cross-border efficiency.

**Recall:** *Open banking = data access; embedded finance = distribution; BNPL = credit at POS.*

### Design Patterns

**Idempotency:** Every payment request must have a unique key to prevent duplicate charges on retry.

**State Machine:** Payment lifecycle: initiated → authorized → captured → settled → refunded/charged back.

**Saga Pattern:** Distributed transaction across payment, inventory, and shipping services.

**Event Sourcing:** All payment state changes as immutable events — audit trail, replay capability.

**Circuit Breaker:** Fail fast when processor is down, route to alternate.

**Recall:** *Idempotency + state machine + saga = payment system reliability.*

**SOTA (2024–2025):** Real-time cross-border, SoftPOS, AI fraud detection (LangGraph + pgvector RAG pipelines), A2A overlays (Wero), programmable payments, EU IPR, UK APP fraud reimbursement, FedNow expansion, durable workflow orchestration (Temporal replacing hand-rolled sagas).

---
