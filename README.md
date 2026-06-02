# Equity Outlook Agent

Generates a four-paragraph institutional global equity outlook monthly, using live web research via the Anthropic API. Output is saved as a dated markdown file in `/output` and optionally emailed.

## Structure

```
equity-outlook/
├── .github/
│   └── workflows/
│       └── monthly_outlook.yml   # GitHub Actions scheduler
├── scripts/
│   └── generate_outlook.py       # Core generation script
├── output/                        # Generated outlooks saved here
├── requirements.txt
└── README.md
```

## Setup

### 1. Add your Anthropic API key as a GitHub Secret

In your repo: **Settings → Secrets and variables → Actions → New repository secret**

| Secret name | Value |
|---|---|
| `ANTHROPIC_API_KEY` | Your Anthropic API key |

### 2. (Optional) Add email secrets to receive the outlook by email

Uses Gmail or any SMTP provider. For Gmail, generate an [App Password](https://myaccount.google.com/apppasswords) — do not use your main account password.

| Secret name | Value |
|---|---|
| `SMTP_HOST` | `smtp.gmail.com` |
| `SMTP_PORT` | `587` |
| `SMTP_USER` | `your@gmail.com` |
| `SMTP_PASS` | Your Gmail App Password |
| `OUTLOOK_RECIPIENT` | Address to deliver to (can be same as SMTP_USER) |

If email secrets are not set, the script saves the file and skips the email step silently.

### 3. Push this repo to GitHub

The workflow triggers automatically on the **1st of every month at 7:00 AM ET**.

## Manual Run

**From GitHub Actions UI:**
1. Go to **Actions → Monthly Equity Outlook → Run workflow**
2. Optionally enter a month override (e.g. `June 2026`)
3. Click **Run workflow**

**From your local machine:**
```bash
pip install anthropic
export ANTHROPIC_API_KEY=your_key_here
python scripts/generate_outlook.py              # current month
python scripts/generate_outlook.py "June 2026"  # specific month
```

## Output

Each run saves a file to `/output/` named `YYYY-MM-equity-outlook.md`, for example:

```
output/2026-06-equity-outlook.md
```

The file is automatically committed back to the repo by the Actions workflow.

## Outlook Structure

Each outlook covers exactly four paragraphs:

1. **US macro backdrop** — Fed policy, inflation, rate expectations, liquidity, financial conditions
2. **Corporate earnings** — revision trends, margin outlook, sector leadership, AI cycle implications
3. **International context** — Europe and Asia growth, policy stance, geopolitics, relative valuation
4. **US small-cap outlook** — valuation, earnings recovery, rate sensitivity, positioning dynamics
