# Agentic Bug Bounty Playbook

This note is for Assignment 2 Part B. It keeps the hunt focused on one valid, high-impact, legally scoped in-the-wild finding.

## Working Thesis

The best short-window target class is an AI-assisted product where attacker-controlled text enters an agent, connector, RAG pipeline, code review bot, support bot, or MCP/tool context. The highest-value finding is not "the model can be tricked" by itself. The finding needs a reproducible chain from untrusted input to a concrete security impact.

Good Part B candidate:

- in a public bug bounty, VDP, or explicitly allowed company-run program
- in scope on the program page
- reproducible with test accounts and synthetic data
- no production damage, no real user data, no spam, no destructive action
- produces evidence that maps cleanly to HackerOne High/Critical, Bugcrowd P1/P2, or CVSS 7.0+

## Primary Target Shape

Prioritise products with at least two of these:

- AI assistant with browsing or connectors
- RAG over user-uploaded docs, tickets, issues, PRs, wiki pages, or emails
- GitHub app, Slack app, Google Drive connector, Notion connector, or helpdesk integration
- workflow automation where the model can call tools
- MCP server/client integration, especially tool descriptions and returned resources
- ability to create two test accounts and share content between them

Avoid for the first pass:

- generic jailbreak-only reports
- out-of-scope social engineering
- scanners against production infrastructure
- anything requiring real customer data, payment flows, or account takeover beyond test accounts

## Valid Impact Patterns

### 1. Cross-tenant RAG data leak

Attacker account A uploads or creates content. Victim/test account B asks a normal question. The model returns A-only or B-only data that violates the product's access controls.

Evidence target:

- synthetic canary appears in the wrong account
- source citation or transcript proves the leak came through the AI/RAG path
- repeat at least 3 times with fresh canary values

### 2. Indirect prompt injection with tool abuse

Attacker-controlled content is ingested by an agent. The agent follows that untrusted content and performs an unintended action with the victim/test account's tools.

Evidence target:

- action is visible in audit log, test issue, test comment, draft message, or test ticket
- action was not requested by the user
- prompt injection source is a webpage, doc, issue, PR, ticket, or connector item

### 3. Connector boundary failure

The agent is asked to work on one connected resource, but injected content causes it to read, summarize, modify, or disclose another resource in the same connector.

Evidence target:

- all resources belong to test accounts
- sensitive value is synthetic, for example `INFO5995_CANARY_20260429_<random>`
- transcript shows the agent crossed the requested boundary

### 4. Insecure LLM output handling

The product renders model output unsafely, causing stored or reflected XSS, markdown-based token leakage, malicious links, or automatic external fetches.

Evidence target:

- harmless payload such as `alert(document.domain)` or a synthetic canary callback
- proof that rendering occurs in an authenticated context
- no credential theft, no real token capture

## Minimum Test Protocol

For every candidate:

1. Save program URL and scope screenshot.
2. Record test account names and target asset.
3. Create a unique canary value.
4. Run the same test at least 5 times.
5. Capture:
   - input content
   - user prompt
   - model/agent output
   - network/audit evidence if applicable
   - timestamp and account used
6. Stop immediately if the chain touches real user data or unintended third-party systems.

## Severity Mapping Notes

Likely High:

- cross-user data disclosure with sensitive synthetic data
- unauthorized tool action in another user's workspace
- stored XSS in an authenticated AI workflow

Likely Critical:

- reliable cross-tenant secret disclosure
- unauthorized action that changes security settings, tokens, billing, admin state, or repo code
- chain works against default settings and needs only attacker-controlled content

Keep claims conservative. If the only evidence is "the assistant repeated an instruction," severity is usually informational or low.

## Report Skeleton

Title:

`Indirect prompt injection in <feature> allows <impact>`

Required sections:

- Target and scope evidence
- Preconditions
- Test accounts used
- Steps to reproduce
- Expected result
- Actual result
- Reproducibility rate
- Security impact
- Severity mapping
- Novelty evidence
- Recommended mitigation

Mitigation ideas:

- separate trusted user instructions from untrusted retrieved content
- enforce source-aware instruction hierarchy
- require confirmation before sensitive tool calls
- apply authorization checks after retrieval and before generation
- block automatic external resource loading from AI-rendered content
- render model output in a sanitized, isolated context

