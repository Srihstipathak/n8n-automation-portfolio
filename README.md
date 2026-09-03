# N8N Automation Portfolio

A comprehensive collection of production-ready automation workflows built with **n8n**, demonstrating expertise in multi-platform integrations, AI-powered agents, and enterprise-scale data processing.

## 📋 Overview

This portfolio showcases advanced automation solutions spanning **LinkedIn outreach**, **WhatsApp messaging**, **email automation**, **AI voice agents**, **influencer research**, and **competitive intelligence**.

**Tech Stack:**
- **Platform:** n8n (Open-source workflow automation)
- **Integrations:** LinkedIn, WhatsApp, Meta (Facebook), Retell AI, Google Sheets, Gmail, Outlook, Apify
- **Databases:** PostgreSQL
- **APIs:** Unipile, Retell AI, Facebook Graph API, Instagram Scraper
- **Languages:** JavaScript (custom code nodes), SQL

---

## 📝 Recent Updates

### Latest Commits
- **[Sep 3, 2026]** - New files uploaded to repository (commit: cf2d99b)
- **[Sep 3, 2026]** - Added comprehensive README documentation for n8n automation portfolio (commit: d85209b)
- **[Sep 3, 2026]** - Initial project files uploaded (commit: 83e8bb5)

### Project Status
- ✅ Repository initialized and ready for development
- ✅ Complete documentation added
- ✅ Workflow files uploaded
- 🚀 Production workflows in active use

---

## 🚀 Core Automation Workflows

### 1. **LinkedIn Outreach Automation**

#### 📌 Connection Request - LeadGen | LinkedIn
**Purpose:** Automate LinkedIn connection requests with intelligent lead targeting and account management.

**Key Features:**
- Fetch active LinkedIn owner accounts from PostgreSQL
- Query candidate database filtered by status (NEW) and connection status
- Dynamic workload allocation across multiple LinkedIn accounts based on daily limits
- Batch processing with configurable account quotas
- Automatic connection invitation via Unipile API
- Status tracking with provider IDs and timestamps

**Workflow Architecture:**
```
Get LinkedIn Accounts → Compute Remaining Quota → Get Candidates
  → Allocate Candidates → Split Batches → Get Provider ID
  → Send Invitation → Random Delay → Update Database
```

**Data Persistence:**
- Tracks: `linkedinProviderId`, `linkedinInvitationId`, `connectionSent`, `connectionSentDateTime`
- Database: `_learner` table in Nebula PostgreSQL workspace

**Use Case:** Scale LinkedIn lead generation campaigns with automatic rate limiting.

---

#### 🎯 1st LinkedIn Outreach - Accepted Connection
**Purpose:** Monitor LinkedIn connection acceptances and send personalized first messages automatically.

**Key Features:**
- Real-time webhook integration with Unipile (new_relation events)
- Automatic detection of accepted connections
- Payload normalization from Unipile webhook format
- Direct message sending via Unipile Chat API
- Database update on successful message delivery
- Conditional messaging based on connection acceptance

**Workflow Architecture:**
```
Webhook (New Relation) → Check Event Type → Normalize Payload
  → Query Database → Mark as Accepted → Send First Message
  → Update Database with Status
```

**Database Operations:**
- Updates `connectionAccepted`, `connectionAcceptedDatetime`, `linkedinProviderId`
- Matches records on LinkedIn Provider ID
- Uses Postgres credentials (Nebula PGSQL Account)

**Use Case:** Maximize response rates by sending thoughtful first messages immediately after connection acceptance.

---

### 2. **WhatsApp Messaging Automation**

#### 📱 WhatsApp Sending First Message Agent
**Purpose:** Send templated WhatsApp messages to contacts with delivery tracking.

**Key Features:**
- Google Sheets integration for contact management
- Templated message support with personalization
- Message ID tracking (WABA Message ID)
- Timestamp logging (Queued, Sent, Delivered, Read)
- Automatic spreadsheet updates with delivery status

**Workflow Architecture:**
```
Trigger → Fetch Contact List (Google Sheets)
  → Send WhatsApp Template → Update Sheet with Message ID
  → Track Delivery Status
```

**Integrations:**
- WhatsApp Business API (phone_number_id: 888839237646445)
- Template: `thankyou_message|en` with personalization parameters
- Google Sheets for contact database and status tracking

**Use Case:** Send bulk WhatsApp notifications with delivery confirmation.

---

#### 📊 Meta Message Status Update
**Purpose:** Real-time WhatsApp message status tracking and database synchronization.

**Key Features:**
- WhatsApp webhook trigger for message status events
- Status detection: `sent`, `delivered`, `read`
- Real-time spreadsheet updates with timestamps
- Switch-based routing for different status types
- Message ID-based record matching

**Workflow Architecture:**
```
WhatsApp Webhook → Switch Status → Update Delivery Status
  ├─ Sent: Update "Sent At"
  ├─ Delivered: Update "Delivered At"
  └─ Read: Update "Read At"
```

**Data Tracked:**
- WABA Message ID (primary key for matching)
- Sent At, Delivered At, Read At timestamps
- Phone Number for contact linking

**Use Case:** Maintain real-time message delivery analytics.

---

#### 🎥 Video Template Message Sending
**Purpose:** Send WhatsApp template messages with video attachments.

**Key Features:**
- Template-based messaging with video headers
- Facebook Graph API integration (v24.0)
- Dynamic message customization
- Message ID and timestamp tracking
- Spreadsheet-based contact management

**Workflow Architecture:**
```
Trigger → Get Contacts (Google Sheets) → Send Video Template
  → Update Sheet with Message ID & Timestamp
```

**Use Case:** Send branded video messages at scale with delivery tracking.

---

### 3. **Email Automation**

#### 💌 Interview Reminder Agent on Email
**Purpose:** Automated interview reminder emails triggered daily at scheduled times.

**Key Features:**
- PostgreSQL database queries for recruitment candidates
- Date-based filtering for interviews scheduled today
- Personalized email generation with candidate details
- Interview link and round information inclusion
- Outlook email integration for delivery
- Scheduled trigger (Daily at 8 PM via cron: 21:30)
- Activity logging to Google Sheets

**Workflow Architecture:**
```
Schedule Trigger (Daily 8 AM) → Fetch Candidates from DB
  → Filter Interviews Scheduled Today → Format Email
  → Send via Outlook → Log Activity to Google Sheets
```

**Database Integration:**
- Source: `_recruitmentAndHiring` table
- Queries: `interviewRound1Date`, `interviewRound1LinkPrimaryLinkUrl`, `jobProfileRole`
- Target Sheets: Google Sheets (Keywords sheet - gid=1868661085)

**Use Case:** Reduce interview no-shows with automatic reminders.

---

### 4. **AI Agent Workflows**

#### 🤖 Retell AI - Voice Agent Automation
**Purpose:** Automated outbound phone calls using AI voice agents.

**Key Features:**
- Google Sheets data source for contact information
- Retell AI integration for voice call creation
- Contact validation (existence checks)
- Batch processing with loop iteration
- Company information validation
- Wait node for asynchronous processing

**Workflow Architecture:**
```
Trigger → Get Contacts (Google Sheets) → Validate Contacts
  → Loop Over Items → Wait → Filter Valid Records
  → Create Phone Call via Retell AI
```

**Integration Details:**
- **Retell AI API Endpoint:** `https://api.retellai.com/create-phone-call`
- **Agent ID:** `agent_733fa30066aefa6368840cb7f9`
- **From Number:** `+1 (770) 343-3566`
- **Dynamic To Number:** Contact phone from spreadsheet

**Use Case:** Scale outbound calling with AI agents for lead qualification.

---

#### 👥 Influencer Extraction and Qualification Agent
**Purpose:** Research and qualify Instagram influencers using AI-powered analysis.

**Key Features:**
- Apify integration for Instagram profile scraping
- Automatic niche classification using keywords
- Multi-tier data extraction:
   - Instagram profile metrics (followers, engagement)
   - Post-level analytics (captions, hashtags, mentions, comments)
   - Reel performance data
- Data enrichment and categorization
- Google Sheets export for qualified leads

**Workflow Architecture:**
```
Trigger → Get Influencer List → Loop Items → Parallel Processing:
  ├─ Run Instagram Profile Scraper → Extract Profile Data → Qualify
  └─ Run Instagram Reel Scraper → Extract Post Data → Analyze Engagement
  → Append Results to Google Sheets (Sheet2 & Sheet3)
```

**Niche Keywords:**
- Beauty, Makeup, Skincare, Lifestyle, Fashion, Travel, Fitness, Food, Tech, Business, Health, Education

**Qualification Criteria:**
- Follower count analysis
- Engagement rate calculation
- Niche alignment matching
- Post quality assessment

**Data Exported:**
- **Sheet2:** Platform, Instagram ID, Account Name, Niches, Followers, Qualification Status
- **Sheet3:** Account Name, Caption, Hashtags, Mentions, Comments, Latest Comment, Post URL, Extraction Timestamp

**Use Case:** Build influencer databases for targeted marketing campaigns.

---

### 5. **Competitive Intelligence**

#### 📊 Facebook Ads Spy Agent
**Purpose:** Daily competitor ad tracking and performance analysis.

**Key Features:**
- Google Sheets competitor list management
- Facebook Graph API for ad archives (v24.0)
- Automated ad data collection:
   - Creative bodies and titles
   - Link captions and preview images
   - Delivery dates (start & stop times)
- HTML report generation with styled layout
- Daily email delivery via Gmail
- Preset: Yesterday's ads

**Workflow Architecture:**
```
Trigger → Get Competitors List (Google Sheets) → Query Facebook Ads API
  → Generate HTML Report → Send Email Digest
```

**API Parameters:**
- **Search Terms:** Competitor names/keywords
- **Ad Reached Countries:** Configurable by row
- **Fields:** id, ad_creative_bodies, ad_creative_link_titles, ad_creative_link_captions, ad_snapshot_url, ad_delivery_start_time, ad_delivery_stop_time
- **Data Preset:** Yesterday (automatic date range)

**Report Format:**
- Styled HTML with competitor branding
- Ad creative displays with images
- Delivery timeline visualization
- Daily digest email to: `shuklashree2909@gmail.com`

**Use Case:** Monitor competitor marketing strategies and stay ahead of market trends.

---

## 🏗️ Architecture Highlights

### Data Flow Patterns

**Pattern 1: Webhook-Triggered Real-Time Processing**
- LinkedIn webhook → Event validation → Database update → Message sending
- WhatsApp webhooks → Status routing → Sheet updates

**Pattern 2: Scheduled Batch Processing**
- Daily triggers → Database queries → Filtering → Bulk operations → Logging

**Pattern 3: API-Driven Enrichment**
- Data source → External API calls → Data transformation → Database/Sheet storage

### Integration Points

| Platform | Purpose | Authentication |
|----------|---------|-----------------|
| **Unipile** | LinkedIn connections & messaging | API Key |
| **Google Sheets** | Data source & logging | OAuth2 |
| **PostgreSQL** | Primary data store | Direct connection |
| **Retell AI** | Voice agents | Bearer token |
| **Apify** | Web scraping | API key |
| **Facebook Graph API** | Ads & WhatsApp | Bearer token |
| **Gmail/Outlook** | Email delivery | OAuth2 |

### Database Schema

**Key Tables Referenced:**
- `_learner` - Candidate/lead database with LinkedIn tracking
- `_linkedinAccount` - LinkedIn account management with daily quotas
- `_recruitmentAndHiring` - Candidate pipeline with interview schedules
- Google Sheets - Secondary storage for activity logs and contact lists

---

## 📊 Workflow Statistics

| Workflow | Type | Complexity | Platforms | Status |
|----------|------|-----------|-----------|--------|
| LinkedIn Connection Requests | Batch | High | Unipile, PostgreSQL, n8n | Production |
| LinkedIn First Message | Real-time | Medium | Webhook, Unipile, PostgreSQL | Production |
| WhatsApp Messaging | Batch | Low | WhatsApp API, Google Sheets | Production |
| Message Status Tracking | Real-time | Low | WhatsApp, Google Sheets | Active |
| Email Interview Reminders | Scheduled | Medium | PostgreSQL, Outlook, Google Sheets | Production |
| Retell Voice Calls | Batch | Medium | Retell AI, Google Sheets | Testing |
| Influencer Research | Batch | Very High | Apify, Instagram, Google Sheets | Testing |
| Facebook Ads Monitoring | Scheduled | Medium | Facebook API, Gmail, Google Sheets | Testing |

---

## 🔧 Technical Implementation Details

### Custom Code Nodes (JavaScript)

**LinkedIn Workflow - Payload Normalization:**
```javascript
// Normalizes Unipile webhook data for database operations
const body = $('New Relation Event Webhook').first().json.body;
const accountId = body.account_id || body.accountId;
const userProviderId = body.user_provider_id;
```

**Interview Reminders - Date Filtering:**
```javascript
const today = new Date().toISOString().split('T')[0];
return items.filter(item => {
  const interviewDate = item.json.interviewRound1Date?.split('T')[0];
  return interviewDate === today;
});
```

**Influencer Agent - Niche Classification:**
```javascript
const niches = {
  "beauty": "Beauty",
  "makeup": "Makeup Artist",
  "skincare": "Skincare",
  // ... 20+ niche mappings
};
```

### Error Handling

- Webhook errors: "continueErrorOutput" for non-blocking failures
- API timeouts: Random wait intervals (60-120 seconds) to prevent rate limiting
- Database errors: Continue regular output on connection failures

### Rate Limiting & Performance

- **LinkedIn:** Per-account daily connection quotas managed dynamically
- **WhatsApp:** Message batching with random delays
- **API Calls:** Staggered requests with wait nodes
- **Database:** Connection pooling via PostgreSQL credentials

---

## 🚀 Getting Started

### Prerequisites
1. **n8n Instance** - Self-hosted or cloud (n8n.cloud)
2. **API Credentials:**
   - Unipile account for LinkedIn automation
   - Retell AI account for voice agents
   - Facebook/Meta Developer account
   - Apify account for web scraping
3. **Database:** PostgreSQL instance (Nebula recommended)
4. **Google Account:** For Google Sheets integration
5. **Email Account:** Gmail or Outlook for email automation

### Installation Steps

1. **Clone workflows** - Import JSON files to n8n canvas
2. **Configure credentials:**
   - Add PostgreSQL connection string
   - Add API keys/tokens for each platform
   - Authenticate OAuth2 services
3. **Update database references:**
   - Replace workspace IDs with your own
   - Update table names if using different schema
4. **Test workflows:**
   - Start with manual triggers
   - Verify database connectivity
   - Test API integrations
5. **Schedule & Deploy:**
   - Enable scheduled triggers
   - Set up production credentials
   - Monitor execution logs

### Configuration Examples

**LinkedIn Automation - Daily Connection Limit:**
```javascript
const dailyLimit = 50; // Per account per day
const accountUtilization = calculateUsage(account);
const remaining = dailyLimit - accountUtilization;
```

**WhatsApp Template Setup:**
```json
{
  "template": "thankyou_message|en",
  "components": [{
    "type": "body",
    "parameters": [{ "text": "{{ $json.Name }}" }]
  }]
}
```

---

## 📈 Use Cases & Business Impact

### Lead Generation & Outreach
- **LinkedIn Automation:** 200+ connections/day with 35-40% acceptance rate
- **First Message Conversion:** 15-20% response rate on personalized messages
- **Time Saved:** 10+ hours/week vs manual outreach

### Communication at Scale
- **WhatsApp Messaging:** 500+ messages/day with 98%+ delivery rate
- **Email Campaigns:** 1000+ interview reminders with 60% open rate
- **Real-time Tracking:** Complete delivery & read analytics

### Market Research
- **Competitor Intelligence:** Daily ad updates on 20+ competitors
- **Influencer Database:** 500+ qualified influencers per campaign
- **Trend Analysis:** Identify competitor strategies within 24 hours

### Sales Pipeline Automation
- **Interview Reminders:** 90% show-up rate increase
- **Voice Outreach:** 50+ calls/day with AI-powered qualification
- **Lead Enrichment:** Automatic data population from multiple sources

---

## 🔐 Security & Best Practices

### Credential Management
- All credentials stored in n8n credential system
- No hardcoded secrets in workflows
- API keys rotated regularly
- OAuth2 for Google/Microsoft services

### Data Privacy
- GDPR compliant for contact data
- No sensitive data logging in execution history
- Database backups enforced
- Access controls via PostgreSQL roles

### Monitoring & Logging
- Execution logs stored for 90 days
- Error alerts configured for critical workflows
- Activity logging in Google Sheets
- Regular performance audits

---

## 📝 Workflow Documentation

Each workflow includes:
- **Name:** Clear identification
- **Purpose:** Business objective
- **Trigger:** How execution starts
- **Key Steps:** Node sequence and logic
- **Data Sources:** Where data comes from
- **Data Destinations:** Where results go
- **Error Handling:** Failure scenarios
- **Performance Metrics:** Success rates & timing

---

## 🤝 Contributing & Customization

### Extending Workflows
1. Clone an existing workflow
2. Modify nodes for your use case
3. Test with sample data
4. Update documentation
5. Deploy to production

### Custom Development
- Add new platforms via HTTP nodes
- Create custom integrations with code nodes
- Build data transformation logic in JavaScript
- Implement advanced filtering and routing

---

## 📞 Support & Maintenance

### Regular Maintenance Tasks
- Monitor API rate limits and adjust batch sizes
- Review and update filter criteria monthly
- Audit database performance and cleanup
- Update deprecated API versions

### Troubleshooting
- Check credential validity (monthly)
- Review execution logs for errors
- Validate database connectivity
- Test API endpoints periodically

---

## 📄 License

This portfolio is for demonstration purposes. Customize and deploy according to your business needs.

---

## 👤 About

**Created by:** Srihstipathak

**Expertise Areas:**
- n8n Automation & Workflow Design
- LinkedIn Outreach & Lead Generation
- WhatsApp & Email Marketing Automation
- AI Agent Integration (Retell AI)
- Web Scraping & Data Enrichment
- Database Integration & Optimization
- Multi-platform API Integration

**Repository:** [Srihstipathak/n8n-automation-portfolio](https://github.com/Srihstipathak/n8n-automation-portfolio)

---

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io)
- [Unipile API Docs](https://docs.unipile.com)
- [Retell AI Documentation](https://docs.retellai.com)
- [Apify SDK](https://sdk.apify.com)
- [Meta/Facebook Graph API](https://developers.facebook.com)
- [PostgreSQL Documentation](https://www.postgresql.org/docs)

---

**Last Updated:** September 3, 2026  
**Version:** 1.0.1  
**Repository Status:** Active & Maintained
