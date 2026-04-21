# I Do Crew - n8n Build Specs

## Purpose

This document turns the workflow map into implementation-grade n8n build instructions. It is written to make the first live workflows buildable inside the local n8n instance.

## Global Standards

### Naming convention
Use this naming style for workflows:
- `IDTC - WhatsApp Lead Capture`
- `IDTC - Website Lead Capture`
- `IDTC - Content Idea Generator`
- `IDTC - Content Production Generator`
- `IDTC - Approval Router`
- `IDTC - Scheduled Publishing`
- `IDTC - Weekly Planner`
- `IDTC - Reporting`

### Shared configuration variables
These should be centralized in n8n credentials, environment values, or config nodes.

- Airtable Base ID
- Airtable API Token
- LLM API Key
- Image Generation API Key
- Video Generation API Key
- Meta API credentials
- TikTok publishing credentials or alternate path
- WhatsApp provider credentials
- Storage bucket or file endpoint
- Internal notification destination

### Shared data conventions
Every workflow should pass these core identifiers whenever possible:
- `brandId`
- `campaignId`
- `contentIdeaId`
- `contentProductionId`
- `leadId`
- `eventType`
- `platform`

## Workflow 1: IDTC - WhatsApp Lead Capture

### Goal
Capture inbound WhatsApp leads, guide them through intake, store their data in Airtable, and notify a human if the lead is qualified.

### Recommended first build priority
This should be the first production workflow built.

### Node-by-node structure

1. **Webhook Trigger**
   - Type: Webhook
   - Purpose: Receive inbound message from WhatsApp provider
   - Output: raw inbound payload

2. **Normalize Payload**
   - Type: Code or Set node
   - Purpose: Extract sender phone number, message body, timestamp, channel id
   - Output fields:
     - `phoneNumber`
     - `messageText`
     - `timestamp`
     - `channel`

3. **Lookup Existing Lead**
   - Type: Airtable Search
   - Table: `Leads`
   - Query: match on `Phone Number`
   - Purpose: determine whether lead already exists

4. **IF Lead Exists**
   - Type: IF node
   - True: existing lead path
   - False: create new lead shell

5. **Create Lead Shell**
   - Type: Airtable Create Record
   - Table: `Leads`
   - Fields:
     - `Phone Number`
     - `Lead Source = WhatsApp Bot`
     - `Lead Status = New`

6. **Load Conversation State**
   - Type: Data store or Airtable lookup
   - Purpose: determine which intake question should be asked next
   - Suggested storage options:
     - n8n data store
     - Airtable `Client Intake Log`
     - dedicated session table if needed

7. **Determine Next Question**
   - Type: Switch node
   - Cases:
     - no event type yet
     - no full name yet
     - no event date yet
     - no location yet
     - no guest count yet
     - no budget range yet
     - no services needed yet
     - no preferred contact method yet
     - complete

8. **Update Airtable Based on Response**
   - Type: Airtable Update Record
   - Table: `Leads`
   - Purpose: write captured answer into correct field

9. **Write Intake Log**
   - Type: Airtable Create or Update
   - Table: `Client Intake Log`
   - Purpose:
     - append summary
     - store raw transcript or latest exchange

10. **Compose Reply Message**
   - Type: Set or Code node
   - Purpose: generate next question or handoff message

11. **Send WhatsApp Reply**
   - Type: HTTP Request or provider node
   - Purpose: send next message to user

12. **Qualification Check**
   - Type: IF node
   - Conditions:
     - budget meets minimum threshold
     - event type present
     - event date present
     - services indicate real intent

13. **Notify Human Operator**
   - Type: Telegram, email, or preferred internal channel
   - Purpose: alert that a qualified lead is ready

14. **Respond to Webhook**
   - Type: Respond to Webhook
   - Purpose: close request cleanly

### Notes
- Store one question at a time.
- Avoid long bot messages.
- Add human handoff trigger on keywords like `person`, `call`, `price`, `quote`.

## Workflow 2: IDTC - Website Lead Capture

### Goal
Capture form submissions from the site and merge them into the same Airtable lead funnel.

### Node-by-node structure

1. Webhook Trigger
2. Validate required fields
3. Normalize values
4. Search Airtable `Leads` by email or phone
5. IF existing lead -> update
6. ELSE create new lead
7. Create `Client Intake Log` summary
8. Send autoresponder email or WhatsApp
9. Notify internal team
10. Respond to webhook

### Required fields
- name
- phone or email
- event type
- date
- location
- message or service interest

## Workflow 3: IDTC - Content Idea Generator

### Goal
Generate structured social content ideas from active campaigns and brand context.

### Node-by-node structure

1. Manual Trigger or Schedule Trigger
2. Airtable Search `Campaigns`
   - filter active campaigns
3. Airtable Fetch linked `Brands`
4. Merge node
   - combine campaign and brand data
5. AI Prompt Builder
   - build structured prompt using campaign goal, service focus, audience, and platform list
6. AI Request Node
   - returns content ideas
7. Code Node
   - parse structured AI output into records
8. Split In Batches
   - one idea at a time
9. Airtable Create Record
   - table: `Content Ideas`
10. Internal Summary Notification

### Expected AI output per idea
- title
- platform
- event type
- pillar
- hook
- CTA
- core message

## Workflow 4: IDTC - Content Production Generator

### Goal
Turn an approved idea into actual production-ready content.

### Trigger
Airtable trigger on `Content Ideas` when `Status = Approved for Production`

### Node-by-node structure

1. Airtable Trigger
2. Airtable Fetch linked `Brand` and `Campaign`
3. Merge Data node
4. AI Prompt Builder for copy package
5. AI Request Node for caption/script package
6. Parse AI output
7. IF static or carousel needed -> image prompt branch
8. IF reel or short video needed -> video prompt branch
9. Image Generation HTTP/API node
10. Video Generation HTTP/API node
11. Upload assets to storage
12. Airtable Create Record in `Content Production`
13. Set `Approval Status = Awaiting Review`
14. Notify reviewer

### Outputs to write
- caption draft
- hashtags
- creative brief
- script
- prompts
- preview URL
- asset links

## Workflow 5: IDTC - Approval Router

### Goal
Route review-ready content to the human approver.

### Node-by-node structure

1. Airtable Trigger on `Content Production`
2. IF `Approval Status = Awaiting Review`
3. Build review summary
4. Send notification to reviewer
   - include title
   - event type
   - platform
   - preview URL
   - Airtable link
5. End

### Reviewer action
Reviewer updates Airtable manually:
- Approved
- Needs Revision
- Rejected

## Workflow 6: IDTC - Revision Workflow

### Goal
Regenerate content after feedback.

### Trigger
Airtable trigger when `Approval Status = Needs Revision`

### Node-by-node structure

1. Airtable Trigger
2. Fetch existing production record
3. Pull reviewer feedback
4. Build revision prompt
5. AI Request for updated caption/script/prompt
6. Conditional branch for asset regeneration if needed
7. Update storage assets if regenerated
8. Update `Content Production`
9. Set status back to `Awaiting Review`
10. Notify reviewer

## Workflow 7: IDTC - Scheduled Publishing

### Goal
Post approved content automatically when its scheduled time arrives.

### Trigger
Schedule trigger every 15 minutes

### Node-by-node structure

1. Schedule Trigger
2. Airtable Search due records in `Content Production` or `Publishing Calendar`
3. IF no records -> end
4. Split in Batches
5. Switch by platform
   - Instagram
   - Facebook
   - TikTok
6. Platform-specific publish node or HTTP request
7. Capture response
8. IF success -> update Airtable `Posted`
9. IF failure -> update Airtable `Failed`
10. Notify internal team on failures

### Publishing requirement
Only records with:
- `Approval Status = Approved`
- valid schedule time
- valid asset URL

## Workflow 8: IDTC - Weekly Planner

### Goal
Create a weekly planning rhythm for content generation.

### Node-by-node structure

1. Schedule Trigger weekly
2. Fetch active campaigns
3. Fetch recent content production records
4. Identify missing pillars/platform gaps via Code node
5. Build planning prompt for AI
6. AI Request
7. Parse suggestions
8. Write new records into `Content Ideas`
9. Notify operator with weekly summary

## Workflow 9: IDTC - Lead Follow-Up Automation

### Goal
Prevent lead leakage and enforce timely follow-up.

### Trigger options
- Schedule trigger daily
- Airtable trigger on status changes

### Node-by-node structure

1. Trigger
2. Search `Leads` for:
   - new and not contacted
   - quote sent but stale
   - awaiting contact
3. Split by lead status
4. Send reminders or task notifications
5. Update `Last Contacted` or internal note fields if needed

## Workflow 10: IDTC - Reporting

### Goal
Provide weekly and monthly operating visibility.

### Node-by-node structure

1. Schedule Trigger weekly or monthly
2. Fetch leads from Airtable
3. Fetch content production and publishing results
4. Aggregate metrics in Code node
5. Build summary report
6. Write report to Airtable or send via message channel

### Example metrics
- new leads
- qualified leads
- quote sent count
- won leads
- posts published
- failed posts
- top content categories by volume

## First Live Workflow Recommendation

Build in this order:
1. `IDTC - WhatsApp Lead Capture`
2. `IDTC - Website Lead Capture`
3. `IDTC - Content Idea Generator`
4. `IDTC - Content Production Generator`
5. `IDTC - Approval Router`
6. `IDTC - Scheduled Publishing`

## Implementation Notes
- Keep each workflow modular.
- Avoid one giant workflow monster.
- Use Airtable as source of truth.
- Use clear status fields instead of hidden logic.
- Keep human approval mandatory before publishing.
