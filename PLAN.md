# I do to Crew - System Plan

## Goals

- Build an automated social media posting workflow for a South African events startup focused on matric dance planning.
- Use Airtable as the control tower.
- Use local n8n as the orchestration layer.
- Use AI to generate copy, images, and short 9:16 videos.
- Require approval before publishing.
- Capture inbound client leads through WhatsApp and save them into Airtable.
- Stand up a website for lead capture, trust, and conversion.

## Major Components

### 1. Airtable
- Brand data
- Campaigns
- Content ideas
- Production queue
- Approvals
- Publishing calendar
- Leads
- Client inquiries

### 2. n8n
- Content ideation workflow
- Asset generation workflow
- Approval routing workflow
- Scheduled publishing workflow
- WhatsApp intake workflow
- Website lead capture workflow

### 3. AI Layer
- LLM for content strategy, copy, hooks, hashtags, captions, scripts, and rewrites
- Image generation for promotional posts and concept visuals
- Video generation for vertical short-form content

### 4. WhatsApp Bot
- Lead capture
- Quote request flow
- FAQ flow
- Booking inquiry collection
- Airtable sync
- Optional handoff to human operator

### 5. Website
- Home
- Packages
- Gallery
- About
- Contact / quote form
- FAQ
- WhatsApp CTA

## Rollout Phases

### Phase 1
- Discovery
- Airtable schema
- n8n workflow map
- Approval system

### Phase 2
- AI content generation pipeline
- Asset storage strategy
- Instagram and Facebook posting

### Phase 3
- WhatsApp bot intake
- Lead syncing to Airtable
- Website launch

### Phase 4
- TikTok workflow
- Optimization and analytics
- Content calendar automation

## Immediate Next Tasks

1. Confirm brand identity details
2. Define Airtable tables and fields
3. Map n8n workflows node by node
4. Decide WhatsApp bot provider path
5. Create GitHub repo and push planning repo
