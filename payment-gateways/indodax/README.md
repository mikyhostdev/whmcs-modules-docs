[🏠 Documentation Home](../../README.md) › [💳 Payment Gateways](../README.md) › **Indodax Crypto Pay (Trade API V2)**

---

# 🪙 Indodax Crypto Pay (Trade API V2) for WHMCS

**Version:** `2.0.0` | **Author:** [MikyHost Global Cloud](https://mikyhost.com) | **Compatibility:** WHMCS 8.x+ | **PHP:** 7.4 – 8.3 (ionCube Loader)

Accept cryptocurrency payments (`USDT`, `USDC`, `BTC`, `ETH`, `SOL`, `BNB`, `TRX`, `XRP`, `DOGE`) directly into your **Indodax** account with instant blockchain & internal-transfer verification powered by the official **Indodax Trade API V2 (`https://api.indodax.com`)**.

---

## 📑 Documentation Chapters

| Chapter | Title | Description |
| :---: | :--- | :--- |
| **01** | **[Installation & MikyHost License Activation](./01-installation-and-license.md)** | Server requirements, directory structure, uploading files, and unlocking the module with your `MIKY-XXXXXXX` license key. |
| **02** | **[Indodax Trade API V2 & Wallet Setup](./02-indodax-api-v2-setup.md)** | Generating your Indodax TAPI v2 Key & Secret, configuring permissions, and initializing deposit addresses. |
| **03** | **[Features, Smart QR Code & Anti-Collision Flow](./03-features-and-checkout-flow.md)** | How Smart QR Codes (`?amount=`), Micro-Decimal Fingerprinting, TXID/VIP Transfer Tracking, and Auto-Sell Volatility Shield work. |
| **04** | **[Troubleshooting, Cron Sync & FAQ](./04-troubleshooting-and-faq.md)** | Resolving common issues, Admin Invoice Force-Sync Inspector, and background Cron reconciliation. |

---

## ✨ Key Module Highlights

1. **Official Indodax Trade API V2 Integration (`https://api.indodax.com`)**
   - Authenticated with `X-APIKEY` and `HMAC-SHA256` (`Sign`) headers.
   - Live market exchange rates via `GET /api/v2/ticker/price`.
   - Automated wallet address & destination tag lookup via `GET /api/v2/capital/deposit/address/list`.
   - Real-time deposit reconciliation via `GET /api/v2/capital/deposit/hisrec`.
2. **Smart Payment QR Code (Auto-Fills Exact Amount)**
   - Encodes standard cryptocurrency payment URIs (`bitcoin:`, `tron:`, `ethereum:`, `solana:`, `ripple:`, `dogecoin:`, `bnb:`) with `?amount=<exact_crypto_amount>` and `&dt=<tag>`, so customer wallets automatically fill in both the recipient address and exact payment amount.
   - Includes a one-click `With Amount` / `Address Only` switch on the checkout widget.
3. **Zero-Collision Simultaneous Payment Tracking**
   - **Micro-Decimal Fingerprinting:** Assigns a unique 2-digit tracking code (`micro_code`, e.g., `#37` → `0.2537 TRX`) to every active session so multiple customers paying the same invoice total at the exact same moment never collide.
   - **Instant TXID / Indodax Internal Transfer (`VIP-...`) Tracking:** Customers can optionally paste their Blockchain TXID or Indodax Internal Transfer ID for immediate verification.
4. **Dedicated USD & IDR Conversion Engine**
   - Supports **USD** and **IDR** invoice currencies natively (or any currency via WHMCS *Convert To For Processing*).
5. **Auto-Sell Volatility Shield (`POST /api/v2/order`)**
   - Optionally converts received volatile cryptocurrencies into **IDR** or **USDT** via an automatic Market Sell order upon deposit confirmation.
6. **MikyHost Commercial License Protection**
   - Built-in license lock that displays the **Module License Key** prompt first and unlocks all gateway settings once verified active.

---

## 🗂️ Package File Structure

```text
whmcs_root/
├── includes/
│   └── hooks/
│       └── indodax_crypto_gateway.php          # Background Cron Sync & Admin Invoice Inspector
└── modules/
    └── gateways/
        ├── indodax.php                         # Main Gateway Module & MikyHost License Guard
        ├── callback/
        │   └── indodax.php                     # AJAX Session, Live Polling & TXID Verification Endpoint
        └── indodax/
            ├── README.md                       # Module Documentation
            ├── logo.png                        # Gateway Logo
            ├── whmcs.json                      # WHMCS Marketplace Metadata
            └── lib/
                ├── IndodaxApiV2.php            # Official Indodax Trade API V2 Client SDK
                ├── IndodaxPaymentEngine.php    # Core Conversion, Anti-Collision & Reconciliation Engine
                └── checkout_widget.php         # Responsive Dark-Mode Client Invoice Checkout UI
```

---
[⬅️ Back to Payment Gateways](../README.md) | **[Next: 01 — Installation & License Activation ➡️](./01-installation-and-license.md)**
