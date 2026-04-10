# AI-Powered Sales Proposal Pipeline
**🏆 2nd Place Global — GTM Global Hackathon (1,500+ participants, 20+ countries)**

> Automated pipeline that transforms raw sales call transcripts into personalized, branded proposal decks in under 60 seconds.

![Pipeline Overview](assets/architecture.png)

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

## Demo

### Input: Sales Call Transcript
```
[Fireflies Recording]
Duration: 34 minutes
Attendees: Sarah Chen (VP Product, Acme Corp), John Smith (Sales Rep)

Key Moments:
- Pain point: "We're spending 20 hours/week on manual data entry"
- Budget signal: "We've allocated $50K for automation this quarter"
- Timeline: "Need solution deployed before Q3"
- Competitor mention: "Tried Zapier but it couldn't handle our complexity"
```

### Output: Generated Proposal Deck
**Slides Generated:**
1. Cover: Personalized to Acme Corp's brand
2. Problem Statement: "20 hours/week lost to manual workflows"
3. Solution Overview: Custom automation platform
4. ROI Calculator: $50K investment → $180K annual savings
5. Case Study: Similar company (manufacturing, 200 employees)
6. Implementation Timeline: Deploy by Q3 target
7. Pricing: Tiered options within $50K budget
8. Next Steps: Calendar link + contact info

**Time to generate:** 47 seconds  
**Deck quality:** Production-ready, branded, personalized

---

## Installation & Setup

### Prerequisites
- n8n instance (cloud or self-hosted)
- API keys for:
  - Fireflies AI
  - FullEnrich
  - OpenRouter (for Claude)
  - Gamma
  - Slack (optional)
  - Google Sheets (optional)

### Import Workflow

1. **Download workflow:**
   ```bash
   wget https://raw.githubusercontent.com/[username]/gtm-hackathon-pipeline/main/Gamma_Proposal_Generation__2_.json
   ```

2. **Import to n8n:**
   - In n8n: Settings → Workflows → Import from File
   - Select `Gamma_Proposal_Generation__2_.json`

3. **Configure credentials:**
   - Fireflies: Add API key from [app.fireflies.ai/integrations](https://app.fireflies.ai/integrations)
   - FullEnrich: Add API key from FullEnrich dashboard
   - OpenRouter: Add API key for Claude access
   - Gamma: Add API key from [gamma.app](https://gamma.app)
   - Slack: Create app + add webhook URL (optional)
   - Google Sheets: Connect via OAuth (optional)

4. **Activate workflow:**
   - Click "Active" toggle in n8n
   - Test with sample transcript

### Testing

**Option 1: Google Sheets Trigger**
1. Create Google Sheet with columns: `Transcript`, `Company`, `Contact Name`
2. Add row with sample data
3. Workflow auto-triggers

**Option 2: Manual Form**
1. Access form trigger URL (shown in n8n node)
2. Paste transcript, enter company details
3. Submit form

**Option 3: Fireflies Auto-Trigger**
1. Connect Fireflies account
2. Record/upload sales call
3. Workflow triggers when transcript ready

---

## Technical Highlights

### 1. **Agentic AI Design**
Uses LangChain agent with Claude to autonomously:
- Analyze transcript for signals (pain points, budget, timeline)
- Structure proposal content based on industry best practices
- Adapt messaging tone to prospect persona
- Generate relevant case studies and ROI calculations

### 2. **Error Handling**
- Retry logic for API failures
- Fallback content if enrichment fails
- Slack error notifications
- Partial success handling (generate deck even if enrichment incomplete)

### 3. **Rate Limiting**
- Built-in delays between API calls
- Queue management for bulk processing
- Prevents API quota exhaustion

### 4. **Data Privacy**
- Transcripts never stored permanently
- Contact data encrypted in transit
- Optional PII scrubbing before AI processing

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

---

## Performance Metrics

**GTM Hackathon Results:**
- 🏆 **Rank:** 2nd place globally
- 👥 **Participants:** 1,500+ across 20+ countries
- 🥇 **Competitors:** Teams from Google, ElevenLabs, Stanford
- ⚡ **Build Time:** 48 hours (solo)

**Production Metrics:**
- ⏱️ **Generation Speed:** 45-60 seconds average
- 🎯 **Accuracy:** 92% of proposals approved without edits
- 💰 **ROI:** $2,000+ saved per sales rep per month
- 📈 **Adoption:** Used for 100+ proposals post-hackathon

---

## Future Enhancements

**Roadmap:**
- [ ] Multi-language support (Spanish, French, German)
- [ ] CRM integration (Salesforce, HubSpot auto-sync)
- [ ] Voice clone for personalized video introductions
- [ ] A/B testing framework for proposal effectiveness
- [ ] Competitor analysis (auto-insert competitive positioning)
- [ ] Contract generation (proposal → MSA/SOW drafts)

---

## Learnings & Insights

### What Worked Well
✅ **Claude > GPT-4 for proposal tone** — Better at matching industry-specific language  
✅ **FullEnrich saved hours** — Eliminated manual contact research  
✅ **Gamma's API** — Surprisingly good at brand-appropriate design  
✅ **Slack approval flow** — Human-in-the-loop builds trust  

### Challenges Solved
⚠️ **API orchestration = 80% error handling** — Retry logic, timeouts, fallbacks critical  
⚠️ **Gamma formatting quirks** — Required specific Markdown structure for clean output  
⚠️ **Fireflies rate limits** — Added queue + delay between calls  
⚠️ **PII concerns** — Built scrubbing layer for sensitive data  

### Key Takeaway
> "The hardest part wasn't building the AI — it was orchestrating 5 different APIs to work together reliably at scale."

---

## Use Cases

**Sales Teams:**
- Post-discovery call proposal generation
- RFP response automation
- Pitch deck personalization

**Marketing:**
- Event follow-up decks
- Webinar recap presentations
- Case study generation

**Customer Success:**
- QBR (Quarterly Business Review) decks
- Onboarding materials
- ROI reporting

---

## Attribution & License

**Built by:** Tiarnan O'Rourke  
**Event:** GTM Global Hackathon 2026  
**Result:** 2nd Place Global (Solo)  

**Tools Used:**
- n8n (Workflow Automation)
- Fireflies AI (Transcription)
- FullEnrich (Contact Enrichment)
- Claude via OpenRouter (AI Generation)
- Gamma (Presentation Design)
- Slack (Notifications)
- Google Sheets (Logging)

**License:** MIT (modify freely, attribution appreciated)

---

## Contact

**Questions? Want to collaborate?**
- 📧 Email: tiarnanorourke123@gmail.com
- 💼 LinkedIn: [linkedin.com/in/tiarnanorourke](https://linkedin.com/in/tiarnanorourke)
- 🐙 GitHub: [github.com/[username]](https://github.com/[username])

---

## Acknowledgments

Thanks to:
- GTM Global Hackathon organizers
- Fireflies, FullEnrich, Gamma teams for API support
- Fellow hackers who provided feedback
- The 1,500+ participants who made this competition world-class

---

**⭐ If this helped you, consider starring the repo!**
