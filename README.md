# AI Tech Lead Scraping & Outreach Automation

An n8n automation for discovering technology and SaaS leads, scraping company and contact pages, extracting structured information with Google Gemini, saving leads to Google Sheets, and sending outreach emails through Gmail.

## Workflow

Daily Scrape
↓
Firecrawl Search
↓
Lead Page Scraping
↓
Gemini Lead Extraction
↓
Contact Page Search
↓
Contact Page Scraping
↓
Gemini Contact Extraction
↓
Google Sheets
↓
Gmail Outreach
↓
Mark Lead Done

## Tech Stack

- n8n
- Docker / Docker Compose
- Firecrawl
- Google Gemini
- Google Sheets API
- Gmail API
- Google OAuth 2.0

## Setup

### 1. Start n8n

```bash
docker compose up -d
