# AI-Powered Sales Proposal Pipeline

> Automated pipeline that transforms raw sales call transcripts into personalized, branded proposal decks in under 60 seconds.


## The Problem

Sales teams waste 45+ minutes per discovery call manually:
- Reviewing meeting recordings for key insights
- Researching prospect company details and contacts
- Drafting personalized proposal content
- Designing branded decks that match client industry/tone
- **Result:** Slow follow-up times, missed deals, rep burnout

## The Solution

End-to-end agentic AI pipeline that automates the entire proposal generation workflow:

```
Sales Call → Fireflies Transcript → AI Analysis → Contact Enrichment → Branded Proposal Deck
```

**Impact:** 45 minutes → 60 seconds (45x faster)

---

## Architecture

### Pipeline Flow

1. **Trigger** — Google Sheets row added OR Fireflies new transcript OR manual form submission
2. **Fireflies AI** — Extracts transcript, key moments, attendee list, meeting metadata
3. **FullEnrich** — Enriches attendee contacts (emails, LinkedIn, company data)
4. **Claude AI Agent** — Analyzes transcript + enriched data to generate personalized proposal content
5. **Gamma API** — Creates branded presentation deck with custom design matching prospect's industry
6. **Slack Notification** — Sends completion alert with proposal link to sales rep
7. **Google Sheets Log** — Records all generated proposals for tracking/analytics

### Tech Stack

| Component | Tool | Purpose |
|-----------|------|---------|
| **Orchestration** | n8n | Workflow automation & API coordination |
| **Transcription** | Fireflies AI | Call recording → structured transcript + key moments |
| **Enrichment** | FullEnrich | Contact data (emails, job titles, company info) |
| **AI Generation** | Claude (via OpenRouter) | Proposal content + personalization |
| **Deck Creation** | Gamma | Branded presentation generation |
| **Notifications** | Slack | Real-time alerts to sales team |
| **Logging** | Google Sheets | Proposal tracking + analytics |

---

## Key Features

### 1. **Multi-Trigger Support**
- ✅ Fireflies automatic transcript detection
- ✅ Google Sheets row addition (manual input)
- ✅ Web form submission
- ✅ Webhook API endpoint

### 2. **Intelligent Proposal Generation**
- Extracts pain points, budget signals, decision timeline from transcript
- Personalizes messaging based on prospect's industry, company size, role
- Adapts tone (technical vs. executive, formal vs. casual)
- Includes relevant case studies and ROI calculations

### 3. **Contact Enrichment**
- Pulls attendee emails, LinkedIn profiles, job titles
- Enriches company data (headcount, funding, tech stack)
- Identifies decision-makers vs. influencers

### 4. **Branded Deck Automation**
- Auto-generates presentation matching prospect's industry aesthetic
- Customizes color schemes, imagery, layout
- Includes company logo, contact info, next steps

### 5. **User Approval Flow**
- Slack notification with proposal preview
- "Approve" or "Decline" buttons
- Optional human review before sending to prospect

### 6. **Analytics & Tracking**
- Logs all proposals in Google Sheets
- Tracks generation time, approval status, prospect info
- Enables performance analysis (conversion rates, deck effectiveness)

---

## Workflow Nodes Explained

| Node | Function |
|------|----------|
| **Google Sheets Trigger** | Detects new rows in tracking sheet |
| **Fireflies - Get Transcripts** | Fetches latest meeting recordings |
| **Fireflies - Get Info** | Extracts transcript text, attendees, metadata |
| **Split Out** | Separates attendee list for enrichment |
| **FullEnrich - Start Enrichment** | Pulls contact emails, LinkedIn, company data |
| **Clean Up** | Normalizes data format for AI agent |
| **Proposal Generator Agent** | Claude analyzes + generates proposal content |
| **Gamma Deck** | Creates branded presentation from AI content |
| **Slack Notification** | Alerts sales rep with preview link |
| **If (Approval)** | Routes to approval flow if enabled |
| **Google Sheets - Log Meeting** | Records proposal metadata |
| **Google Sheets - Generated** | Marks proposal as complete |



**⭐ If this helped you, consider starring the repo!**
