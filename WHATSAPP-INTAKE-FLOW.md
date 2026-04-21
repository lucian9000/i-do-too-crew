# I do Too Crew - WhatsApp Intake Flow

## Purpose

The WhatsApp bot is the first contact and lead qualification layer for inbound prospects. Its job is to capture structured event details and save them to Airtable.

## Goals
- Capture leads fast
- Reduce manual back-and-forth
- Collect enough info for quote follow-up
- Keep tone friendly and premium
- Route high-intent leads to a human quickly

## Entry Points
- Click to WhatsApp from Instagram
- Click to WhatsApp from Facebook
- Website WhatsApp CTA
- Paid ads using WhatsApp as destination
- Direct inbound messages

## Core Bot Principles
- Ask one thing at a time
- Keep messages short
- Offer button-style choices where possible
- Avoid long paragraphs
- Hand over to a human when the lead is ready or confused

## Suggested Conversation Flow

### Step 1: Welcome
Message:
- Welcome to I do Too Crew.
- Are you planning a wedding or a matric dance?

Expected response:
- Wedding
- Matric Dance

Captured field:
- `Event Type`

### Step 2: Name
Message:
- Amazing. What’s your name?

Captured field:
- `Full Name`

### Step 3: Date
Message:
- When is the event?

Captured field:
- `Event Date`

### Step 4: Location
Message:
- What area or venue is the event in?

Captured field:
- `Location`
- optional `School or Venue`

### Step 5: Guest Count
Message:
- Roughly how many guests are you expecting?

Captured field:
- `Guest Count`

### Step 6: Budget Range
Message:
- What budget range are you working with?

Suggested options:
- Under R10k
- R10k to R25k
- R25k to R50k
- R50k+

Captured field:
- `Budget Range`

### Step 7: Services Needed
Message:
- What do you need help with?

Suggested options:
- Decor
- Planning
- Coordination
- Full event setup
- Not sure yet

Captured field:
- `Services Needed`

### Step 8: Contact Preference
Message:
- What’s the best way for us to follow up, WhatsApp, call, or email?

Captured fields:
- `Preferred Contact Method`
- `Email` if required

### Step 9: Wrap-up
Message:
- Perfect. We’ve got your details.
- A team member will follow up shortly with the next steps.

Bot actions:
- Create or update lead in Airtable
- Write transcript summary to `Client Intake Log`
- Notify operator if lead is qualified

## Qualification Logic

### High intent lead indicators
- Budget above minimum threshold
- Event date within active planning window
- Requests full service or coordination
- Responds clearly and fully

### Actions for high intent leads
- Mark `Lead Status = Qualified`
- Send operator alert
- Create follow-up task

### Actions for lower intent leads
- Mark `Lead Status = New`
- Add to nurture list
- Optionally send brochure or checklist

## Fallback Handling

### If user is confused
Message:
- No problem. I can connect you with a team member.

### If user goes off-script
- Save raw message
- Route to human review if confidence is low

### If user stops replying
- Save partial lead
- Trigger reminder after defined period

## Airtable Mappings
- Full Name -> `Leads.Full Name`
- Phone Number -> `Leads.Phone Number`
- Event Type -> `Leads.Event Type`
- Event Date -> `Leads.Event Date`
- Location -> `Leads.Location`
- Guest Count -> `Leads.Guest Count`
- Budget Range -> `Leads.Budget Range`
- Services Needed -> `Leads.Services Needed`
- Preferred Contact Method -> `Client Intake Log.Preferred Contact Method`
- Conversation Summary -> `Client Intake Log.Conversation Summary`
- Raw Transcript -> `Client Intake Log.Raw Intake Transcript`

## Human Handoff Conditions
- User asks for pricing immediately
- User asks to speak to a person
- User asks a complex question
- Bot confidence is low
- Lead is high value or urgent

## Suggested Follow-Up Message Templates

### Qualified lead
- Thanks, we’ve got everything we need. We’ll message you shortly with the next steps.

### Partial lead
- We’ve saved your details so far. If you’d like, reply anytime and we’ll pick up where we left off.

### Human handoff
- I’m handing this over to the team so they can help you properly.

## Recommended Implementation Notes
- Store conversation state per phone number
- Use Airtable record IDs to maintain continuity
- Keep transcripts for context
- Add tags for `Wedding` and `Matric Dance`
- Trigger operator notifications for urgent or high-value leads
