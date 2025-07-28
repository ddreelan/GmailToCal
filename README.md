````markdown
# GmailToCal 📅📬

Automatically scan your Gmail for FIFO/mechanical fitter job emails and schedule them into your Google Calendar.

## Features

- 🔐 Securely connects to Gmail and Google Calendar using OAuth2
- 📥 Filters recent emails for job opportunities
- 📆 Creates corresponding calendar events
- ⏱️ Can be scheduled to run automatically each day via GitHub Actions

---

## 🧪 Quickstart

### 1. Fork this Repository

Click the **"Fork"** button in the top-right of this GitHub page and clone it locally:

```bash
git clone https://github.com/YOUR_USERNAME/GmailToCal.git
cd GmailToCal
````

---

## 🔑 Setup API Keys

### 2. Google Cloud Project Setup

To authenticate with Gmail and Google Calendar APIs:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or use an existing one)
3. Enable these APIs:

   * Gmail API
   * Google Calendar API
4. Create OAuth 2.0 credentials:

   * Type: Desktop
   * Download the `credentials.json` file

Save the file to the root directory of this repo.

---

### 3. Create and Encode Your Token

The script expects a base64-encoded token stored in a GitHub secret.

You can generate it by running the script locally once:

```bash
python GmailToCalendar.ipynb  # Or run in Jupyter
```

Once authenticated, a `token.json` file will be created.

Encode it to base64:

```bash
base64 token.json > token.b64
```

Copy the contents of `token.b64`, then go to:

**GitHub → Repo → Settings → Secrets and Variables → Actions → New repository secret**

Create a new secret:

* Name: `GMAIL_API_TOKEN_BASE64`
* Value: *Paste base64 string from `token.b64`*

---

## ⚙️ Schedule the Script with GitHub Actions

### 4. Enable GitHub Actions

In the `.github/workflows/main.yml` file:

```yaml
on:
  schedule:
    - cron: '20 7 * * *'  # Every day at 07:20 UTC
  workflow_dispatch:       # Optional: allows manual runs from GitHub UI
```

🕒 **Note:** Adjust the cron expression to your desired time. For example:

* Perth time 15:20 = UTC 07:20 → use `'20 7 * * *'`

To schedule it at 8:00 AM Perth time:

```yaml
- cron: '0 0 * * *'  # 8:00 AM AWST = 00:00 UTC
```

> Use [crontab.guru](https://crontab.guru/) to generate expressions.

---

## 🐍 Running Locally

```bash
pip install -r requirements.txt
jupyter notebook
# Open and run GmailToCalendar.ipynb
```

---

## 📁 File Overview

* `GmailToCalendar.ipynb`: Main notebook that scans emails and schedules events
* `.github/workflows/main.yml`: GitHub Actions workflow file
* `credentials.json`: Your Google API credentials (not committed)
* `token.json`: Generated OAuth token (base64-encoded for GitHub Actions)

---

## 🧠 Tips

* You can filter by keyword, email sender, or subject line in the notebook
* Use `workflow_dispatch` to run the job manually from the GitHub UI
* Don't commit your raw `token.json` or `credentials.json` for security

---

## 📬 Example Email to Calendar Event Mapping

If an email subject contains:

> *Shutdown job - BHP - Port Hedland - 14th Sept*

It will create a calendar event titled:

> **Shutdown job - BHP - Port Hedland**
> Date: **14th September**
> Duration: **Default 12 hrs** (modifiable in code)

---

## 📜 License

MIT License

---

## 🙌 Acknowledgements

Made by Danny Dreelan — inspired by the needs of FIFO workers automating their workflow.