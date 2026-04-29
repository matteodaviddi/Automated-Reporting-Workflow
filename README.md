# 📊 Automated Reporting Workflow
### Make + Google Sheets + OpenAI + QuickChart + Gmail

An automated reporting system that reads data from Google Sheets, analyzes it with OpenAI GPT-4, generates professional HTML reports with charts via QuickChart, and delivers them via email — fully automated with Make.

---

## 🚀 What it does

- **Reads** data automatically from Google Sheets every day
- **Aggregates** all rows into a single structured text
- **Analyzes** the data with OpenAI GPT-4, generating a report with:
  - Introductory message
  - Executive KPI cards (Total Revenue, Units Sold, Best Product)
  - Horizontal bar chart — Revenue by Category
  - Histogram — Units Sold by Month
  - Line chart — Monthly Revenue Trend
  - Strategic Recommendations box
  - Closing message
- **Delivers** the report as a professional HTML email via Gmail

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [Make](https://make.com) | Workflow automation |
| Google Sheets | Data source + calculated KPIs |
| OpenAI API (GPT-4o) | AI-powered report generation |
| [QuickChart](https://quickchart.io) | Chart image generation |
| Gmail | HTML email delivery |

---

## 📐 Workflow Architecture

```
Schedule (Daily trigger)
        ↓
Google Sheets — Search Rows
        ↓
Tools — Text Aggregator
        ↓
OpenAI — Generate Completion (GPT-4o)
        ↓
Gmail — Send HTML Email
```

---

## 📋 Google Sheets Structure

### Main data table (tab: `Vendite`, columns A-F)

| Mese | Categoria | Prodotto | Unità vendute | Fatturato (€) | Trend |
|------|-----------|----------|---------------|---------------|-------|
| Gennaio | Elettronica | Cuffie BT Pro | 142 | 14200 | +12% |
| ... | ... | ... | ... | ... | ... |

### KPI summary (columns H-I)

| H | I |
|---|---|
| Fatturato Totale | `=SOMMA(E2:E13)` |
| Unità Totali | `=SOMMA(D2:D13)` |
| Fatturato Medio | `=MEDIA(E2:E13)` |
| Prodotto Migliore | `=INDICE(C2:C13;CONFRONTA(MAX(E2:E13);E2:E13;0))` |
| Mese Migliore | `=INDICE(A2:A13;CONFRONTA(MAX(E2:E13);E2:E13;0))` |
| Prodotto Peggiore | `=INDICE(C2:C13;CONFRONTA(MIN(E2:E13);E2:E13;0))` |

---

## ⚙️ Setup Guide

### Prerequisites
- [Make account](https://make.com) (free tier works)
- Google account with Google Sheets access
- [OpenAI API key](https://platform.openai.com)
- Gmail account

### Steps

1. **Copy the sample data** into a new Google Sheet with the structure above
2. **Import the blueprint** on Make:
   - Go to Scenarios → Create new scenario
   - Click ⋮ → Import Blueprint → upload `blueprint.json`
3. **Connect your accounts** in Make:
   - Google Sheets → authorize with your Google account
   - OpenAI → paste your API key
   - Gmail → authorize with your Gmail account
4. **Update the Google Sheets module** with your spreadsheet and sheet name
5. **Run once** to test → check your inbox
6. **Activate** the scenario toggle for daily automatic delivery

---

## 🤖 Key Design Decisions

**Why Text Aggregator?**
Google Sheets Search Rows returns one bundle per row (12 rows = 12 bundles). The Text Aggregator collapses them into a single text block so OpenAI receives one message and Gmail sends one email.

**Why QuickChart?**
OpenAI's HTML generation is unreliable for complex SVG charts. QuickChart generates charts via URL and returns stable PNG images that render correctly in all email clients.

**Why replace() in Gmail?**
OpenAI occasionally adds markdown code fences (` ```html `) despite prompt instructions. The replace formula in Gmail's Content field strips them before sending.

---

## ⚠️ Common Issues

| Error | Cause | Fix |
|-------|-------|-----|
| `Unable to parse range` | Sheet name typed manually | Always select from dropdown |
| Multiple emails sent | Text Aggregator misconfigured | Set Source Module to Search Rows |
| Charts not visible | Gmail blocks external images | Gmail Settings → Always show external images |
| ` ```html ` at top of email | OpenAI adds markdown | Use replace() formula in Gmail Content field |
| References non-existing module | Variable points to deleted module | Reselect variable from panel |

---

## 🔧 Adapting for Clients

1. **Clone** the scenario (⋮ → Clone) — never modify the original
2. **Update Search Rows** with client's spreadsheet and sheet name
3. **Update Text Aggregator** variables to match client's column names
4. **Update OpenAI prompt** with client's KPIs, products, and preferred tone
5. **Update Gmail** with client's email and company name in subject

Each customization takes ~15-20 minutes once you have the base scenario ready.

---

## 📄 License

MIT — feel free to use, modify and share.

---

## 👤 Author

**Matteo Daviddi**
AI Automation Developer
[LinkedIn](www.linkedin.com/in/matteodaviddi) · [GitHub](github.com/matteodaviddi)
