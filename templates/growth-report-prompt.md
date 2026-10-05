# Weekly Growth Report — Productized Template (n8n + Claude)

**Companion to:** `workflow.json` (Weekly Dev Summary) — same 8-node pattern, repackaged as a client-facing Growth Report
**Price when sold as service:** Included in Fiverr Standard/Premium and Upwork proposals; standalone Gumroad variant $19-29
**Listing cost:** $0 (template is a README + prompt variant — no new infra)

---

## EN — What This Template Is

A **white-label weekly growth report** that reuses the exact `workflow.json` you already own — retargeted from "dev summary" to "growth report" for **non-technical buyers** (F&B owners, marketers, agency clients).

**Same nodes, different prompt, different buyer.** The workflow already fetches commits/issues/PRs; for growth reports, point it at:
- **Option A (GitHub-native):** Same 3 feeds, but Claude prompt reframed as "growth narrative for stakeholders" (features shipped → customer impact → next week's bets)
- **Option B (API-swap):** Replace GitHub nodes with any growth data source (Shopify orders, Google Analytics, Meta Ads, Notion DB, Airtable) — same Merge → Claude → Webhook shape

This file is the **prompt pack + positioning** to sell either option without rebuilding.

---

## EN — Gumroad / Fiverr Add-On Description (paste as upsell)

### 📈 Weekly Growth Report — Turn Any Data Into a Friday Narrative

Your stakeholders don't want a dashboard — they want a **story**. This add-on turns the Weekly Dev Summary workflow into a **client-ready growth report** delivered to Slack/Discord/Email every Friday.

**What changes vs. the base workflow:**
- **Claude prompt reframed** — from "engineering summary" to "growth narrative: what shipped, what moved the metric, what we bet next week" (prompt included below, EN/FR/AR)
- **Source-swappable** — GitHub by default; swap 3 fetch nodes to Shopify / GA4 / Meta / Notion / Airtable in 10 minutes (field map included)
- **Branded delivery** — webhook payload includes report title, period, and CTA link (client portal / Notion / PDF)

**You get:** 3 Claude prompts (EN/FR/AR) + source field map + branded webhook template + 1-page PDF report shell (HTML → PDF via n8n).

---

## Claude Prompts — Growth Report Variants (paste into Claude node)

### Prompt 1: GitHub → Growth Narrative (EN, for dev-savvy stakeholders)

```
You are a growth lead writing a Friday stakeholder update. Language: {{ $json.language }}. Repo: {{ $json.githubRepo }}.

Turn this week's GitHub activity into a GROWTH REPORT with 3 sections:

1. **Shipped** — what we delivered (commits/PRs) and why it matters to users
2. **Momentum** — issues closed, velocity vs last week, what unblocked
3. **Next bets** — top open issues/PRs worth betting next week, with 1-line why

Data (commits, closed issues, merged PRs last 7 days):
{{ JSON.stringify($json, null, 2) }}

Rules:
- Concise, stakeholder-friendly (no jargon). One emoji per section max.
- Highlight customer impact, not just code.
- End with one clear CTA: what needs approval or input.
- Keep under 280 words.
```

### Prompt 2: Same, in French (FR)

```
Tu es un growth lead rédigeant le rapport hebdo. Langue: {{ $json.language }}. Repo: {{ $json.githubRepo }}.

Transforme l'activité GitHub de la semaine en RAPPORT DE CROISSANCE en 3 sections:
1. Livré — ce qui a été livré et son impact utilisateur
2. Momentum — tickets fermés, vélocité, déblocages
3. Prochains paris — top issues/PRs à parier la semaine prochaine

Données:
{{ JSON.stringify($json, null, 2) }}

Règles: concis, orienté impact, un emoji max par section, CTA clair à la fin, <280 mots.
```

### Prompt 3: Arabic — تقرير نمو أسبوعي (AR, for Saudi/Gulf clients)

```
أنت مسؤول نمو تكتب تحديث الجمعة لأصحاب المصلحة. اللغة: العربية. المستودع: {{ $json.githubRepo }}.

حوّل نشاط GitHub لهذا الأسبوع إلى تقرير نمو من 3 أقسام:

1. **ما تم إنجازه** — ما سلمناه ولماذا يهم العميل
2. **الزخم** — القضايا المغلقة والسرعة وما تم فكّه
3. **رهانات الأسبوع القادم** — أهم القضايا/الطلبات للرهان عليها مع سبب بسطر واحد

البيانات (آخر 7 أيام):
{{ JSON.stringify($json, null, 2) }}

القواعد: مختصر وواضح، ركّز على أثر العميل لا التفاصيل التقنية، رمز تعبيري واحد لكل قسم كحد أقصى، اختم بطلب إجراء واضح، أقل من 280 كلمة.
```

### Prompt 4: Generic Growth Data → Report (swap GitHub for any source)

```
You are a growth analyst. Language: {{ $json.language }}.

Turn this week's business data into a GROWTH REPORT (3 sections):

1. **Shipped / Sold / Published** — what moved this week, with numbers
2. **Momentum** — trend vs last week, what drove it
3. **Next bets** — top 3 actions for next week, with expected impact

Data (last 7 days, from {{ $json.sourceName }}):
{{ JSON.stringify($json, null, 2) }}

Rules: numbers first, narrative second. One emoji per section max. End with one CTA. <280 words.
```

---

## Source Field Map — How to Swap GitHub for Any API (10-minute change)

Keep nodes 1-2 and 6-8 untouched. Replace nodes 3-5 only:

| Original Node | Replace With | Example URL | Key Fields to Keep |
|---|---|---|---|
| GitHub - Commits | Shopify Orders | `GET /admin/api/2024-01/orders.json?created_at_min={{ $now.minus({weeks:1}).toISO() }}` | `orders[]`, `total_price`, `created_at` |
| GitHub - Closed Issues | GA4 Report | `POST /v1beta/properties/{id}:runReport` (dateRange `7DaysAgo`..`today`) | `rows[]`, `metricValues`, `dimensionValues` |
| GitHub - Merged PRs | Meta Ads Insights | `GET /act_{id}/insights?date_preset=last_7d&fields=spend,impressions,actions` | `data[]`, `spend`, `actions` |
| Any | Notion DB Query | `POST /v1/databases/{id}/query` + filter `last_edited_time >= 7d ago` | `results[]`, `properties` |
| Any | Airtable List | `GET /v0/{base}/{table}?filterByFormula=DATETIME_DIFF(TODAY(),{Date},'days')<=7` | `records[]`, `fields` |

**After swap:** keep `Aggregate (Merge)` as-is — it will merge whatever 3 JSON arrays you feed it. Update the Claude prompt to reference `sourceName` (e.g. "Shopify", "GA4") so the narrative matches.

**Auth per source:** Create the matching credential in n8n (Shopify OAuth, Google OAuth for GA4, Meta access token, Notion token, Airtable PAT) — same pattern as `githubApi`.

---

## Branded Webhook Payload Template (for Slack/Discord/Email)

Replace the current `Deliver - Discord/Slack Webhook` body with:

```json
{
  "username": "Growth Report Bot",
  "embeds": [{
    "title": "📈 Weekly Growth Report — {{ $now.minus({weeks:1}).toFormat('dd LLL') }} → {{ $now.toFormat('dd LLL yyyy') }}",
    "description": "{{ $json.content[0].text }}",
    "color": 5763719,
    "footer": { "text": "Source: {{ $('Set Config').item.json.githubRepo || $json.sourceName }} · Next report Fri 5pm KSA" },
    "timestamp": "{{ $now.toISO() }}"
  }]
}
```

For **email** (n8n Email node): Subject `Weekly Growth Report — {{ $now.toFormat('dd LLL yyyy') }}`, HTML body = same `content[0].text` wrapped in your branded HTML shell.

---

## 1-Page PDF Report Shell (HTML → PDF via n8n)

If the client wants a PDF, add after Claude node: **HTML → PDF** (n8n community node `n8n-nodes-pdf` or HTTP Request to `https://api.pdfshift.io/v3/convert`):

```html
<!doctype html><html lang="en"><meta charset="utf-8">
<style>
  body{font-family:Inter,system-ui,sans-serif;max-width:720px;margin:40px auto;color:#111}
  h1{font-size:22px;border-bottom:2px solid #111;padding-bottom:8px}
  h2{font-size:14px;text-transform:uppercase;letter-spacing:.08em;color:#555;margin-top:28px}
  p{line-height:1.6;font-size:14px}
  .meta{color:#888;font-size:12px}
  .cta{margin-top:32px;padding:16px;background:#f4f4f5;border-radius:8px}
</style>
<h1>Weekly Growth Report</h1>
<p class="meta">{{ $json.githubRepo || $json.sourceName }} · {{ $now.minus({weeks:1}).toFormat('dd LLL') }} → {{ $now.toFormat('dd LLL yyyy') }}</p>
<div>{{ $json.content[0].text }}</div>
<div class="cta"><strong>Next step:</strong> Reply to this email or ping on Slack to approve next week's bets.</div>
</html>
```

---

## Pricing Ladder for This Template (when sold standalone)

- **Template only (prompts + field map):** $19 — Gumroad, instant download
- **Workflow + Growth prompts bundle:** $29 — same as base workflow, prompts included (recommended — just bundle it)
- **Done-for-you Growth Report setup:** $150-350 — Fiverr/Upwork, includes source swap + branded delivery + PDF

**Recommendation:** Don't list separately at first — bundle prompts into the $29 Gumroad product as "Bonus: Growth Report prompts (EN/FR/AR)" — increases perceived value, zero extra work.

---

## Checklist — Ship Growth Report Variant

- [ ] Copy one Claude prompt above into the Claude node (replace existing `messages` value)
- [ ] (If swapping source) Replace nodes 3-5 per field map, test with manual run
- [ ] Update Set Config `language` to `AR` for Saudi clients
- [ ] Replace webhook body with branded embed template (or Email node)
- [ ] (Optional) Add HTML→PDF node for PDF delivery
- [ ] Screenshot new execution → add as 2nd Gumroad image ("Growth Report mode")
- [ ] Update Gumroad description: add "Bonus: Growth Report prompts (EN/FR/AR) + source field map" bullet

---

## Trust Footer (same as other products)

Al Rajhi IBAN: SA0980000622608016010728 · STC Pay: 0574450435 · FL-153074569 · myn8n-automation.org/preview · VPS sf-vps
