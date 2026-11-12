[🏠 Documentation Home](../../README.md) › [🌐 Domain Registrars](../README.md) › [Google Cloud Domains](./README.md) › **01 — Installation & License Activation**

---

# 01 — Installation & MikyHost License Activation

Follow this step-by-step guide to install **Google Cloud Domains for WHMCS** on your server and activate your **MikyHost Module License Key**.

---

## 1. Server Requirements

Before uploading the module files, ensure your hosting environment meets the following requirements:

| Requirement | Minimum / Recommended |
| :--- | :--- |
| **WHMCS Version** | WHMCS `8.0` or newer (`8.8+` / `9.x` recommended) |
| **PHP Version** | PHP `8.1`, `8.2`, or `8.3` |
| **PHP Extensions** | `cURL`, `JSON`, `OpenSSL`, `PDO / MySQL` (`intl` optional — built-in Punycode fallback included) |
| **Outbound Firewall** | Outbound HTTPS (`port 443`) access to `https://domains.googleapis.com`, `https://oauth2.googleapis.com`, and `https://mikyhost.com` |
| **Google Cloud Account** | Active Google Cloud Project with Billing enabled and **Cloud Domains API** activated |

---

## 2. Uploading the Module Files

1. Download the **Google Cloud Domains for WHMCS** archive from your [MikyHost Client Area](https://mikyhost.com).
2. Extract and upload the folder directly into your **WHMCS root directory** (for example `/var/www/vhosts/yourdomain.com/httpdocs/` or `/home/username/public_html/`):
   - `modules/registrars/googlecloud/googlecloud.php`
   - `modules/registrars/googlecloud/whmcs.json`
   - `modules/registrars/googlecloud/logo.png`
   - `modules/registrars/googlecloud/README.md`
   - `modules/registrars/googlecloud/lib/ApiClient.php`
   - `modules/registrars/googlecloud/lib/AuthHelper.php`
   - `modules/registrars/googlecloud/lib/ContactHelper.php`
   - `modules/registrars/googlecloud/lib/Exception.php`
3. Verify file permissions are set to `0644` for PHP files and `0755` for directories.

---

## 3. Configuring WHMCS System Default Nameservers

Before registering domains, confirm your default nameservers are configured in WHMCS:
1. Navigate to **Configuration (⚙️ System Settings) › General Settings › Domains**.
2. Fill in **Default Nameserver 1** and **Default Nameserver 2** (and optionally 3–5), for example `ns1.yourdomain.com` and `ns2.yourdomain.com`.
3. Click **Save Changes**.

> Whenever a customer or admin places a domain registration without custom nameservers, the module automatically uses these WHMCS System Default Nameservers.

---

## 4. Activating the Registrar & Entering Your License Key

1. Log in to your **WHMCS Admin Area**.
2. Navigate to **Configuration (⚙️ System Settings) › Domain Registrars** (or **Setup › Products/Services › Domain Registrars**).
3. Locate **Google Cloud Domains** and click **Activate**.
4. **Initial License Lock Screen:**
   - Upon activation, all Google Cloud configuration fields are locked and only the **Module License Key** field (plus the **📖 Documentation** button) is shown with the red notice:
     > 🔒 **Module License is Invalid or Expired!**  
     > *Please enter a valid MikyHost License Key above and click Save Changes to unlock the module configuration.*
   - The license input is protected against browser password managers so your license key is never overwritten by saved admin passwords or masked asterisks.
5. Enter your MikyHost License Key (e.g., `MIKY-XXXXXXX`) into **Module License Key** and click **Save Changes**.
6. **Automatic Unlock:**
   - The module verifies your license key against the MikyHost Licensing Server (`https://mikyhost.com/check_license.php`) and stores a signed 15-day `localkey` cache automatically.
   - Once verified active, a green **✓ License Active** badge appears and all **Google Cloud Domains API** settings unlock immediately on the page.

---
[⬅️ Back to Module Overview](./README.md) | **[Next: 02 — Google Cloud Domains API v1 & IAM Setup ➡️](./02-google-cloud-api-setup.md)**
