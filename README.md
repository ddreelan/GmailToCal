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
git clone https://github.com/ddreelan/GmailToCal.git
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

## 🗓️ Set Up Your Calendar ID

We recommend **creating a new calendar** to separate job events from your personal calendar.

1. Go to [Google Calendar](https://calendar.google.com/)
2. On the left sidebar, click the `+` next to **Other calendars** → **Create new calendar**
3. Name it something like `FIFO Jobs` and click **Create calendar**
4. Click on the calendar in the list → Go to **Settings**
5. Scroll down to **Integrate calendar**
6. Copy the **Calendar ID** (looks like `something@group.calendar.google.com`)

Create a GitHub secret for it:

* Name: `CALENDAR_ID`
* Value: *Paste the Calendar ID*

---

## 🧠 Add OpenAI for Smarter Parsing (Optional)

The notebook supports GPT-powered parsing for job dates, locations, and role details.

To use it:

1. Go to [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)
2. Log in and click **Create new secret key**
3. Copy the key (starts with `sk-...`)
4. In your GitHub repo, go to:

   **Settings → Secrets and Variables → Actions → New repository secret**

Create a new secret:

* Name: `OPENAI_API_KEY`
* Value: *Paste your OpenAI key*

> You can skip this if you're manually extracting job details without GPT.

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

## 📜 License

MIT License

---

## 🙌 Acknowledgements

Made by Danny Dreelan — I did the nerdy bits so the grease monkeys don’t have to. You’re welcome.
