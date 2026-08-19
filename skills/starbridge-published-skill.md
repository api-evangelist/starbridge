---
name: Starbridge
description: Use when building and configuring Bridges to monitor buying signals, enriching buyer and contact data, syncing intelligence to CRMs, or querying the REST API for programmatic access to buyer and signal data. Reach for this skill when helping users find opportunities, build target account lists, identify decision-makers, or automate go-to-market workflows in the public sector.
metadata:
    mintlify-proj: starbridge
    version: "1.0"
---

# Starbridge Skill

## Product summary

Starbridge is an AI-native GTM intelligence platform for teams selling to local government, K-12, and higher education. It turns public sector data into actionable intelligence through **Bridges** — smart spreadsheets powered by AI agents that automatically monitor buying signals (meetings, RFPs, contracts, job changes), enrich them with contacts and account intelligence, and deliver results to your CRM, inbox, or Slack.

**Key files and concepts:**
- **Bridges**: Core unit of work — a monitored search for signals across a buyer list, enriched with columns
- **Buyer lists**: Define the universe of accounts to monitor (dynamic filters or static uploads)
- **Columns**: Enrichment layers added to Bridges (AI analysis, contacts, CRM sync, web research, personalized outreach)
- **Consumer view**: How reps see and interact with a Bridge (filtered, sorted, shared)
- **REST API**: Programmatic access to bridges, buyers, signals, and columns at `https://core-http2-157602306522.us-central1.run.app/external/public/swagger/documentation.yaml`

**Primary docs:** https://hc.starbridge.ai

## When to use

Reach for this skill when:
- **Building or configuring Bridges** — creating signal monitors for meetings, RFPs, contracts, job changes, contacts, or custom web signals
- **Setting up buyer lists** — defining static or dynamic account universes to monitor
- **Enriching Bridges with data** — adding AI analysis, contact enrichment, CRM lookups, personalized outreach, or web research columns
- **Configuring CRM integrations** — syncing account data, contacts, signals, or job changes to Salesforce or HubSpot
- **Sharing Bridges with teams** — configuring consumer views and permissions for reps
- **Querying the API** — retrieving bridge data, buyer attributes, or signals programmatically
- **Troubleshooting signal quality** — adjusting search phrases, match score thresholds, or scoring criteria

## Quick reference

### Bridge types and when to use each

| Bridge Type | Data Anchor | Use When | Credits |
|---|---|---|---|
| **Meetings** | Board meetings, council sessions, strategic plans | You want early signals before RFPs or budgets are finalized | No |
| **RFPs** | Active procurement postings | You want to know the moment a relevant bid is posted | No |
| **Purchases** | Contracts, purchase orders, spend data | You want to identify active spend, contract expirations, or competitor intelligence | No |
| **Job Changes** | Leadership transitions, new hires, role changes | You want to reach new decision-makers early | Yes |
| **Contacts** | Verified decision-makers and key roles | You want to build prospecting lists or fill CRM gaps | Yes |
| **Buyers** | Static or dynamic account lists | You want to score your market or create a foundation for other Bridges | No |
| **Conferences** | Upcoming events and attendees | You want to maximize conference presence or build pre-event outreach | No |
| **Custom Web Signals** | Anything else (grants, news, announcements) | You have a specific signal outside standard types | Yes |

### Bridge building workflow (3 phases)

1. **Describe** — Write natural language description of what you're looking for (who, what, timeframe, what "good" looks like)
2. **Refine** — Review and tune buyer filters, bridge filters, search phrases, and match score criteria; preview results
3. **Create** — Save the Bridge when preview looks right

### Column types and use cases

| Column Type | Purpose | Input | Output |
|---|---|---|---|
| **Bridge-specific** | Pull standardized attributes tied to signal (meeting date, RFP due date, contract value) | Signal type | Structured field |
| **AI Analysis** | Score, analyze, or generate insights using natural language | Prompt + other columns | Text, number, or structured output |
| **Web Agent** | Search the internet for supplementary context | Prompt | Text or structured output |
| **Vendor Presence** | Identify competitors or vendors a buyer works with | Competitor list | Vendor names, pricing, contract dates |
| **Personalized Outreach** | Generate tailored emails for each row | Template + dynamic variables | Email text |
| **CRM Lookup** | Pull data from your CRM Account or Contact object | CRM field mapping | CRM record data |
| **CRM Sync** | Push Starbridge data back to your CRM | Field mapping + write behavior | Success/error status |
| **Reference** | Bring data from one Bridge into another | Source Bridge + column | Referenced data |

### API endpoints (REST)

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/external/bridge/{bridgeId}` | GET | Fetch Bridge configuration, status, row count |
| `/api/external/bridge` | GET | List all Bridges (filter by name, type, owner, access) |
| `/api/external/bridge/{bridgeId}/rows` | GET | List Bridge rows with filtering and pagination |
| `/api/external/bridge/{bridgeId}/rows/{rowId}/status` | PUT | Update row status (Actioned, Starred, Not interested) |
| `/api/external/buyer/quick/search` | GET | Search for buyers by name and state |
| `/api/external/buyer/{buyerId}` | GET | Get buyer attributes and summary |
| `/api/external/signal` | GET | List recent signals for a buyer or Bridge |

### Common configuration patterns

**Match score threshold tuning:**
- **Too noisy?** Raise threshold to exclude low-confidence matches
- **Missing results?** Lower threshold or add search phrases
- **Scores feel off?** Adjust positive/negative criteria or use free-form scoring prompt

**Search phrase strategy:**
- Start broad (cast wide net with 20–30 phrases for Meetings)
- Filter hard in scoring criteria (positive/negative match criteria or free-form prompt)
- Use "Add similar phrases" to expand coverage automatically

**Buyer filter options:**
- **Buyer lists** — pre-configured static or dynamic lists
- **Buyer filters** — geography, type, size, classification, attributes
- **Select buyers** — pick individual accounts by name

## Decision guidance

### When to use natural language vs. manual filters

| Scenario | Use Natural Language | Use Manual Filters |
|---|---|---|
| You have a clear, specific intent to describe | ✓ | |
| You want Starbridge to generate structured search | ✓ | |
| You need fine-grained control over every filter | | ✓ |
| You're building a complex, multi-criteria search | ✓ | |
| You're familiar with the UI and want speed | | ✓ |
| You want to see exactly what filters were applied | ✓ | |

### When to use positive/negative criteria vs. free-form scoring

| Scenario | Use Positive/Negative | Use Free-Form Prompt |
|---|---|---|
| Standard scoring (strong match + red flags) | ✓ | |
| You need explicit control over score scale (5 vs. 4 vs. 3) | | ✓ |
| You want to calibrate edge cases explicitly | | ✓ |
| You need richer, more detailed reasoning | | ✓ |
| Most use cases (default, generated for you) | ✓ | |

### When to sync to CRM vs. consume in Starbridge

| Data Type | Sync to CRM | Consume in Starbridge |
|---|---|---|
| Account enrichment (budget, enrollment, scores) | ✓ | |
| Contacts (new hires, decision-makers) | ✓ | |
| Dynamic signals (meetings, RFPs, job changes) | ✓ | |
| One-time research or analysis | | ✓ |
| Data reps need in their system of record | ✓ | |
| Data for builders to refine Bridges | | ✓ |

## Workflow

### Building a Bridge (typical task)

1. **Define your buyer universe**
   - Decide: static list (upload CSV), dynamic filters (geography, type, size), or CRM-synced list
   - Create the buyer list in Builders → Buyer Lists
   - Test that it includes the right accounts

2. **Describe what you're looking for**
   - Open Builders → Create Bridge → select Bridge type
   - Write natural language description: who (buyer type/geography), what (topic/signal), timeframe, what "good" looks like
   - Example: "K–12 school districts in California with 15,000+ students discussing new math curriculum in the last 12 months, prioritizing districts evaluating vendors or forming pilot committees"

3. **Refine the structured search**
   - Review buyer filters (correct geography/type?)
   - Review bridge filters (date range, status, issuer type correct?)
   - Review search phrases (add/remove to match your intent)
   - Adjust match score criteria: add strong-match criteria and red flags
   - Preview results and iterate until they look right

4. **Create the Bridge**
   - Click "Save search as Bridge"
   - Bridge is created with all existing matching signals; new signals are added automatically

5. **Enrich with columns**
   - Click "Add enrichment" and select column type
   - For AI analysis: write prompt, set output format, test on first 5 rows
   - For CRM sync: map fields, choose write behavior (Overwrite vs. Write if empty)
   - Run on all rows once preview looks correct

6. **Configure consumer view**
   - Switch to Consume view
   - Customize layout, set default filters, configure sorting
   - Save consumer view

7. **Share with team**
   - Click "Share" button
   - Select users or teams to grant access
   - Consumers are automatically subscribed to the Bridge

### Syncing data to CRM (typical task)

1. **Connect your CRM** (if not already done)
   - Go to Settings → Integrations → Connect CRM
   - Authenticate Salesforce or HubSpot
   - Enable Account and Contact objects and required fields

2. **Complete account matching**
   - Go to Settings → Account Matching
   - Map CRM accounts to Starbridge buyers
   - Resolve unmatched accounts

3. **Plan what to sync**
   - Decide: account enrichment, contacts, signals, or job changes
   - Identify target CRM fields (standard or custom)

4. **Add lookup column** (if pulling CRM data into Bridge)
   - Click "Add enrichment" → Configure integration → Pull data from my CRM
   - Select Account or Contact object
   - Configure matching rule (use org-level account matching or custom rule)
   - Test on first 5 rows

5. **Add sync column** (if pushing Bridge data to CRM)
   - Click "Add enrichment" → Configure integration → Send data to my CRM
   - Select object and sync type (Update or Upsert)
   - Map Starbridge columns to CRM fields
   - Choose write behavior: Overwrite (always update) or Write if empty (fill once)
   - Add run conditions if needed (e.g., only sync if lookup succeeded)
   - Test on first 5 rows, then run all rows

6. **Verify sync**
   - Check CRM records to confirm data was written correctly
   - Review field values and account associations

### Querying the API (typical task)

1. **Generate API key**
   - Go to Settings → API Keys
   - Click "Create API Key"
   - Give it a descriptive name (e.g., "Zapier integration", "Custom app")
   - Copy and store securely

2. **Authenticate requests**
   - Include API key in Authorization header: `Authorization: Bearer YOUR_API_KEY`
   - All requests go to `https://core-http2-157602306522.us-central1.run.app/external/public/swagger/documentation.yaml`

3. **List Bridges**
   - GET `/api/external/bridge`
   - Filter by name, type, owner, or access
   - Returns Bridge IDs, names, filter types, status, row counts

4. **Get Bridge rows**
   - GET `/api/external/bridge/{bridgeId}/rows`
   - Paginate with `pageNumber` and `pageSize`
   - Filter by column values or row status
   - Returns row data with all columns

5. **Search buyers**
   - GET `/api/external/buyer/quick/search?buyerName=...&buyerStateCode=...`
   - Returns matching buyers with IDs, types, locations
   - Use buyer ID for downstream queries

6. **Get buyer signals**
   - GET `/api/external/signal?buyerId=...`
   - Returns recent signals (meetings, RFPs, job changes) for that buyer
   - Filter by bridge type or date range

## Common gotchas

- **Search phrases too narrow** — You'll miss relevant signals. Start broad (20–30 phrases), filter hard in scoring criteria.
- **Match score threshold too high** — You'll exclude good matches. Preview "Below threshold" to see what you're missing.
- **Buyer list doesn't include all target accounts** — Verify the list before building Bridges on it. Use dynamic filters or upload a CSV with complete account data.
- **CRM account matching incomplete** — Unmatched accounts won't sync. Complete account matching before adding sync columns.
- **Write behavior set to "Overwrite" when you meant "Write if empty"** — You'll overwrite existing CRM data. Double-check write behavior before running sync.
- **Forgetting to test on first 5 rows** — Prompts are iterative. Always test enrichment columns on a small sample before running all rows.
- **Editing natural language prompt after Bridge creation** — You can't. If you want a fundamentally different intent, duplicate the Bridge and start over.
- **Syncing dynamic signals without a custom object** — Job changes, RFPs, and meetings are multi-instance per account. Sync to a custom object with automations, not directly to Account.
- **Not setting run conditions on sync columns** — Sync will attempt to run on every row, including those with failed lookups. Add conditions like "Only run if lookup Record ID is not empty."
- **API key exposed in code** — Store API keys in environment variables or secrets management, never in version control.

## Verification checklist

Before sharing a Bridge or syncing data:

- [ ] **Bridge configuration**: Preview shows relevant results, match scores look right, no obvious false positives
- [ ] **Search phrases**: Broad enough to catch relevant signals, specific enough to exclude noise
- [ ] **Buyer list**: Includes all target accounts, no obvious gaps or mismatches
- [ ] **Enrichment columns**: Tested on first 5 rows, output format is correct, no errors
- [ ] **Consumer view**: Layout is clear, default filters make sense, sorting is intuitive
- [ ] **Sharing**: Correct users/teams have access, Bridge is shared before consumers try to view it
- [ ] **CRM sync**: Account matching is complete, field mappings are correct, write behavior matches intent
- [ ] **Run conditions**: Sync columns have conditions to prevent running on failed lookups or invalid data
- [ ] **API integration**: API key is stored securely, requests are authenticated, pagination is handled correctly

## Resources

**Comprehensive page listing:** https://hc.starbridge.ai/llms.txt

**Critical documentation:**
- [How Bridges Work](https://hc.starbridge.ai/what-is-a-bridge) — Foundational overview of Bridge architecture and use cases
- [Building a Bridge](https://hc.starbridge.ai/builders/how-to-build-a-bridge) — Step-by-step walkthrough of the full Bridge creation workflow
- [CRM Integration Overview](https://hc.starbridge.ai/crm-integration) — Setup sequence for connecting Salesforce or HubSpot and syncing data
- [REST API Overview](https://hc.starbridge.ai/api-reference/rest/overview) — Programmatic access to Bridges, buyers, and signals

---

> For additional documentation and navigation, see: https://hc.starbridge.ai/llms.txt