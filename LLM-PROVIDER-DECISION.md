# I do Too Crew - LLM Provider Decision

## Purpose

This document defines the most cost-effective LLM strategy for I do Too Crew, including the cheapest stack, a recommended hybrid stack, the Docker path for local Ollama deployment, task routing between local and paid models, and the exact recommendation for this project.

## Decision Summary

### Cheapest overall stack
- Local self-hosted LLM via Ollama in Docker
- No recurring API spend for baseline text generation and routing tasks

### Cheapest stack with better output quality
- Ollama for low-value/high-volume tasks
- Google Gemini Flash for higher-quality final outputs

### Recommended final decision for this project
**Use a hybrid stack**:
- Local Ollama model as the default internal engine
- Gemini Flash as the paid quality layer

This gives the strongest balance of:
- low cost
- no dependency on a premium API for everything
- enough quality for marketing copy and campaign outputs
- operational flexibility inside n8n

## Option 1: Cheapest Stack

### Architecture
- n8n
- Airtable
- Ollama in Docker
- local file storage
- WhatsApp bot and content workflows point to local model endpoint

### Benefits
- lowest recurring LLM cost
- no external LLM API key required after setup
- private and local
- simple to call over HTTP from n8n

### Risks
- output quality may be weaker for polished sales copy
- wedding copy may feel less refined without editing
- hook quality for short-form social may need human tuning
- model performance depends on available hardware

### Best use cases
- lead intake classification
- extracting structured fields from WhatsApp
- tagging and routing leads
- internal summaries
- first-draft captions
- content idea clustering
- FAQ answers

## Option 2: Hybrid Stack

### Architecture
- Ollama in Docker as local default model server
- Gemini Flash for premium outputs where quality matters
- n8n routes tasks to either local or paid model based on workflow stage

### Benefits
- keeps cost low
- reserves paid usage for only important content
- allows better quality where brand perception matters
- provides resilience if external API usage needs to be reduced

### Risks
- slightly more complex routing logic
- requires one paid provider account for quality tasks

### Best use cases
#### Local model
- WhatsApp intake parsing
- classification
- internal summaries
- data cleaning
- idea grouping
- low-stakes draft generation

#### Paid model
- final captions
- final ad copy
- refined hooks
- content rewrites after review
- premium campaign ideas
- website copy drafts

## Why not use a premium provider for everything?

Using premium external models for every workflow step is simple but inefficient.

For this project, many tasks are repetitive and operational rather than creative. Those should not consume expensive API calls if a local model can do them well enough.

## Docker Path for Ollama

## Recommended deployment
Deploy Ollama as a dedicated Docker container on the Linux server and expose it to the internal network or to localhost for n8n.

### Example Docker command
```bash
docker run -d \
  --name ollama \
  -p 11434:11434 \
  -v ollama:/root/.ollama \
  ollama/ollama
```

### Example model pull
After the container is running:
```bash
docker exec -it ollama ollama pull llama3.1
```

### Example local endpoint
n8n can call:
```text
http://localhost:11434/api/generate
```

If n8n runs in Docker, use the correct Docker network host reference instead of raw localhost.

## Recommended Ollama model path
Start with a lightweight general model first, then test heavier options if the server can handle them.

Suggested evaluation order:
1. Llama 3.1 class model
2. Qwen class model if available and strong enough
3. Any stronger local model only if hardware supports it reliably

## Task Routing Decision

## Tasks that should go to the local model
These are cheap, repetitive, and operational.

- WhatsApp message parsing
- extracting name, budget, date, guest count
- lead classification
- assigning event type
- internal summaries
- structured JSON output for Airtable fields
- first-pass content ideas
- topic clustering
- basic FAQ responses
- simple rewrite or shortening tasks

## Tasks that should go to the paid model
These affect public-facing quality and conversion more directly.

- final social captions
- ad copy
- premium wedding messaging
- strong hooks for reels and TikTok
- call-to-action refinement
- client-facing website copy drafts
- revision passes after review feedback
- higher-quality campaign planning output

## Suggested n8n Routing Rule

### Local model route
Use local model if task is:
- internal only
- structured extraction
- classification
- low-risk draft work

### Paid model route
Use paid model if task is:
- customer-facing
- public-facing
- conversion-sensitive
- brand-sensitive
- approval-sensitive

## Operational Recommendation for I do Too Crew

### Exact recommendation
For this project, implement:

1. **Ollama in Docker** as the local LLM endpoint
2. **Gemini Flash** as the paid quality model
3. **n8n routing logic** to separate low-value and high-value tasks

### Why this is the right fit
I do Too Crew needs:
- cheap operational automation
- decent writing quality
- reliable lead handling
- good-enough creative iteration
- stronger polish only where the audience actually sees it

That makes a hybrid architecture the most cost-effective real-world solution.

## Recommended Build Sequence

### Phase 1
- Deploy Ollama in Docker
- test local model endpoint
- wire n8n to local model
- use it for WhatsApp lead parsing and internal summaries

### Phase 2
- add Gemini Flash credential to n8n
- route final captions and public copy through Gemini Flash

### Phase 3
- test quality thresholds
- decide whether some tasks can move fully local later
- optimize cost by reducing paid model usage where unnecessary

## Success Criteria

The LLM layer is working well when:
- WhatsApp leads are captured cleanly into Airtable
- content drafts are generated without large manual cleanup
- final captions are good enough to approve quickly
- paid usage stays limited to high-value outputs
- total operating cost stays low enough for an early-stage startup

## Final Decision

### Chosen direction
**Hybrid stack: Ollama + Gemini Flash**

### Reason
It is the most cost-effective architecture that still protects output quality where it matters.
