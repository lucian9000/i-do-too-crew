# I do Too Crew - n8n Workflow Map

## Purpose

n8n is the orchestration layer that connects Airtable, AI generation, approvals, publishing, and lead capture.

## Workflow 1: Content Idea Generator

### Trigger
- Manual trigger
- Scheduled weekly trigger
- New active campaign in Airtable

### Steps
1. Trigger starts workflow
2. Airtable node loads active campaign data
3. Airtable node loads brand profile
4. AI node generates content ideas based on:
   - event type
   - audience
   - campaign goal
   - platform
5. Formatter node structures ideas
6. Airtable node writes ideas to `Content Ideas`

### Outputs
- Batch of content ideas ready for review

## Workflow 2: Content Production Generator

### Trigger
- Airtable record changes to `Approved for Production`

### Steps
1. Airtable trigger on `Content Ideas`
2. Pull linked brand and campaign data
3. AI node generates:
   - caption draft
   - hook
   - CTA
   - hashtags
   - image prompt
   - video prompt
   - script
4. Image generation step creates visual asset
5. Video generation step creates 9:16 short video concept or asset
6. Storage step saves files to chosen storage layer
7. Airtable update creates or updates `Content Production`
8. Mark `Approval Status = Awaiting Review`

### Outputs
- Review-ready content asset package

## Workflow 3: Approval Router

### Trigger
- New `Content Production` record with `Approval Status = Awaiting Review`

### Steps
1. Detect review-ready asset
2. Send notification to reviewer
   - email
   - WhatsApp
   - Telegram
   - internal preferred channel
3. Include preview URL and production record link
4. Reviewer updates Airtable manually
5. Conditional logic:
   - If approved -> continue to scheduling workflow
   - If revision requested -> send feedback into regeneration workflow
   - If rejected -> close record

### Outputs
- Approved or revised content

## Workflow 4: Revision Workflow

### Trigger
- `Content Production` changed to `Needs Revision`

### Steps
1. Pull prior draft and reviewer feedback
2. AI rewrite node updates caption, script, prompt, or CTA
3. Regenerate requested asset if needed
4. Update Airtable record
5. Return asset to review state

### Outputs
- Updated content package for approval

## Workflow 5: Scheduled Publishing

### Trigger
- Scheduled polling every 15 minutes

### Steps
1. Search `Content Production` or `Publishing Calendar` for approved records where scheduled time is due
2. Split by platform
3. Platform publishing nodes:
   - Instagram publish path
   - Facebook publish path
   - TikTok publish path (depending on API path)
4. Capture API response
5. Update Airtable:
   - Publish Status
   - Posted URL
   - Error notes if failed
6. Notify if failure occurs

### Outputs
- Posts published automatically after approval

## Workflow 6: WhatsApp Lead Capture

### Trigger
- Incoming WhatsApp message from business bot channel

### Steps
1. Receive inbound message webhook
2. Check if sender already exists in Airtable `Leads`
3. If not, create lead shell record
4. Start guided intake sequence
5. Collect:
   - name
   - event type
   - date
   - location
   - guest count
   - budget
   - services needed
   - preferred contact method
6. Update Airtable `Leads`
7. Save conversation summary to `Client Intake Log`
8. Notify human if lead is high intent

### Outputs
- Qualified lead record in Airtable

## Workflow 7: Website Lead Capture

### Trigger
- Form submission from website

### Steps
1. Receive form webhook
2. Validate required fields
3. Create or update lead in Airtable
4. Create `Client Intake Log` summary
5. Send autoresponder
6. Notify team or assign follow-up

### Outputs
- Website leads merged into same funnel as WhatsApp leads

## Workflow 8: Lead Follow-Up Automation

### Trigger
- New lead created
- Lead status changed
- Follow-up date reached

### Steps
1. Check lead status
2. If new and uncontacted, send internal alert
3. If quote sent and no reply after threshold, create reminder task
4. If lead is won, move to client pipeline
5. If lead is lost, tag reason if available

### Outputs
- Reduced lead leakage

## Workflow 9: Weekly Content Planner

### Trigger
- Weekly scheduled trigger

### Steps
1. Pull active campaigns
2. Pull content gaps by pillar and platform
3. Ask AI for next week’s recommended content mix
4. Write content recommendations into `Content Ideas`
5. Notify operator with weekly content queue summary

### Outputs
- Weekly structured planning cadence

## Workflow 10: Reporting Workflow

### Trigger
- Weekly scheduled trigger
- Monthly scheduled trigger

### Steps
1. Pull leads created, quotes sent, booked events
2. Pull published posts and platform outputs
3. Summarize pipeline and content performance
4. Save report summary to Airtable or send via preferred channel

### Outputs
- Weekly and monthly operating visibility

## Dependencies
- Airtable API key and base ID
- LLM provider credentials
- Image generation provider
- Video generation provider
- Meta credentials for Instagram/Facebook
- TikTok API or alternate posting process
- WhatsApp business integration
- File storage destination

## Recommended Build Order
1. WhatsApp Lead Capture
2. Airtable schema
3. Content Idea Generator
4. Content Production Generator
5. Approval Router
6. Scheduled Publishing
7. Website Lead Capture
8. Reporting
