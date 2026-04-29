# Part B Target Shortlist

Retrieval date: 2026-04-29.

## Ranking Rule

Score each target from 0 to 2 for each column:

- Scope clarity: program explicitly allows the feature and testing method
- AI attack surface: agent, connector, RAG, MCP, chatbot, code review bot, or automation
- Testability: two test accounts or controllable workspace available
- Impact path: plausible data leak, unauthorized action, or unsafe rendering
- Duplicate risk: lower score if many public reports already cover the same issue class

Prioritise total score >= 7.

## Candidate 1: OpenAI Safety Bug Bounty

Platform:

- Bugcrowd

Why it matters:

- OpenAI's Safety Bug Bounty explicitly includes agentic risks including MCP.
- The public announcement lists third-party prompt injection and data exfiltration where attacker text reliably hijacks a victim's agent, including Browser, ChatGPT Agent, and similar agentic products.
- Reports need plausible material harm and reproducibility; the announcement says third-party prompt injection/data exfiltration must reproduce at least 50% of the time.

Links:

- https://openai.com/index/safety-bug-bounty/
- https://bugcrowd.com/openai
- https://openai.com/index/prompt-injections/

Initial focus:

- Browser / ChatGPT Agent reading attacker-controlled webpages
- connector content that tries to cross a user-requested resource boundary
- MCP/tool context injection, only where third-party terms permit testing

Do not submit:

- generic jailbreaks with no abuse impact
- claims based only on rude output or public information
- tests that violate third-party terms

## Candidate 2: GitHub Bug Bounty / GitHub AI Workflows

Platform:

- HackerOne / company-run GitHub program

Why it matters:

- Assignment 2 explicitly allows GitHub as a company-run program.
- GitHub has AI and automation surfaces: Copilot Chat, code review assistants, issue/PR automation, Actions workflows, and app integrations.

Initial focus:

- attacker-controlled issue or PR text influencing an AI review or triage workflow
- unsafe markdown/rendering in AI-generated comments
- permission boundary confusion between public and private repo context

Guardrails:

- use owned repositories only
- do not target third-party repositories without scope
- keep all test secrets synthetic

## Candidate 3: HackerOne Programs With AI Features

Platform:

- HackerOne

Search terms:

- `AI`
- `assistant`
- `LLM`
- `agent`
- `chatbot`
- `automation`
- `workflow`
- `copilot`
- `connector`
- `MCP`
- `RAG`

Initial focus:

- support bots that ingest tickets or knowledge base pages
- SaaS products with AI document search
- code review or issue triage bots
- workflow products that let an assistant call actions

Selection rule:

- only test assets listed in scope
- reject programs that prohibit automation, external callbacks, AI testing, or multi-account testing unless permission is explicit

## Candidate 4: OpenClaw

Platform:

- explicitly allowed company-run target for Assignment 2

Initial focus:

- plugin and MCP trust boundaries
- prompt injection through workspace files or tool descriptions
- unsafe sharing of main-session memory into shared contexts

Guardrail:

- confirm course/project scope before active testing if the target is not already covered by an explicit brief.

