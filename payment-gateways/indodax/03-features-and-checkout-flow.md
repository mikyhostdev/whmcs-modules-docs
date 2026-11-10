[🏠 Documentation Home](../../README.md) › [💳 Payment Gateways](../README.md) › [Indodax Crypto Pay](./README.md) › **03 — Features, Smart QR Code & Anti-Collision Flow**

---

# 03 — Features, Smart QR Code & Anti-Collision Checkout Flow

This guide explains how the **Indodax Crypto Pay** checkout widget and backend payment engine process transactions accurately—even when dozens of customers pay identical invoice amounts at the same time.

---

## 1. Smart QR Code ("Barcode") with Auto-Filled Amount

When a customer selects a cryptocurrency and network on an unpaid WHMCS invoice and clicks **Generate Crypto Payment Address**, the module renders a **Smart Payment QR Code**:

- **Default Mode (`With Amount`):**  
  Encodes a standard cryptocurrency payment URI containing **both the recipient wallet address AND the exact crypto amount to pay** (plus Destination Tag/Memo when applicable):
  - **TRON / TRC20:** `tron:<address>?amount=<crypto_amount_exact>`
  - **Bitcoin:** `bitcoin:<address>?amount=<crypto_amount_exact>`
  - **Ethereum / ERC20 / BEP20 / Polygon:** `ethereum:<address>?amount=<crypto_amount_exact>`
  - **Solana:** `solana:<address>?amount=<crypto_amount_exact>`
  - **BNB:** `bnb:<address>?amount=<crypto_amount_exact>`
  - **Ripple (XRP):** `ripple:<address>?amount=<crypto_amount_exact>&dt=<deposit_tag>&memo=<deposit_tag>`
  - **Dogecoin:** `dogecoin:<address>?amount=<crypto_amount_exact>`
- **Customer Experience:**  
  When the customer scans the QR Code using their mobile crypto wallet, **the exact payment amount (including the unique tracking decimals) is filled in automatically**—the customer does not need to type the amount manually.
- **Fallback Toggle (`Address Only`):**  
  A compact toggle button (`With Amount` | `Address Only`) is provided right below the QR code for users scanning from exchange address-book scanners that only accept raw wallet addresses.

---

## 2. Anti-Collision Simultaneous Payments (Micro-Decimal Fingerprinting)

Because centralized exchange accounts (like Indodax) use shared deposit addresses per coin/network for your merchant account, what happens if **Customer A** and **Customer B** both pay a **$10.00 USD** invoice in **TRX** at the exact same minute?

### How MikyHost Prevents Collisions:
1. **Unique 2-Digit Micro-Code (`micro_code`):**  
   When locking a session, `IndodaxPaymentEngine` appends a unique micro-decimal fingerprint (`01`–`99`, with automatic expansion to `100`–`999` if all 99 slots are concurrently active) right above the base crypto amount:
   - **Customer A (Invoice #101):** Base `0.2530 TRX` → Assigned **Unique Code `#37`** → **`0.2537 TRX`**
   - **Customer B (Invoice #102):** Base `0.2530 TRX` → Assigned **Unique Code `#90`** → **`0.2590 TRX`**
   - **Customer C (Invoice #103):** Base `0.2530 TRX` → Assigned **Unique Code `#91`** → **`0.2591 TRX`**
2. **6-Hour Anti-Collision Guard:**  
   No two active or recently expired sessions (within a 6-hour safety window) for the same coin can ever be assigned the same `crypto_amount_exact`.
3. **Visual Highlighting on the Invoice:**  
   The checkout widget highlights the unique digits in green (e.g., `0.25`**`37`** `TRX`) and displays a badge (`Unique Code #37 • Track #<SessionID>`) so the customer knows why sending the exact amount is important.

---

## 3. Instant TXID & Indodax Internal Transfer (`VIP-...`) Tracking

In addition to automatic micro-decimal matching, the checkout widget includes an **Instant Payment Tracking (Optional TXID / Transfer ID)** box:

- Customers transferring on-chain can paste their **Blockchain Transaction Hash (TXID)**.
- Customers transferring from another Indodax account (Indodax Internal Transfer) can paste their **Indodax Transfer ID** (e.g., `VIP-TRX-15B091-6AF36CA2`).
- Clicking **Verify TXID** immediately locks that transaction hash to the customer's session (`submit_txid`), preventing any other session from claiming it and triggering an immediate check against `GET /api/v2/capital/deposit/hisrec`.

---

## 4. Automatic Invoice Marking (`Paid`) & Volatility Shield

Once `IndodaxPaymentEngine::verifySessionRecord()` detects a matching deposit with `depositStatus: "success"`:
1. The session in `mod_indodax_crypto_sessions` is updated to `status = completed` with the Indodax Deposit ID (`indodax_deposit_id`) and `tx_id`.
2. The transaction is logged to WHMCS Gateway Log (`tblgatewaylog` → `Success`).
3. `addInvoicePayment()` records the exact invoice balance in `tblaccounts` and marks the WHMCS Invoice status as **`Paid`**.
4. If **Auto-Convert Crypto** is enabled, the module places a `MARKET SELL` order via `POST /api/v2/order` to convert volatile crypto into `IDR` or `USDT`.

---
[⬅️ Previous: 02 — Indodax API V2 Setup](./02-indodax-api-v2-setup.md) | **[Next: 04 — Troubleshooting & FAQ ➡️](./04-troubleshooting-and-faq.md)**
