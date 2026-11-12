[🏠 Documentation Home](../../README.md) › [🌐 Domain Registrars](../README.md) › [Google Cloud Domains](./README.md) › **04 — Troubleshooting, Admin Diagnostics & FAQ**

---

# 04 — Troubleshooting, Admin Diagnostics & FAQ

This chapter covers real-time diagnostics in the WHMCS Admin Area, one-click administrative tools, cron synchronization, and solutions to common questions.

---

## 1. Admin Domain Diagnostics Panel

When viewing any domain assigned to **Google Cloud Domains** in **WHMCS Admin › Clients › Domain Registrations**, the module automatically queries Google Cloud API and displays a live diagnostic summary:

- **GCP Registration State:** `ACTIVE`, `REGISTRATION_PENDING`, `SUSPENDED`, `EXPIRED`, or `EXPORTED`.
- **GCP Renewal Method:** Displays both `preferredRenewalMethod` and effective `renewalMethod` (e.g., `RENEWAL_DISABLED` vs `AUTOMATIC_RENEWAL`).
- **GCP Transfer Lock:** Shows `LOCKED`, `UNLOCKED`, or `Unsupported by Registry`.
- **GCP WHOIS Privacy:** Shows `REDACTED_CONTACT_DATA` or `PUBLIC_CONTACT_DATA`.
- **GCP DNS & DNSSEC:** Shows active DNS provider (`Custom DNS`) and count of configured DNSSEC DS records.
- **GCP Child Nameservers:** Lists all active glue records (`ns1.example.com (198.51.100.10)`).
- **GCP Alerts / Issues:** Highlights actionable registry or account alerts such as `UNVERIFIED_EMAIL`, `DNS_NOT_ACTIVATED`, or `PROBLEM_WITH_BILLING`.

---

## 2. One-Click Admin Custom Buttons

Three custom action buttons are available directly on the WHMCS Admin Domain management toolbar:

1. **Reset Authorization Code:** Generates a new random EPP / transfer authorization code on Google Cloud and displays it immediately.
2. **Disable GCP Auto-Renew:** Forces `preferredRenewalMethod` to `RENEWAL_DISABLED` on Google Cloud at any time.
3. **Resend Verification Email:** Triggers a fresh **ICANN Registrant Verification Email** from Google Cloud to the current registrant email address (ideal when `GCP Alerts / Issues` shows `UNVERIFIED_EMAIL` or a domain is suspended pending ICANN verification).

---

## 3. WHMCS Domain Sync Cron

Ensure WHMCS Domain Synchronization is enabled under **Configuration (⚙️ System Settings) › Automation Settings › Domain Sync Settings**:
- Synchronizes `Active`, `Expired`, `Transferred Away` (`EXPORTED`), and `Cancelled` (`REGISTRATION_FAILED`) statuses automatically.
- Automatically locks newly provisioned `ACTIVE` domains to `RENEWAL_DISABLED` and resets `AUTOMATIC_RENEWAL` back to `RENEWAL_DISABLED` after paid renewals complete on expiration day.

---

## 4. Viewing Detailed API Logs

If an API call fails, inspect the exact request and response in WHMCS:
1. Go to **Configuration (⚙️ System Settings) › System Logs › Module Log** (or **Utilities › Logs › Module Log**).
2. Click **Enable Debug Logging** if not already enabled.
3. Look for entries under module **`googlecloud`**. Sensitive fields (`private_key`, `access_token`, `serviceAccountJson`, and EPP authorization `code`) are automatically scrubbed for security.

---

## 5. Frequently Asked Questions (FAQ)

### Q1: Why do I only see "Module License Key" when I activate Google Cloud Domains?
This is expected behavior. The MikyHost License Guard hides all Google Cloud settings until a valid **Module License Key** (`MIKY-XXXXXXX`) is entered and saved. Once verified active, all configuration fields unlock immediately.

### Q2: Why does multi-year registration (2–10 years) return an error?
Google Cloud Domains API v1 only supports **1-year** initial registrations and **1-year** renewals per transaction. In **System Settings › Domain Pricing**, enable 1-year pricing and set 2–10 year terms to `-1.00` (disabled) for TLDs using Google Cloud Domains.

### Q3: Why did an inbound domain transfer (Transfer-In) fail?
Google Cloud officially retired inbound domain transfers from third-party registrars into Cloud Domains. Outbound transfers (EPP code retrieval & `.uk` IPS Tag push) remain fully supported.

### Q4: A newly registered domain became `SUSPENDED` after 15 days — how do I fix it?
ICANN requires the registrant to click the verification link sent to their email address within 15 days of registration. Open the domain in WHMCS Admin, check if `GCP Alerts / Issues` lists `UNVERIFIED_EMAIL`, verify the registrant's email in **Contact Information**, and click **Resend Verification Email**. Once the customer clicks the link, Google Cloud unsuspends the domain automatically.

---
[⬅️ Previous: 03 — Features & Workflow](./03-features-and-workflow.md) | [🏠 Back to Google Cloud Domains Overview](./README.md)
