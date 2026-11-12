[🏠 Documentation Home](../../README.md) › [🌐 Domain Registrars](../README.md) › **Google Cloud Domains (API v1)**

---

# 🌐 Google Cloud Domains Registrar for WHMCS

**Version:** `1.1.0` | **Author:** [MikyHost Global Cloud](https://mikyhost.com) | **Compatibility:** WHMCS 8.x / 9.x | **PHP:** 8.1 – 8.3

Automate real-time domain availability lookups, domain suggestions, 1-year registrations, billing-safe renewals, custom nameservers, child nameservers (glue records), outbound transfers, and smart WHOIS contact management directly through the official **Google Cloud Domains API v1 (`https://domains.googleapis.com/v1`)**.

---

## 📑 Documentation Chapters

| Chapter | Title | Description |
| :---: | :--- | :--- |
| **01** | **[Installation & MikyHost License Activation](./01-installation-and-license.md)** | Server requirements, uploading files, WHMCS default nameservers, and unlocking the module with your `MIKY-XXXXXXX` license key. |
| **02** | **[Google Cloud Domains API v1 & IAM Setup](./02-google-cloud-api-setup.md)** | Enabling Cloud Domains API in GCP Console, creating an IAM Service Account JSON key, and configuring WHMCS settings. |
| **03** | **[Features, Billing-Safe Renewal & DNS Workflow](./03-features-and-workflow.md)** | How availability search, 1-year registration, billing-safe renewal & cron sync, glue records, outbound transfers, and WHOIS sanitization work. |
| **04** | **[Troubleshooting, Admin Diagnostics & FAQ](./04-troubleshooting-and-faq.md)** | Using the Admin Domain Diagnostics panel, one-click Admin action buttons, WHMCS Domain Sync Cron, and resolving common errors. |

---

## ✨ Key Module Highlights

1. **Official Google Cloud Domains API v1 Integration (`https://domains.googleapis.com/v1`)**
   - Zero external Composer SDK bloat — lightweight RS256 Service Account OAuth2 JWT authentication with **AES-256-GCM encrypted token caching** (`0600` strict file permissions).
   - Native Punycode / IDN domain support (`idn_to_ascii`, Symfony Polyfill, and built-in pure-PHP RFC 3492 fallback).
2. **Real-Time Domain Availability & Suggestions**
   - Supports multi-TLD batch checking (`CheckAvailability`) and alternative domain recommendations (`GetDomainSuggestions`), including multi-level TLDs (`.co.id`, `.co.uk`, `.com.au`).
3. **WHMCS System Default Nameservers & DNSSEC-Safe Updates**
   - Automatically inherits your **WHMCS Default Nameservers** (*System Settings › General Settings › Domains*) when custom nameservers are not specified on an order.
   - Updating custom nameservers (`NS1`–`NS5`) preserves existing **DNSSEC `dsRecords`** automatically.
   - Full **Child Nameservers / Glue Records** (`RegisterNameserver`, `ModifyNameserver`, `DeleteNameserver`) for both IPv4 and IPv6 addresses.
4. **Billing-Safe Renewal & Cron Sync Engine**
   - Direct API renewal (`:renewDomain`) with dynamic TLD price discovery for recently expired domains (within the 30-day grace period).
   - Coordinated `AUTOMATIC_RENEWAL` activation for active domains paid in WHMCS, with **automatic reset back to `RENEWAL_DISABLED`** via WHMCS Cron Sync so unpaid domains never auto-bill your GCP project.
5. **Smart WHOIS Contact & Address Sanitizer**
   - Automatically cleans dummy customer postal codes (`00000`, `-`, `N/A`), street lines, E.164 phone numbers, and US/CA/AU state codes (including state-matched fallback postal codes) so registrations are never rejected by Google's strict address validator.
   - Automatic per-TLD WHOIS privacy negotiation (`REDACTED_CONTACT_DATA` vs `PUBLIC_CONTACT_DATA`).
6. **MikyHost Commercial License Protection**
   - Built-in license guard that prompts for your **Module License Key** upon activation and unlocks all registrar configuration settings immediately once verified active.

---

## 🗂️ Package File Structure

```text
whmcs_root/
└── modules/
    └── registrars/
        └── googlecloud/
            ├── googlecloud.php         # Main WHMCS Registrar Entry Point & MikyHost License Guard
            ├── whmcs.json              # WHMCS Marketplace & Admin Metadata
            ├── logo.png                # Registrar Logo for WHMCS Admin UI
            ├── README.md               # Quick-Start Module Guide
            └── lib/
                ├── ApiClient.php       # Google Cloud Domains API v1 Client & Punycode Engine
                ├── AuthHelper.php      # OAuth2 Service Account JWT Signer & Encrypted Token Cache
                ├── ContactHelper.php   # WHOIS Contact Parser & Smart Address Sanitizer
                └── Exception.php       # Custom API Exception Handler
```

---
[⬅️ Back to Domain Registrars](../README.md) | **[Next: 01 — Installation & License Activation ➡️](./01-installation-and-license.md)**
