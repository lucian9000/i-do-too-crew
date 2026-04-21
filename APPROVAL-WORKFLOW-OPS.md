# I do Too Crew - Approval Workflow Operations Guide

## Purpose

This document defines how content moves from draft to approval to publishing. The goal is to keep the system understandable, low-risk, and easy to operate without publishing mistakes.

## Core Principle

Nothing gets posted automatically unless it has passed explicit approval.

## Approval Stages

### Stage 1: Draft
Meaning:
- content exists but is not ready for review

Typical state:
- caption draft generated
- asset still missing or incomplete
- script not checked yet

### Stage 2: Awaiting Review
Meaning:
- content package is complete enough for human review

Required before entering this stage:
- caption draft exists
- creative brief exists
- preview asset or link exists
- intended platform is defined
- CTA is present

### Stage 3: Needs Revision
Meaning:
- reviewer has requested changes

Examples:
- caption tone wrong
- visual too harsh for weddings
- CTA unclear
- watermark placement bad
- asset does not match platform

### Stage 4: Approved
Meaning:
- content is cleared for scheduling and publishing

### Stage 5: Rejected
Meaning:
- content should not proceed

Examples:
- bad brand fit
- wrong audience
- duplicated concept
- poor quality

## Required Review Fields in Airtable

The `Content Production` table should include at minimum:
- `Approval Status`
- `Reviewer Feedback`
- `Scheduled Date`
- `Publish Status`
- `Preview URL`
- `Platform`
- `Event Type`
- `CTA`

## Human Reviewer Responsibilities

The reviewer should check:
1. Does the content match the event type?
2. Does the tone fit weddings, matric dances, or both?
3. Is the CTA clear?
4. Does the watermark help rather than distract?
5. Is the asset strong enough for the chosen platform?
6. Is the caption readable and not generic?
7. Is this safe to publish publicly?

## Review Checklist by Content Type

### Static Posts
- text is readable
- logo is not overpowering
- headline is strong
- CTA is visible
- visual quality is acceptable

### Carousels
- slide 1 has a strong hook
- structure makes sense slide-to-slide
- last slide has a CTA
- visual consistency holds across slides

### Reels / Short Videos
- first 2 seconds hook attention
- subtitles or text overlays are readable
- clip pacing is strong
- end frame includes CTA or brand lockup if needed
- watermark does not hurt watchability

## Approval Outcomes

### Approved
Action:
- set `Approval Status = Approved`
- ensure schedule exists
- publishing system can pick it up

### Needs Revision
Action:
- set `Approval Status = Needs Revision`
- write specific notes
- avoid vague notes like `fix it`

Good feedback examples:
- Make this feel softer and more romantic for wedding audiences.
- Change CTA from generic inquiry to book a consultation.
- Reduce watermark opacity and move it away from the subject.

### Rejected
Action:
- set `Approval Status = Rejected`
- include reason
- do not re-enter publish queue unless intentionally reworked

## Operational Rules

1. One final reviewer should own the final sign-off.
2. Feedback should be specific and actionable.
3. High-risk or paid content should get stricter review.
4. New template types should be reviewed more carefully than familiar ones.
5. Publishing failures should never silently disappear; they must alert the operator.

## Suggested Airtable Views

### Review Queue
Filter:
- `Approval Status = Awaiting Review`

### Revisions Needed
Filter:
- `Approval Status = Needs Revision`

### Approved and Ready to Schedule
Filter:
- `Approval Status = Approved`
- `Publish Status != Posted`

### Failed Publishing
Filter:
- `Publish Status = Failed`

## Approval Service-Level Targets

Suggested turnaround times:
- organic post review: same day
- urgent promo post: within 2 hours
- scheduled campaign content: within 24 hours

## Recommended Low-Risk Start

For the first version of the system:
- all posts require manual review
- all publishing is scheduled only after approval
- TikTok content gets extra review for hook strength
- wedding content gets extra review for tone and elegance

## Future Optimization

Once confidence is high:
- low-risk templates can use faster approval
- only high-visibility posts need deeper review
- repeated formats can move faster if performance and quality stay high
