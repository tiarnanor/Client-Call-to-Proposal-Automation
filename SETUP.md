# Setup Guide — GTM Sales Proposal Pipeline

## Quick Start (5 minutes)

### Step 1: Prerequisites

**Required:**
- n8n instance ([n8n.io/cloud](https://n8n.io/cloud) or self-hosted)
- Fireflies AI account ([fireflies.ai](https://fireflies.ai))
- FullEnrich account ([fullenrich.com](https://fullenrich.com))
- OpenRouter account ([openrouter.ai](https://openrouter.ai)) for Claude access
- Gamma account ([gamma.app](https://gamma.app))

**Optional:**
- Slack workspace (for notifications)
- Google account (for Sheets logging)

---

## Step 2: Get API Keys

### Fireflies AI
1. Login to [app.fireflies.ai](https://app.fireflies.ai)
2. Go to Integrations → API
3. Generate new API key
4. Copy key (starts with `ff_...`)

### FullEnrich
1. Login to FullEnrich dashboard
2. Navigate to Settings → API Keys
3. Generate new key
4. Copy key

### OpenRouter (for Claude)
1. Sign up at [openrouter.ai](https://openrouter.ai)
2. Add credits ($5 minimum)
3. Generate API key from dashboard
4. Copy key (starts with `sk-or-...`)

### Gamma
1. Login to [gamma.app](https://gamma.app)
2. Go to Settings → API
3. Generate key
4. Copy key

### Slack (Optional)
1. Create new Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Enable Incoming Webhooks
3. Add webhook to channel
4. Copy webhook URL

### Google Sheets (Optional)
1. Will authenticate via OAuth in n8n
2. No separate key needed

---

## Step 3: Import Workflow to n8n

### Download Workflow
```bash
# Clone this repo
git clone https://github.com/[username]/gtm-hackathon-pipeline.git
cd gtm-hackathon-pipeline

# Or download directly
wget https://raw.githubusercontent.com/[username]/gtm-hackathon-pipeline/main/Gamma_Proposal_Generation__2_.json
```

### Import to n8n
1. Open n8n instance
2. Click hamburger menu (☰) → **Workflows**
3. Click **Import from File**
4. Select `Gamma_Proposal_Generation__2_.json`
5. Click **Import**

---

## Step 4: Configure Credentials

### Add Fireflies Credential
1. In workflow, click any Fireflies node
2. Click **Select Credential** → **Create New**
3. Name: `Fireflies API`
4. Paste API key from Step 2
5. Click **Save**

### Add FullEnrich Credential
1. Click FullEnrich node
2. Click **Select Credential** → **Create New**
3. Name: `FullEnrich API`
4. Paste API key
5. Click **Save**

### Add OpenRouter Credential
1. Click "OpenRouter" node
2. Click **Select Credential** → **Create New**
3. Name: `OpenRouter (Claude)`
4. Paste API key
5. **Model:** Select `anthropic/claude-3.5-sonnet`
6. Click **Save**

### Add Gamma Credential
1. Click "Gamma Deck" HTTP Request node
2. In Headers section, add:
   - **Key:** `Authorization`
   - **Value:** `Bearer YOUR_GAMMA_API_KEY`
3. Click **Save**

### Add Slack Credential (Optional)
1. Click "Notification" Slack node
2. Click **Select Credential** → **Create New**
3. Paste webhook URL from Step 2
4. Click **Save**

### Add Google Sheets Credential (Optional)
1. Click any Google Sheets node
2. Click **Select Credential** → **Create New**
3. Choose **OAuth2** authentication
4. Follow OAuth flow to connect Google account
5. Grant permissions
6. Click **Save**

---

## Step 5: Configure Google Sheet (Optional)

If using Google Sheets logging:

1. Create new Google Sheet named `Sales Proposals`
2. Add columns:
   - `Timestamp`
   - `Company`
   - `Contact Name`
   - `Contact Email`
   - `Transcript`
   - `Proposal Link`
   - `Status`
   - `Generated At`

3. In n8n workflow:
   - Open "Google Sheets Trigger" node
   - Select your sheet
   - Set trigger column: `Transcript`
   - Click **Save**

4. Update other Google Sheets nodes:
   - "Log Meeting" node → Set sheet ID
   - "Generated" node → Set sheet ID
   - "Generation Declined" node → Set sheet ID

---

## Step 6: Test Workflow

### Option 1: Manual Form Test
1. Click "On form submission" node
2. Copy the **Form URL** (shown in node)
3. Open URL in browser
4. Fill in:
   - Company: `Acme Corp`
   - Contact Name: `Sarah Chen`
   - Transcript: `[Paste sample transcript or use demo below]`
5. Submit form
6. Check n8n executions tab for results

### Option 2: Google Sheets Test
1. Open your `Sales Proposals` sheet
2. Add new row with sample data:
   - Company: `Test Corp`
   - Contact Name: `John Doe`
   - Transcript: `[Paste sample below]`
3. Workflow triggers automatically
4. Check executions in n8n

### Option 3: Fireflies Auto-Trigger
1. Record test sales call
2. Upload to Fireflies
3. Wait for transcription (2-5 minutes)
4. Workflow triggers when ready
5. Check n8n executions

---

## Sample Transcript (for testing)

```
Meeting: Discovery Call with Acme Corp
Duration: 28 minutes
Attendees: Sarah Chen (VP Product), Mike Johnson (CTO), John Smith (Sales Rep)

Transcript:
John: Thanks for taking the time today. I'd love to learn more about your current workflow challenges.

Sarah: We're spending about 20 hours per week on manual data entry across our sales and support teams. It's becoming unsustainable as we scale.

Mike: We've tried Zapier, but it couldn't handle the complexity of our integrations. We need something more robust.

John: What's your timeline for implementing a solution?

Sarah: Ideally before Q3. We've allocated $50,000 in budget for automation tools this quarter.

Mike: The key requirement is that it needs to integrate with our existing Salesforce, Zendesk, and Stripe setup.

John: Got it. And what would success look like for you?

Sarah: If we can reduce manual work by 80% and improve data accuracy, that would be a huge win. We're currently seeing about 15% error rates in manual transfers.
```

Expected output:
- ✅ Proposal deck generated in 45-60 seconds
- ✅ Includes: Pain points, solution overview, ROI calculation, pricing
- ✅ Personalized to Acme Corp
- ✅ Slack notification sent (if enabled)
- ✅ Logged in Google Sheets (if enabled)

---

## Step 7: Activate Workflow

1. In n8n workflow view, toggle **Active** switch (top right)
2. Status changes to "Active" (green)
3. Workflow now runs automatically on triggers

---

## Troubleshooting

### "API key invalid" errors
- Double-check keys have no extra spaces
- Verify keys are active in respective dashboards
- Regenerate key if needed

### Fireflies not triggering
- Check Fireflies node polling interval (default: 1 minute)
- Verify API key has transcript access permissions
- Ensure meeting is fully transcribed (not in progress)

### Gamma deck fails to generate
- Check Gamma API quota (may have monthly limits)
- Verify proposal content is under character limit
- Check Headers in HTTP Request node include `Authorization`

### FullEnrich returns no data
- Contact may not exist in FullEnrich database
- Try with well-known company contacts first
- Check API credits haven't been exhausted

### Claude generation errors
- Verify OpenRouter has credits
- Check model is set to `anthropic/claude-3.5-sonnet`
- Ensure transcript isn't too long (>100K tokens)

### Slack notifications not sending
- Verify webhook URL is correct
- Check Slack app permissions
- Test webhook with curl first

---

## Customization

### Change AI Model
1. Open "OpenRouter" node
2. Select different model:
   - `anthropic/claude-3-opus` (more creative)
   - `anthropic/claude-3-haiku` (faster, cheaper)
   - `openai/gpt-4-turbo` (alternative)

### Modify Proposal Template
1. Open "Proposal Generator Agent" node
2. Edit system prompt to change:
   - Tone (formal vs. casual)
   - Structure (slide order)
   - Content focus (ROI vs. features)
3. Click **Save**

### Add Custom Fields
1. Open Google Sheets nodes
2. Add new columns to read/write
3. Update "Set" nodes to include new fields
4. Modify AI prompt to use new data

### Change Notification Channel
1. Open "Notification" Slack node
2. Update webhook URL to different channel
3. Customize message text
4. Click **Save**

---

## Performance Optimization

### Reduce Execution Time
- Enable parallel processing for enrichment
- Use Claude Haiku for faster (less detailed) proposals
- Cache frequent lookups

### Handle High Volume
- Add rate limiting nodes
- Implement queue system
- Use batch processing for bulk runs

### Improve Accuracy
- Add human review step before sending
- A/B test different AI prompts
- Collect feedback loop data

---

## Security Best Practices

### API Keys
- Never commit keys to version control
- Use n8n credential storage (encrypted)
- Rotate keys quarterly

### Data Privacy
- Scrub PII before AI processing
- Enable transcript auto-deletion
- Use environment variables for sensitive configs

### Access Control
- Restrict n8n workflow edit permissions
- Use Slack private channels for notifications
- Limit Google Sheets sharing

---

## Support

**Issues?**
- Open GitHub issue with:
  - n8n version
  - Error message
  - Execution ID
  - Steps to reproduce

**Questions?**
- Check [Discussions](https://github.com/[username]/gtm-hackathon-pipeline/discussions)
- Email: tiarnanorourke123@gmail.com

**Feature Requests?**
- Open feature request issue
- Describe use case + expected behavior
- Include examples if possible

---

## Next Steps

Once workflow is running:
1. ✅ Test with 5-10 real sales calls
2. ✅ Gather feedback from sales team
3. ✅ Iterate on proposal template
4. ✅ Add custom fields for your industry
5. ✅ Scale to full team rollout

Happy automating! 🚀
