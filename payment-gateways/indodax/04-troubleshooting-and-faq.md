[🏠 Documentation Home](../../README.md) › [💳 Payment Gateways](../README.md) › [Indodax Crypto Pay](./README.md) › **04 — Troubleshooting, Cron Sync & FAQ**

---

# 04 — Troubleshooting, Cron Sync & FAQ

This guide covers background synchronization, admin inspection tools, and solutions to common operational questions.

---

## 1. Admin Invoice Inspector & Force-Sync Button

When viewing any invoice in the **WHMCS Admin Area** (`clientsinvoices.php?action=edit&id=...`) that has one or more Indodax payment sessions:

- The hook (`includes/hooks/indodax_crypto_gateway.php`) renders the **Indodax Crypto Pay (Trade API V2) — Payment Sessions Inspector** panel directly on the invoice page.
- It displays:
  - Session ID, Coin & Network, Locked Fiat Amount, and **Exact Crypto Amount (with Micro-Code)**.
  - Current Status (`awaiting_transfer`, `blockchain_pending`, `completed`, `expired`).
  - Recorded `TXID` / `Indodax Deposit ID` and Auto-Convert status.
- **Force-Sync Button:**  
  Click **Sync with Indodax API V2 Now** to immediately query Indodax's deposit ledger for that invoice and mark it **Paid** on the spot.

---

## 2. Background Cron Synchronization (`AfterCronJob`)

What if a customer transfers cryptocurrency and closes their browser tab before the blockchain confirmation finishes?

- Ensure `includes/hooks/indodax_crypto_gateway.php` is present in your WHMCS `includes/hooks/` directory.
- Every time the standard **WHMCS System Cron** (`crons/cron.php`) runs, the `AfterCronJob` hook automatically reconciles pending and recently expired sessions from the last 96 hours against Indodax Trade API V2.
- As soon as Indodax marks the deposit `success`, the WHMCS Cron marks the invoice **Paid** and triggers service provisioning automatically.

---

## 3. Frequently Asked Questions (FAQ)

### Q1: Why did the module settings only show "Module License Key" when I opened Payment Gateways?
This is expected behavior. The MikyHost License Guard keeps all API and coin settings hidden until a valid **Module License Key** (`MIKY-XXXXXXX`) is entered and saved. Once your license key is verified active, all settings unlock immediately.

### Q2: Why does an error say "No deposit address found on your Indodax account for [COIN]"?
Indodax only generates wallet addresses on demand the first time you open a coin's deposit page. Log in to **[indodax.com/finance](https://indodax.com/finance/)**, select the coin/network, and click **Deposit** once to generate your address.

### Q3: A customer paid via Indodax Internal Transfer (Sesama Indodax), is it supported?
**Yes!** Indodax records internal transfers in `/api/v2/capital/deposit/hisrec` with a `VIP-...` transaction ID (e.g., `VIP-TRX-...`) and `depositStatus: "success"`. The module automatically detects both on-chain blockchain deposits and internal Indodax transfers.

### Q4: How do I support currencies other than USD and IDR (e.g., EUR, GBP, SGD)?
In WHMCS Admin › **Setup › Payments › Payment Gateways**, locate **Indodax** and set **Convert To For Processing** to either **`USD`** or **`IDR`**. WHMCS will automatically convert the invoice total at checkout and record the payment back in the client's native currency when paid.

---
[⬅️ Previous: 03 — Features & Checkout Flow](./03-features-and-checkout-flow.md) | [🏠 Back to Indodax Overview](./README.md)
