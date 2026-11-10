# 📋 MikyHost Standard Module Documentation Template

Use this structure whenever adding a new WHMCS module to `mikyhostdev/whmcs-modules-docs`:

```text
<category-folder>/
└── <module-slug>/
    ├── README.md                              # Module Overview, Features & Table of Contents
    ├── 01-installation-and-license.md         # Server Requirements, File Upload & MikyHost License Key
    ├── 02-configuration-and-api-setup.md      # API Credentials & Module Settings Guide
    ├── 03-features-and-workflow.md            # Client Area UI, Admin Area Controls & Automation
    └── 04-troubleshooting-and-faq.md          # Common Issues, Logs & Troubleshooting Steps
```

### Checklist When Publishing a New Module
1. Create a dedicated folder inside the matching category (`payment-gateways/`, `server-provisioning/`, `domain-registrars/`, or `addon-modules/`).
2. Add the 5 standard Markdown files (`README.md` + `01` through `04`) with top breadcrumb navigation and bottom Next/Previous links.
3. Add the module entry to the category's `README.md` table and the root `README.md` catalog table.
4. Set `"docs_url"` in the module's `whmcs.json` to point to `https://github.com/mikyhostdev/whmcs-modules-docs/tree/main/<category-folder>/<module-slug>`.
