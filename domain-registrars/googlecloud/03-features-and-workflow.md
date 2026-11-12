[🏠 Documentation Home](../../README.md) › [🌐 Domain Registrars](../README.md) › [Google Cloud Domains](./README.md) › **03 — Features, Billing-Safe Renewal & DNS Workflow**

---

# 03 — Features, Billing-Safe Renewal & DNS Workflow

This chapter details how the **Google Cloud Domains** registrar module handles domain lookups, registrations, billing-safe renewals, DNSSEC-safe nameserver updates, child nameservers, transfers, and WHOIS privacy.

---

## 1. Real-Time Availability Search & Domain Suggestions

- **`CheckAvailability`:** Combines fast batch queries via `registrations:searchDomains` with targeted `retrieveRegisterParameters` lookups for specific TLDs (`tldsToInclude`), accurately handling multi-level TLDs (such as `.co.id`, `.co.uk`, `.com.au`) and Unicode / IDN domain names.
- **`GetDomainSuggestions`:** Surfaces available alternative domain names directly inside the WHMCS shopping cart.

---

## 2. Automated Domain Registration & Per-TLD Privacy

When an order is paid (or accepted in WHMCS Admin):
1. Verifies a **1-year** registration term (`regperiod == 1`) and queries `retrieveRegisterParameters` for real-time availability, exact `yearlyPrice`, `domainNotices` (e.g., `HSTS_PRELOADED` for `.app` / `.dev`), and `supportedPrivacy`.
2. **Automatic WHOIS Privacy Negotiation:**
   - If **ID Protection** is enabled, the module selects `REDACTED_CONTACT_DATA` when supported by the registry, or falls back to `PUBLIC_CONTACT_DATA` on registries that prohibit WHOIS redaction (such as `.us` or `.au`) so orders never fail.
3. **Automatic Nameserver Resolution:**
   - Uses custom nameservers (`NS1`–`NS5`) from the order, or automatically falls back to your **WHMCS System Default Nameservers**.
4. **Post-Provisioning Auto-Renew Lock:**
   - Because Google Cloud API forbids setting `RENEWAL_DISABLED` during resource creation, the module submits a clean creation payload and automatically switches `preferredRenewalMethod` to **`RENEWAL_DISABLED`** once the domain reaches `ACTIVE` state (immediately or on the next WHMCS Domain Sync Cron).

---

## 3. Billing-Safe Domain Renewal & Cron Sync Engine

Google Cloud Domains API handles active and expired renewals differently. MikyHost bridges both so **WHMCS remains the single source of truth for billing**:

- **Expired Domains (Within 30-Day Grace Period):**
  - When WHMCS triggers `RenewDomain` on an `EXPIRED` registration, the module dynamically resolves the exact TLD renewal price via `resolveRenewalPrice()`, executes `POST :renewDomain` to add +1 year immediately, and locks `preferredRenewalMethod` back to `RENEWAL_DISABLED`.
- **Active Domains (Paid Before Expiration Date):**
  - Google Cloud's `:renewDomain` endpoint cannot be called before expiration; active domains renew on expiration day when `preferredRenewalMethod` is set to `AUTOMATIC_RENEWAL`.
  - When a client pays their renewal invoice in WHMCS, `RenewDomain` sets `preferredRenewalMethod = AUTOMATIC_RENEWAL` in GCP.
  - Before expiration day, `googlecloud_Sync` detects that WHMCS's renewed expiry date is ahead of GCP's `expireTime` and **preserves WHMCS's renewed expiry date** without rolling it backward.
  - Once Google Cloud executes the renewal on expiration day and advances `expireTime`, the next `googlecloud_Sync` run **automatically resets `preferredRenewalMethod` back to `RENEWAL_DISABLED`**—preventing unwanted auto-billing in the following year if unpaid!

---

## 4. Custom Nameservers (DNSSEC-Safe) & Child Nameservers (Glue Records)

- **DNSSEC-Safe Nameserver Updates (`SaveNameservers`):**
  - Inspects existing DNS settings before updating nameservers. If the domain already uses `customDns` with active **DNSSEC `dsRecords`**, the module updates `customDns.nameServers` while keeping all existing DS records intact.
- **Child Nameservers / Glue Records (`RegisterNameserver`, `ModifyNameserver`, `DeleteNameserver`):**
  - Clients and admins can create, update, and delete private nameservers (e.g., `ns1.clientdomain.com` → `198.51.100.10` and/or `2001:db8::1`) directly from WHMCS using `dnsSettings.glueRecords`.

---

## 5. Outbound Transfers, EPP Codes & `.uk` IPS Tag Push

- **Registrar Lock (`GetRegistrarLock` / `SaveRegistrarLock`):** Reads `effectiveTransferLockState` and detects registries where transfer locks are unsupported (`TRANSFER_LOCK_UNSUPPORTED_BY_REGISTRY`).
- **EPP / Authorization Code (`GetEPPCode` & Admin Reset):** Retrieves transfer authorization codes and provides clear messaging for ICANN's 60-day post-registration lock.
- **IPS Tag Push Transfer (`ReleaseDomain`):** Native support for `.uk` / Nominet push transfers via `POST :initiatePushTransfer` with the gaining registrar's IPS Tag.
- **Inbound Transfers (`TransferDomain`):** Per Google Cloud's deprecation policy, third-party inbound transfers (*Transfer-In*) are retired by Google Cloud; the module returns a clear explanation if attempted.

---

## 6. Smart Contact & Postal Code Sanitizer

When **Auto-Sanitize Contact Address** is `Enabled`:
- Full state/province names in the US (including `PR`, `GU`, `VI`, `AS`, `MP`), Canada, and Australia are normalized to official ISO codes (`California` → `CA`, `Ontario` → `ON`).
- Dummy postal codes (`00000`, `-`, `N/A`) are replaced with a **state-matched valid postal code** (`CA` → `90001`, `NY` → `10001`, `PR` → `00901`, `ON` → `M5V 2T6`).
- Phone numbers are automatically formatted to **E.164** (`+62...`, `+1...`).

---
[⬅️ Previous: 02 — Google Cloud API Setup](./02-google-cloud-api-setup.md) | **[Next: 04 — Troubleshooting & FAQ ➡️](./04-troubleshooting-and-faq.md)**
