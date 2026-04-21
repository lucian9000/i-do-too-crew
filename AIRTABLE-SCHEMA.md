# I Do Crew - Airtable Schema

## Purpose

Airtable acts as the operational control tower for marketing, approvals, lead capture, client intake, and publishing.

## Base Structure

Recommended base name:
- `I Do Crew OS`

## Table 1: Brands
Used to store core brand settings.

### Fields
- `Brand Name` (single line text)
- `Business Description` (long text)
- `Primary Services` (multiple select)
  - Weddings
  - Matric Dances
  - Event Styling
  - Coordination
- `Target Market` (long text)
- `Tone of Voice` (single select)
  - Elegant
  - Playful
  - Romantic
  - Premium
  - Youthful
- `Brand Colours` (long text)
- `Fonts` (long text)
- `Logo` (attachment)
- `Website URL` (url)
- `WhatsApp Number` (phone)
- `Instagram Handle` (single line text)
- `Facebook Page` (single line text)
- `TikTok Handle` (single line text)
- `Primary CTA` (single line text)
- `Booking Link` (url)
- `Notes` (long text)

## Table 2: Campaigns
Used for seasonal, paid, and organic campaign planning.

### Fields
- `Campaign Name` (single line text)
- `Brand` (link to Brands)
- `Audience Type` (multiple select)
  - Brides
  - Couples
  - Parents
  - Students
  - Schools
- `Service Focus` (multiple select)
  - Weddings
  - Matric Dances
- `Goal` (single select)
  - Awareness
  - Leads
  - Bookings
  - Engagement
- `Offer` (long text)
- `Start Date` (date)
- `End Date` (date)
- `Status` (single select)
  - Planned
  - Active
  - Paused
  - Complete
- `Budget` (currency)
- `Landing Page URL` (url)
- `Notes` (long text)

## Table 3: Content Ideas
Stores raw and approved content concepts.

### Fields
- `Idea Title` (single line text)
- `Campaign` (link to Campaigns)
- `Brand` (link to Brands)
- `Platform` (multiple select)
  - Instagram
  - Facebook
  - TikTok
  - WhatsApp Status
- `Content Pillar` (single select)
  - Inspiration
  - Transformation
  - Education
  - Trust
  - Emotion
  - Conversion
- `Event Type` (single select)
  - Wedding
  - Matric Dance
  - Both
- `Audience` (single line text)
- `Hook` (long text)
- `Core Message` (long text)
- `CTA` (single line text)
- `Priority` (single select)
  - Low
  - Medium
  - High
- `Status` (single select)
  - Backlog
  - Approved for Production
  - Rejected
- `Notes` (long text)

## Table 4: Content Production
Stores actual generated content and asset metadata.

### Fields
- `Production Name` (formula or single line text)
- `Content Idea` (link to Content Ideas)
- `Brand` (link to Brands)
- `Campaign` (link to Campaigns)
- `Post Type` (single select)
  - Static Image
  - Carousel
  - Reel
  - Story
  - Short Video
- `Caption Draft` (long text)
- `Caption Final` (long text)
- `Hashtags` (long text)
- `Creative Brief` (long text)
- `Image Prompt` (long text)
- `Video Prompt` (long text)
- `Script` (long text)
- `Thumbnail` (attachment)
- `Asset Files` (attachment)
- `Preview URL` (url)
- `Approval Status` (single select)
  - Draft
  - Awaiting Review
  - Needs Revision
  - Approved
  - Rejected
- `Reviewer Feedback` (long text)
- `Scheduled Date` (date time)
- `Publish Status` (single select)
  - Not Scheduled
  - Scheduled
  - Posted
  - Failed
- `Posted URL` (url)
- `Publishing Notes` (long text)

## Table 5: Approvals
Optional dedicated review log.

### Fields
- `Approval Name` (single line text)
- `Content Production` (link to Content Production)
- `Reviewer Name` (single line text)
- `Decision` (single select)
  - Approved
  - Revision Requested
  - Rejected
- `Feedback` (long text)
- `Reviewed At` (date time)

## Table 6: Leads
Core lead capture table from WhatsApp, website, and ads.

### Fields
- `Lead ID` (autonumber or formula)
- `Full Name` (single line text)
- `Phone Number` (phone)
- `Email` (email)
- `Lead Source` (single select)
  - WhatsApp Bot
  - Website Form
  - Instagram
  - Facebook
  - TikTok
  - Referral
- `Event Type` (single select)
  - Wedding
  - Matric Dance
- `Event Date` (date)
- `Location` (single line text)
- `Guest Count` (number)
- `Budget Range` (single select)
  - Under R10k
  - R10k to R25k
  - R25k to R50k
  - R50k+
- `School or Venue` (single line text)
- `Services Needed` (multiple select)
  - Decor
  - Planning
  - Coordination
  - Styling
  - Photography support
  - Full package
- `Lead Status` (single select)
  - New
  - Qualified
  - Awaiting Contact
  - Contacted
  - Quote Sent
  - Won
  - Lost
- `Assigned To` (single line text)
- `Notes` (long text)
- `Last Contacted` (date time)

## Table 7: Client Intake Log
Detailed intake transcript and structured responses.

### Fields
- `Intake ID` (autonumber)
- `Lead` (link to Leads)
- `Channel` (single select)
  - WhatsApp
  - Website
  - Manual
- `Conversation Summary` (long text)
- `Raw Intake Transcript` (long text)
- `Urgency` (single select)
  - Low
  - Medium
  - High
- `Preferred Contact Method` (single select)
  - WhatsApp
  - Call
  - Email
- `Next Action` (single line text)
- `Next Action Date` (date time)

## Table 8: Publishing Calendar
Optional scheduling table if you want cleaner publishing separation.

### Fields
- `Calendar Item` (single line text)
- `Content Production` (link to Content Production)
- `Platform` (single select)
  - Instagram
  - Facebook
  - TikTok
- `Scheduled Date` (date time)
- `Status` (single select)
  - Scheduled
  - Posted
  - Failed
- `Response Log` (long text)
- `Post URL` (url)

## Key Views to Create

### Leads
- New Leads
- Awaiting Contact
- Quote Sent
- Won Leads
- Wedding Leads
- Matric Leads

### Content Production
- Awaiting Review
- Approved to Schedule
- Scheduled This Week
- Failed Posts

### Campaigns
- Active Campaigns
- Upcoming Campaigns

## Automation Notes
- New WhatsApp intake -> create lead in `Leads`
- Full WhatsApp intake transcript -> save to `Client Intake Log`
- Approved idea -> create row in `Content Production`
- Approved content + schedule -> create row in `Publishing Calendar`
