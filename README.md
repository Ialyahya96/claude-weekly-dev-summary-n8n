# Claude Weekly Dev Summary for n8n 🚀
> Automated weekly engineering changelogs: GitHub activity (commits, closed issues, merged PRs) → Anthropic Claude Sonnet 4 → formatted Slack & Discord digest every Friday.

[![n8n 1.77+](https://img.shields.io/badge/n8n-v1.77%2B-EA4B71?style=flat-square&logo=n8n)](https://n8n.io)
[![Claude Sonnet 4](https://img.shields.io/badge/AI-Claude%20Sonnet%204-D97706?style=flat-square&logo=anthropic)](https://anthropic.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Instant Download](https://img.shields.io/badge/Buy%20on%20Gumroad-$29-238636?style=flat-square&logo=gumroad)](https://jrbcodes.gumroad.com/l/luqoza)
[![Multi-Rail Checkout](https://img.shields.io/badge/Crypto%20%2F%20Saudi%20Checkout-Live-74e0b5?style=flat-square)](https://myn8n-automation.org/checkout/)

---

## ⚡ Overview

Stop writing weekly engineering updates by hand. 

This production workflow runs automatically every Friday at 5:00 PM, pulls the past 7 days of repository telemetry across GitHub, passes it through a calibrated Claude Sonnet prompt, and broadcasts a manager-grade narrative summary directly into your team's Slack or Discord channel.

### 🌐 Live Preview & Direct Access
- **Interactive Overview:** [myn8n-automation.org/preview/weekly-dev-summary](https://myn8n-automation.org/preview/weekly-dev-summary/)
- **1-Click Card / Apple Pay ($29):** [Gumroad Listing](https://jrbcodes.gumroad.com/l/luqoza)
- **Multi-Rail Direct Checkout (USDC / STC Pay / Al Rajhi):** [Official Checkout Portal](https://myn8n-automation.org/checkout/)

---

## 📸 Execution Proof

![Verified Runtime Execution](execution.png)

---

## 🛠️ Architecture Pipeline

```mermaid
graph LR
    A[Schedule Trigger: Fri 5pm] --> B[Set Config: Repo / Language / Webhook]
    B --> C[Fetch Commits last 7d]
    B --> D[Fetch Closed Issues last 7d]
    B --> E[Fetch Merged PRs last 7d]
    C --> F[Data Aggregator]
    D --> F
    E --> F
    F --> G[Claude Sonnet 4 Engine]
    G --> H[Slack / Discord Webhook Dispatch]
```

---

## 📋 Quick Setup (5 Steps)

1. **Import:** Download [`workflow.json`](workflow.json) and import into your n8n workspace (`Workflows` → `Import from File`).
2. **Credentials:** Attach your existing credentials in n8n:
   - `githubApi` (GitHub PAT with `repo` read permissions)
   - `anthropicApi` (Anthropic API Key)
3. **Configure:** In the `Set Config` node, specify:
   - `githubRepo`: e.g. `your-org/your-repo`
   - `language`: `EN` or `FR`
   - `webhookUrl`: Your Slack or Discord incoming webhook URL
4. **Schedule:** Defaults to Friday at 5:00 PM (`0 17 * * 5`). Adjust the cron trigger if your sprint ends at a different time.
5. **Test & Deploy:** Click `Test workflow` to trigger a manual test run and verify the formatted report in your channel.

---

## 📦 Commercial Package Includes

When acquiring the production license via [Gumroad](https://jrbcodes.gumroad.com/l/luqoza) or [Direct Checkout](https://myn8n-automation.org/checkout/):
- ✅ Complete, battle-tested `workflow.json` (8 nodes, hermetic execution)
- ✅ Step-by-step setup documentation & webhook payload templates
- ✅ Lifetime updates and priority configuration support
- ✅ Unrestricted commercial use across unlimited internal client projects

---

## 👨‍💻 Maintainer & Licensing

Developed by **Ibrahim Al-Yahya** ([@Ialyahya96](https://github.com/Ialyahya96))  
Licensed under the [MIT License](LICENSE).
