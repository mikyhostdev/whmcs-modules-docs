[🏠 Documentation Home](../../README.md) › [🌐 Domain Registrars](../README.md) › [Google Cloud Domains](./README.md) › **02 — Google Cloud Domains API v1 & IAM Setup**

---

# 02 — Google Cloud Domains API v1 & IAM Setup

Once your **MikyHost Module License Key** is verified active, follow this guide to create your **Google Cloud Service Account** and configure the registrar settings in WHMCS.

---

## 1. Enabling Cloud Domains API in Google Cloud Console

1. Sign in to the [Google Cloud Console](https://console.cloud.google.com/).
2. Select an existing project or create a new Google Cloud Project, and ensure an active **Cloud Billing Account** is linked to the project.
3. Navigate to **APIs & Services › Library**.
4. Search for **Cloud Domains API** (`domains.googleapis.com`) and click **Enable**.

---

## 2. Creating an IAM Service Account & JSON Key

1. In Google Cloud Console, navigate to **IAM & Admin › Service Accounts**.
2. Click **+ Create Service Account**:
   - **Service account name:** `whmcs-domains-registrar`
   - **Description:** `WHMCS Domain Registrar Automation`
   - Click **Create and Continue**.
3. **Grant IAM Role:**
   - Select the role **Cloud Domains Admin** (`roles/domains.admin`).
   - Click **Continue**, then click **Done**.
4. **Generate JSON Key:**
   - Click on the newly created `whmcs-domains-registrar` service account.
   - Open the **Keys** tab › click **Add Key** › **Create new key**.
   - Select **JSON** and click **Create** to download the `.json` credentials file to your computer.

---

## 3. WHMCS Registrar Settings Explained

Open **WHMCS Admin › System Settings › Domain Registrars › Google Cloud Domains** and configure the unlocked fields:

| Setting Name | Default | Description |
| :--- | :---: | :--- |
| **Module License Key** | — | Your active MikyHost commercial license key (`MIKY-XXXXXXX`). |
| **Service Account JSON Key** | — | Open your downloaded GCP `.json` key file in a text editor, copy the entire JSON content (or its Base64-encoded string), and paste it here. |
| **Project ID** | *Auto* | Optional. Leave blank to use the `project_id` embedded inside your Service Account JSON, or enter a custom GCP Project ID to override it. |
| **API Location** | `global` | Google Cloud Domains location ID. Keep set to `global`. |
| **Auto-Sanitize Contact Address** | `Enabled` | Automatically normalizes invalid customer street addresses, dummy postal codes (`00000`, `N/A`, `-`), E.164 phone numbers, and state/province codes before submitting to Google Cloud. |
| **Fallback Street Address** | `100 Main Street` | Fallback street line used when a customer's street address is missing or dummy. |
| **Fallback City** | `New York` | Fallback locality used when a customer's city is missing or dummy. |
| **Fallback State / Province** | `NY` | Fallback administrative area code used when a customer's state is missing or invalid. |
| **Fallback Postal Code** | `10001` | Fallback postal code used when a customer's postal code is missing or invalid. |
| **Fallback Country Code** | `US` | Default 2-letter ISO 3166-1 alpha-2 country code (`US`, `ID`, `SG`, `GB`, etc.). |
| **Validate Only (Test Mode)** | `Off` | Enables Google Cloud's `validateOnly` dry-run mode so you can test registration, DNS, and contact payloads without placing live charges. |

Click **Save Changes** when finished.

---

## 4. Configuring TLD Pricing & Lookup Provider in WHMCS

1. Go to **Configuration (⚙️ System Settings) › Domain Pricing**.
2. Add the TLDs you wish to sell (e.g., `.com`, `.net`, `.org`, `.io`, `.dev`, `.app`), select **Google Cloud Domains** in the **Auto Registration** column, and configure **1-Year** pricing.
   > **Important:** Google Cloud Domains API v1 processes initial registrations and renewals in **1-year** increments. Disable multi-year (`2`–`10` year) registration pricing for TLDs assigned to Google Cloud Domains by setting those multi-year price fields to `-1.00`.
3. *(Optional)* To power real-time availability checks & domain suggestions on your storefront via Google Cloud Domains, click **Change** next to **Lookup Provider** at the top of the **Domain Pricing** page, choose **Domain Registrar**, and select **Google Cloud Domains**.

---
[⬅️ Previous: 01 — Installation & License](./01-installation-and-license.md) | **[Next: 03 — Features, Renewal & DNS Workflow ➡️](./03-features-and-workflow.md)**
