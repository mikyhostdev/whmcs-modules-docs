[🏠 Documentation Home](../../README.md) › [💳 Payment Gateways](../README.md) › [Indodax Crypto Pay](./README.md) › **01 — Installation & License Activation**

---

# 01 — Installation & MikyHost License Activation

Follow this step-by-step guide to install **Indodax Crypto Pay (Trade API V2)** on your WHMCS server and activate your **MikyHost Module License Key**.

---

## 1. Server Requirements

Before uploading the module files, ensure your hosting environment meets the following requirements:

| Requirement | Minimum / Recommended |
| :--- | :--- |
| **WHMCS Version** | WHMCS `8.0` or newer (`8.8` – `8.11+` recommended) |
| **PHP Version** | PHP `7.4`, `8.1`, `8.2`, or `8.3` |
| **PHP Extensions** | `ionCube PHP Loader`, `cURL`, `JSON`, `OpenSSL`, `PDO / MySQL` |
| **Outbound Firewall** | Outbound HTTPS (`port 443`) access to `https://api.indodax.com` and `https://mikyhost.com` |
| **Invoice Currencies** | `USD` and/or `IDR` (or *Convert To For Processing* enabled) |

---

## 2. Uploading the Module Files

1. Download the **Indodax Crypto Pay for WHMCS** module archive from your [MikyHost Client Area](https://mikyhost.com).
2. Extract and upload the contents directly into your **WHMCS root directory** (for example `/var/www/vhosts/yourdomain.com/httpdocs/` or `/home/username/public_html/`):
   - `modules/gateways/indodax.php`
   - `modules/gateways/callback/indodax.php`
   - `modules/gateways/indodax/lib/IndodaxApiV2.php`
   - `modules/gateways/indodax/lib/IndodaxPaymentEngine.php`
   - `modules/gateways/indodax/lib/checkout_widget.php`
   - `modules/gateways/indodax/whmcs.json`
   - `modules/gateways/indodax/logo.png`
   - `includes/hooks/indodax_crypto_gateway.php`
3. Verify file permissions are set to `0644` for PHP files and `0755` for directories.

> **Database Table Creation:**  
> You do **not** need to import any `.sql` file manually. The module automatically creates and maintains the `mod_indodax_crypto_sessions` table as soon as you open the Payment Gateway settings.

---

## 3. Activating the Gateway & Entering Your License Key

1. Log in to your **WHMCS Admin Area**.
2. Navigate to **Configuration (⚙️ System Settings) › Apps & Integrations** or **Setup › Payments › Payment Gateways**.
3. Locate **Indodax** (`Accept cryptocurrency payments • Instant Verification`) and click **Activate**.
4. **Initial License Lock Screen:**
   - When the module is first activated, all configuration fields are locked and only the **Module License Key** field is displayed along with the red notice:
     > 🔒 **Module License is Invalid or Expired!**  
     > *Please enter a valid MikyHost License Key above and click Save Changes to unlock the module settings.*
5. Enter your MikyHost License Key (e.g., `MIKY-XXXXXXX`) in the **Module License Key** field and click **Save Changes**.
6. **Automatic Unlock:**
   - The module verifies your license key with the MikyHost Licensing Server (`https://mikyhost.com/check_license.php`) and caches a signed 15-day `localkey`.
   - Once verified, a green **✓ License Active** badge appears and all Indodax Trade API V2 configuration fields immediately unlock on the page.

---
[⬅️ Back to Module Overview](./README.md) | **[Next: 02 — Indodax Trade API V2 Setup ➡️](./02-indodax-api-v2-setup.md)**
