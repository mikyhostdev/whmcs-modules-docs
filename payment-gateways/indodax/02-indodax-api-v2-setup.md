[🏠 Documentation Home](../../README.md) › [💳 Payment Gateways](../README.md) › [Indodax Crypto Pay](./README.md) › **02 — Indodax Trade API V2 & Wallet Setup**

---

# 02 — Indodax Trade API V2 & Wallet Setup

Once your **MikyHost Module License Key** is active, configure your **Indodax Trade API V2** credentials and gateway preferences.

---

## 1. Generating Your Indodax Trade API V2 Credentials

1. Log in to your merchant Indodax account at [https://indodax.com](https://indodax.com).
2. Navigate to the Trade API management page: **[https://indodax.com/trade_api](https://indodax.com/trade_api)**.
3. Create a new **API Key** and configure its permissions:
   - **Read / View Permission (`view`):** **Required** (Used to fetch deposit wallet addresses and verify incoming deposit history via `GET /api/v2/capital/deposit/hisrec`).
   - **Trade Permission (`trade`):** **Optional** (Only required if you enable the **Auto-Convert Crypto** feature to automatically sell volatile coins into `IDR` or `USDT`).
   - **Withdraw Permission (`withdraw`):** **Do NOT enable** (Not required for receiving customer payments).
4. Copy your **API Key** and **Secret Key** and paste them into the WHMCS Payment Gateway settings:
   - **Indodax API v2 API Key**
   - **Indodax API v2 Secret Key**

---

## 2. Initializing Deposit Wallet Addresses on Indodax

Indodax Trade API V2 (`GET /api/v2/capital/deposit/address/list`) returns wallet addresses that have already been generated on your Indodax account.

1. Open your Indodax Wallet at **[https://indodax.com/finance/](https://indodax.com/finance/)**.
2. For each cryptocurrency and network you plan to accept (`USDT`, `USDC`, `BTC`, `ETH`, `SOL`, `BNB`, `TRX`, `XRP`, `DOGE`), click **Deposit** at least once in the Indodax dashboard so Indodax assigns a permanent deposit address to your account.

---

## 3. Gateway Configuration Fields Explained

| Setting Name | Default | Description |
| :--- | :--- | :--- |
| **Module License Key** | — | Your commercial license key from MikyHost (e.g., `MIKY-XXXXXXX`). |
| **Indodax API v2 API Key** | — | Your API Key from `indodax.com/trade_api`. |
| **Indodax API v2 Secret Key** | — | Your Secret Key used for HMAC-SHA256 request signing. |
| **Select Cryptocurrency** | All 9 Coins | Interactive checkboxes in WHMCS Admin to toggle which coins (`USDT, USDC, BTC, ETH, SOL, BNB, TRX, XRP, DOGE`) appear on customer invoices. |
| **Payment Expiration Time** | `30 Minutes` | Duration for which a customer's locked exchange rate and payment session remain valid (`15m` up to `72h`). |
| **Auto-Convert Crypto** | `Disabled` | • `Disabled`: Keep received cryptocurrency in your Indodax wallet.<br>• `Auto-Convert Volatile Crypto to IDR`: Market-sell volatile coins to IDR.<br>• `Auto-Convert Volatile Crypto to USDT`: Market-sell volatile coins to USDT. |
| **Sandbox / Demo Mode** | `Off` | Enables demo wallet addresses and a one-click **Simulate Paid** button on the invoice widget for safe end-to-end testing. |
| **Convert To For Processing** | `None` / `USD` / `IDR` | Built-in WHMCS setting. If your store uses currencies other than `USD` or `IDR` (such as `EUR`, `SGD`, `MYR`), set this to `USD` or `IDR`. |

---
[⬅️ Previous: 01 — Installation & License](./01-installation-and-license.md) | **[Next: 03 — Features & Checkout Flow ➡️](./03-features-and-checkout-flow.md)**
