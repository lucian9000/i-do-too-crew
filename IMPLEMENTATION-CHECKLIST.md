# I Do Crew - Implementation Checklist

## Purpose

This checklist converts the strategy and architecture into an execution sequence that can be followed step by step.

## Phase 1: Foundations

### Repository and planning
- [x] Create repo
- [x] Write planning docs
- [x] Write Airtable schema
- [x] Write n8n workflow map
- [x] Write WhatsApp intake flow
- [x] Write build specs
- [x] Write website sitemap
- [x] Write content template system
- [x] Write approval workflow guide

### Brand and assets
- [ ] Confirm production logo pack
- [ ] Create transparent logo exports
- [ ] Decide watermark treatment
- [ ] Gather sample event imagery

## Phase 2: Airtable Setup
- [ ] Create Airtable base
- [ ] Create `Brands` table
- [ ] Create `Campaigns` table
- [ ] Create `Content Ideas` table
- [ ] Create `Content Production` table
- [ ] Create `Approvals` table
- [ ] Create `Leads` table
- [ ] Create `Client Intake Log` table
- [ ] Create `Publishing Calendar` table
- [ ] Create required views

## Phase 3: Credentials and Integrations
- [ ] Airtable API access
- [ ] LLM API access
- [ ] Image generation access
- [ ] Video generation access
- [ ] WhatsApp provider chosen and configured
- [ ] Meta business access confirmed
- [ ] TikTok posting path confirmed
- [ ] Storage path confirmed
- [ ] Internal notification channel chosen

## Phase 4: Lead Engine

### WhatsApp bot
- [ ] Set up inbound webhook in n8n
- [ ] Build lead lookup logic
- [ ] Build guided intake flow
- [ ] Save leads to Airtable
- [ ] Save intake logs to Airtable
- [ ] Add human handoff triggers
- [ ] Add qualified lead notifications

### Website lead capture
- [ ] Build quote form fields
- [ ] Connect form webhook to n8n
- [ ] Save site leads to Airtable
- [ ] Send autoresponder
- [ ] Notify operator

### Lead follow-up
- [ ] Create uncontacted lead reminder logic
- [ ] Create stale quote reminder logic
- [ ] Create follow-up task logic

## Phase 5: Content Engine
- [ ] Build content idea generator
- [ ] Build weekly planner
- [ ] Build content production generator
- [ ] Define storage path for assets
- [ ] Ensure generated assets write back to Airtable

## Phase 6: Approval and Publishing
- [ ] Build approval router
- [ ] Build revision workflow
- [ ] Build scheduled publishing workflow
- [ ] Add failure alerting
- [ ] Test approval to publish sequence end-to-end

## Phase 7: Website
- [ ] Acquire domain name
- [ ] Configure DNS access
- [ ] Build home page
- [ ] Build weddings page
- [ ] Build matric dances page
- [ ] Build packages page
- [ ] Build gallery page
- [ ] Build about page
- [ ] Build contact / quote page
- [ ] Build FAQ page
- [ ] Add WhatsApp CTA across site
- [ ] Connect form submissions to n8n

## Phase 8: Operations
- [ ] Confirm reviewer role
- [ ] Confirm lead response owner
- [ ] Confirm content approval SLA
- [ ] Confirm publishing schedule cadence
- [ ] Confirm reporting cadence

## Phase 9: Testing
- [ ] Test WhatsApp intake flow end-to-end
- [ ] Test website lead capture end-to-end
- [ ] Test Airtable writes and updates
- [ ] Test approval workflow
- [ ] Test Instagram posting path
- [ ] Test Facebook posting path
- [ ] Test TikTok workflow path
- [ ] Test failure handling

## Phase 10: Launch
- [ ] Launch lead engine
- [ ] Launch website
- [ ] Launch content planning workflow
- [ ] Launch production workflow
- [ ] Launch approval workflow
- [ ] Launch scheduled publishing
- [ ] Start weekly reporting

## Recommended Immediate Next Steps
1. Create the Airtable base
2. Build the WhatsApp lead capture workflow first
3. Build the website lead capture webhook second
4. Stand up the quote form on the website
5. Only then move into content automation and publishing
