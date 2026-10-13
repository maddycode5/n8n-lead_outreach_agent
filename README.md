# AI Tech Lead Scraping & Outreach Automation

An end-to-end **n8n automation for discovering technology and SaaS companies, extracting decision-maker information using AI, enriching contact details, storing leads in Google Sheets, and sending personalized outreach emails through Gmail.**

The workflow combines **Firecrawl + Google Gemini + Google Sheets + Gmail + n8n** to automate the lead-generation and outreach pipeline.

> This repository contains the workflow structure only.  
> You must connect your own API keys, OAuth credentials, Google Sheet, and Gmail account.

---

##  What This Project Does

The automation performs the following pipeline:

```text
Daily Trigger
    |
    v
Search Technology / SaaS Leads
    |
    v
Split Search Results
    |
    v
Scrape Company Website
    |
    v
Extract Company & Contact Information
    |
    v
Check for Email
    |
    v
Search for Contact Page
    |
    v
Scrape Contact Page
    |
    v
Extract Contact Information
    |
    v
Save Lead to Google Sheets
    |
    v
Send Outreach Email
    |
    v
Mark Lead as Done
```

The goal is to reduce repetitive manual work involved in:

- Finding potential technology/SaaS companies
- Scraping company websites
- Finding founders, CTOs and technology decision makers
- Extracting emails and phone numbers
- Maintaining a lead database
- Sending outreach emails

---

#  Features

-  Automated web lead discovery
-  Website scraping using Firecrawl
-? AI-powered information extraction using Google Gemini
- Contact Founder / CTO / technology decision-maker extraction
- Email Email extraction
- Phone Phone number extraction
- Links LinkedIn URL extraction
- Company Company and industry extraction
- Update Contact-page recovery when the first scrape does not provide a contact
- Sheets Automatic Google Sheets lead storage
- Email Gmail outreach automation
- Schedule Scheduled daily execution
- Docker Docker-based local n8n deployment
- Security Credentials remain with the user instead of being stored in this repository

---

# Workflow Workflow Architecture

## High-Level Architecture

```text
Daily Trigger
    |
    v
Firecrawl Search
    |
    v
Split Results
    |
    v
Scrape Website
    |
    v
Gemini Extraction
    |
    v
Check for Email
    |
    v
Contact Search
    |
    v
Contact Scraping
    |
    v
Gemini Contact Extraction
    |
    v
Google Sheets
    |
    v
Gmail Outreach
    |
    v
Mark Lead Done
```

---

# Tech Stack Tech Stack

| Technology | Purpose |
|---|---|
| [n8n](https://n8n.io/) | Workflow automation |
| [Docker](https://www.docker.com/) | Local deployment |
| [Firecrawl](https://www.firecrawl.dev/) | Web search and website scraping |
| Google Gemini | AI-based information extraction |
| Google Sheets | Lead database |
| Gmail | Email outreach |
| GitHub | Workflow distribution |

---

# Prerequisites Prerequisites

Before starting, install:

### Required

- Docker Desktop
- Git
- A Google account
- Firecrawl account
- Google Cloud project

You do **not** need to install Python or Node.js to run the workflow.

---

# Docker 1. Run n8n Using Docker

Clone the repository:

```bash
git clone https://github.com/maddycode5/n8n-lead_outreach_agent.git
cd n8n-lead_outreach_agent
```

Start n8n:

```bash
docker compose up -d
```

Check that the container is running:

```bash
docker ps
```

You should see an n8n container running.

Open n8n:

```text
http://localhost:5678
```

### Stop n8n

```bash
docker compose down
```

### Restart n8n

```bash
docker compose restart
```

The included Docker Compose configuration uses a persistent Docker volume so that your n8n data survives container restarts.

---

# Setup 2. Install the Firecrawl n8n Community Node

This workflow uses the Firecrawl community node.

Inside n8n:

```text
Settings
   v
Community Nodes
   v
Install
```

Install:

```text
@mendable/n8n-nodes-firecrawl
```

After installation, restart n8n if required.

---

# API Key 3. Create Your Firecrawl API Key

Create an account on Firecrawl and generate an API key.

Inside n8n:

```text
Credentials
   v
Create Credential
   v
Firecrawl
```

Configure your Firecrawl credential using your own API key.

The workflow uses Firecrawl for:

- Web search
- Company website scraping
- Contact page discovery
- Contact page scraping

> Never place your Firecrawl API key directly inside the workflow JSON or commit it to GitHub.

---

# AI 4. Configure Google Gemini

The workflow uses Google Gemini for extracting structured information from scraped webpages.

Create a Gemini / Google AI credential in n8n and connect it to the Gemini Chat Model nodes.

The workflow contains two extraction stages:

### Company Extraction

Extracts information such as:

```text
company_name
website
founder_owner
cto_technology_decision_maker
job_title
email
phone
linkedin_url
location
industry
```

### Contact Extraction

The second extraction stage attempts to recover:

```text
contact_name
job_title
email
phone
linkedin_url
```

from the company's contact page.

---

# Sheets 5. Configure Google Sheets

Create a Google Sheet that will store your leads.

Recommended columns:

```text
company_name
website
contact_name
job_title
email
phone
linkedin_url
location
industry
```

Create a Google Sheets credential in n8n and authenticate using your Google account.

Then open:

```text
Save Lead to Sheet
```

and select your own:

- Google account
- Spreadsheet
- Sheet

The workflow will write extracted lead information into the sheet.

---

# Email 6. Configure Gmail

Create a Gmail credential in n8n and authenticate your Gmail account.

Open:

```text
Email Tech Lead
```

and connect your Gmail credential.

The workflow sends outreach emails using the email address extracted from the lead.

### Important

Before enabling automatic outreach, test the workflow using an email address that you control.

This allows you to verify:

- Email formatting
- Subject
- Recipient
- Extracted lead information
- Gmail authentication

---

# Security 7. Google OAuth Configuration

If you are running n8n locally and configuring Google OAuth manually, your redirect URL will generally be:

```text
http://localhost:5678/rest/oauth2-credential/callback
```

Add this URL to your Google OAuth application's authorized redirect URIs.

For a local development setup, make sure the Google OAuth application and test users are configured correctly.

---

# Import 8. Import the Workflow

The workflow is located at:

```text
workflows/lead-outreach.json
```

Inside n8n:

```text
Workflows
   v
Import from File
   v
Select workflows/lead-outreach.json
```

After importing, open the workflow and configure the credentials.

---

# Links 9. Connect Your Credentials

After importing the workflow, check every node that uses an external service.

You will need to connect your own credentials for:

```text
Firecrawl
Google Gemini
Google Sheets
Gmail
```

The workflow logic and expressions are already configured.

You should **not need to rebuild the workflow from scratch**.

---

# Testing 10. Test the Workflow

Before activating the workflow, test the nodes individually.

Recommended testing order:

```text
1. Search Tech Leads
2. Split Results
3. Scrape Lead Page
4. Gemini Lead Extraction
5. Contact Search
6. Contact Page Scraping
7. Gemini Contact Extraction
8. Google Sheets
9. Gmail
```

Verify that the extracted data contains:

```text
Company
Website
Contact
Job Title
Email
Phone
LinkedIn
Location
Industry
```

Then perform a complete end-to-end test.

---

# Schedule 11. Daily Automation

The included workflow uses a daily schedule.

Default configuration:

```text
Trigger Interval: Days
Days Between Triggers: 1
Trigger Hour: Midnight
Trigger Minute: 0
```

Therefore, the workflow is configured to run once every day.

For automatic execution:

1. Keep Docker running.
2. Keep the n8n container running.
3. Activate the workflow in n8n.

You can change the schedule according to your requirements.

---

# Customization 12. Customizing Lead Search

The Firecrawl search node contains the search query used for discovering leads.

You can modify the query according to your target market.

For example:

```text
Indian SaaS startups
```

or:

```text
Indian AI startups founders CTO
```

or:

```text
B2B SaaS companies technology decision makers
```

You can customize:

- Geography
- Industry
- Company size
- Technology
- Target customer
- Decision-maker type

---

# Project Structure Project Structure

```text
n8n-lead_outreach_agent/
|
|-- workflows/
|   `-- lead-outreach.json
|
|-- docker-compose.yml
|-- .gitignore
`-- README.md
```

### `workflows/lead-outreach.json`

The exported n8n workflow.

### `docker-compose.yml`

Docker configuration for running n8n locally.

### `.gitignore`

Prevents common credentials, secrets, local files and generated files from being committed.

### `README.md`

Project documentation and setup instructions.

---

# Security Security

**Never commit credentials to GitHub.**

Do not commit:

```text
.env
API keys
OAuth client secrets
Access tokens
Refresh tokens
Private keys
Credential JSON files
Passwords
```

Credentials should be configured inside your own n8n instance.

This repository is designed so that users can download/import the workflow while keeping their own credentials private.

If you accidentally expose an API key:

1. Revoke the exposed key immediately.
2. Generate a new key.
3. Remove the secret from the repository/history if necessary.
4. Update your n8n credential.

---

# WARNING: Responsible Use

This workflow is intended for legitimate lead-generation and business outreach.

When using automated scraping and email outreach:

- Respect website terms of service.
- Respect robots.txt and applicable scraping restrictions.
- Follow applicable privacy and data-protection laws.
- Do not collect unnecessary personal information.
- Do not send spam.
- Respect unsubscribe requests.
- Use appropriate email-sending limits.
- Verify that your outreach complies with applicable regulations.

You are responsible for how you configure and use this workflow.

---

# Troubleshooting Troubleshooting

## n8n is not opening

Check Docker:

```bash
docker ps
```

If the container is stopped:

```bash
docker compose up -d
```

Check logs:

```bash
docker compose logs -f n8n
```

---

## Port 5678 is already in use

Check which process is using port `5678`.

Alternatively, change the port mapping in:

```text
docker-compose.yml
```

For example:

```yaml
ports:
  - "5679:5678"
```

Then access:

```text
http://localhost:5679
```

---

## Firecrawl authentication error

Check:

- Firecrawl API key
- Firecrawl credential
- Internet connection
- Firecrawl account/API limits

Make sure the credential is connected to the Firecrawl nodes.

---

## Gemini authentication error

Check:

- Google/Gemini API credentials
- API access
- Selected Gemini model
- Credential connected to the Gemini Chat Model

---

## Google OAuth error

Check:

- Google Cloud project
- Required APIs
- OAuth consent configuration
- Test users
- Authorized redirect URI

For local n8n, the callback is:

```text
http://localhost:5678/rest/oauth2-credential/callback
```

---

## Google Sheets does not receive data

Check:

1. Google Sheets credential
2. Spreadsheet ID
3. Sheet name
4. Column names
5. Node execution data

Test the Google Sheets node independently before running the complete workflow.

---

# Update Updating the Project

If you modify the workflow locally:

```bash
git status
```

Add changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Update lead outreach workflow"
```

Push:

```bash
git push
```

---

# Contributing Contributing

Contributions are welcome.

You can:

- Improve the workflow
- Add new lead sources
- Improve extraction logic
- Add additional enrichment
- Improve documentation
- Report bugs
- Suggest new features

Fork the repository, make your changes, and open a pull request.

---

# License License

Add an appropriate open-source license before distributing this project for broad reuse.

---

# Star Support

If this project is useful, consider giving the repository a Star on GitHub.

---

## Built With

**n8n + Firecrawl + Google Gemini + Google Sheets + Gmail + Docker**


<!-- AUTO-WORKFLOW-DOCS:START -->
## Workflow Documentation

- **Workflow:** Tech Lead Outreach Agent
- **Workflow ID:** dS8IJWOMaHszEDQr
- **Node count:** 25
- **Connection count:** 27
- **Generated:** 2026-10-13 14:12:46

### Triggers

- Daily Scrape

### Nodes

- **Daily Scrape** - scheduleTrigger
- **Search Tech Leads** - @mendable/n8n-nodes-firecrawl.firecrawl
- **Split Results** - splitOut
- **Scrape Lead Page** - @mendable/n8n-nodes-firecrawl.firecrawl
- **Scrape Succeeded** - if
- **Has Email** - if
- **Prepare Lead Data** - set
- **Save Lead to Sheet** - googleSheets
- **Email Tech Lead** - gmail
- **Mark Lead Done** - googleSheets
- **Prepare Contact Search** - set
- **Find Contact Page** - @mendable/n8n-nodes-firecrawl.firecrawl
- **Contact URL Found?** - if
- **Scrape Contact Page** - @mendable/n8n-nodes-firecrawl.firecrawl
- **Contact Scrape Succeeded** - if
- **Has Contact Email** - if
- **Prepare Recovered Lead Data** - set
- **Google Gemini Chat Model** - @n8n/n8n-nodes-langchain.lmChatGoogleGemini
- **Information Extractor** - @n8n/n8n-nodes-langchain.informationExtractor
- **Google Gemini Chat Model1** - @n8n/n8n-nodes-langchain.lmChatGoogleGemini
- **Information Extractor2** - @n8n/n8n-nodes-langchain.informationExtractor
- **Prepare Duplicate Check** - set
- **Read Existing Lead** - googleSheets
- **Code in JavaScript** - code
- **Merge** - merge

<!-- AUTO-WORKFLOW-DOCS:END -->