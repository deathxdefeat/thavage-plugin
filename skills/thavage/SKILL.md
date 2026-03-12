---
name: thavage
description: Estimate what a project would cost with and without AI. Supports retrospective benchmarking for completed work, prospective DIY year-one planning, and modern-agency benchmarking for scoped software builds. Trigger on phrases like "what would this have cost", "cost comparison", "/cost-estimate", "/plan-build", "/agency-benchmark", "what will it cost to build", "what would an agency charge", or requests for AI savings, ROI, or traditional-team anchors.
license: Apache-2.0
compatibility: Works in Claude.ai, Claude Code, Cowork, and Claude API. No external dependencies required. File system access improves estimates when scanning codebases.
metadata:
  author: f3-strategy
  version: "1.3.0"
  website: https://thavage.com
---

# Thavage — AI Cost Estimator

Generate either:
- a retrospective benchmark showing what completed work would have cost without AI, or
- a prospective DIY year-one budget for a planned software build, anchored against a traditional team quote, or
- a modern-agency benchmark showing what an AI-native partner would likely charge for the same planned build.

## When to use this skill

- User describes a project and asks what it would have cost
- User says "/cost-estimate" or "estimate this project"
- User says "/plan-build" or asks what it will cost to build a planned product
- User says "/agency-benchmark" or asks what a modern agency would charge
- User asks about AI savings, ROI, or cost comparison
- User wants to justify AI tool spend with hard numbers
- User is building something and wants to know the traditional equivalent cost

## Mode selection

- Use `retrospective` mode when the user already built something or wants a completed-work benchmark.
- Use `prospective` mode when the user is planning a software build and wants a DIY year-one budget plus a traditional-team anchor.
- Use `modern_agency` mode when the user is planning a software build and wants a low/mid/high quote range for an AI-native partner.
- If the mode is ambiguous, ask one short clarifying question only if needed. Otherwise infer from language like "built" vs "want to build".

## How to gather context

Before generating an estimate, scan everything available:
- Codebase, architecture, file structure
- Project docs, READMEs, specs
- Conversation history and memory
- Any description the user provides

If the project is ambiguous, ask ONE clarifying question maximum, then proceed with stated assumptions. Be conservative — round hours UP when uncertain.

## Locked Rate Table (US Market, 2025–2026)

Use these exact rates. No ranges. No deviation.

| Role | Rate/hr | Used For |
|---|---|---|
| Senior Full-Stack Developer | $125 | Core engineering, architecture, integrations |
| Mid-Level Developer | $95 | Standard feature work, testing |
| DevOps / Infrastructure | $110 | CI/CD, cloud config, deployment |
| Data Engineer | $120 | Pipelines, schema design, data sourcing |
| Product Manager | $95 | Scoping, requirements, coordination |
| UX/UI Designer | $85 | Wireframes, UI implementation, user flows |
| Researcher / Analyst | $75 | Domain research, data gathering, benchmarking |
| Compliance / Legal | $150 | Regulatory review, policy, legal research |
| Copywriter / Content | $85 | Documentation, marketing copy, content |
| QA Engineer | $70 | Testing, bug triage, test automation |

## Estimation Rules

1. **Value EVERYTHING.** Domain expertise, research, regulatory work, strategy, data sourcing, DevOps, documentation — all get a dollar value. Founder time counts. Thinking counts. Research counts.
2. **Detect mode** automatically: `retrospective` for completed work, `prospective` for planned DIY builds, `modern_agency` for partner-build quote benchmarking.
3. **Retrospective pre-AI line items:** Use the rate table above. If a component spans multiple roles, use the primary role's rate. Aim for 6–12 line items. Minimum 3.
4. **Retrospective post-AI line items:** AI tools + minimal human oversight. Human oversight hours use $125/hr (senior dev supervising AI output). AI subscription cost: Claude Pro ~$20/mo or actual API cost if known. Minimum 3 line items.
5. **Prospective DIY budgets are software-first.** Use them for MVPs, internal tools, dashboards, launch sites, and scoped product builds. If the work is heavily physical, regulated, or services-led, say prospective mode is a weak fit.
6. **Prospective mode must include:** build-phase spend, monthly operating cost, DIY year-one total, and a traditional-team anchor.
7. **Modern-agency mode must include:** a low/mid/high quote range, midpoint benchmark, team shape, and a comparison against DIY and traditional delivery.
8. **When scale is missing in a planning mode,** assume MVP or early-production usage and state that assumption explicitly.
9. **Team size** = number of humans needed (not AI tools).
10. **Calendar time** = wall-clock duration assuming standard work patterns.
11. **Be conservative.** When uncertain, round hours UP, not down. Don't inflate savings — credibility matters more than impressive numbers.
12. **Every line item** must have all required fields. Retrospective line items need `label`, `hours`, `cost`, `rate_basis`. Prospective DIY line items need `label`, `basis`, and `cost`. Modern-agency line items need `label`, `hours`, `cost`, and `rate_basis`.
13. **All cost values are integers** unless the user explicitly asks for more precision.

## Calculations

- `cost_pct` = round((pre_cost - post_cost) / pre_cost × 100)
- `time_pct` = round((pre_months - post_months) / pre_months × 100)
- `team_pct` = round((pre_team - post_team) / pre_team × 100)
- `speed_multiplier` = round(pre_hours / post_hours)
- `roi` = round((pre_ai.total_cost - post_ai.total_cost) / post_ai.total_cost)
- `hero_stat` = formatted net savings string (e.g. "$257,947 saved")
- `hook_line` = one sentence, loss-framed ("What would have cost…" or "What took…")

## Output Format

Use the structure that matches the detected mode.

### Retrospective

```
WHAT THIS WOULD'VE COST WITHOUT AI

[One punchy hook line — loss-framed]

BEFORE vs. AFTER
Metric       | Pre-AI   | Post-AI  | Reduction
-------------|----------|----------|----------
Total Cost   | $X       | $X       | X%
Human Hours  | X hrs    | X hrs    | X%
Calendar Time| X months | X months | X%
Team Size    | X people | X people | X%

KEY NUMBERS
Speed multiplier: Xx faster
ROI on AI tools: Xx (every $1 spent on AI produced $X of value)
Net savings: $X
$/AI hour: $X of value generated per hour of AI work

PRE-AI LINE ITEMS
Component               | Hours | Rate                 | Cost
------------------------|-------|----------------------|--------
[each component]        | X     | $X/hr [role]         | $X
TOTAL                   | X hrs |                      | $X

POST-AI LINE ITEMS
Component               | Hours | Rate                 | Cost
------------------------|-------|----------------------|--------
[each component]        | X     | Claude/API or direct AI tool cost | $X
TOTAL                   | X hrs |                      | $X

FULL PROJECT FORECAST
(Only include if project is partially complete)
                         | Pre-AI   | Post-AI  | Savings
-------------------------|----------|----------|------------------
Estimated full cost      | $X       | $X       | $X (X%)
Estimated full timeline  | X months | X months | X months (X%)

ASSUMPTIONS
Rates: US market 2025–2026
Not included by default: hosting and infrastructure at scale, marketing and customer acquisition, legal entity formation and procurement paperwork, ongoing maintenance after launch
Confidence: [Low / Medium / High] — [one sentence on why]
```

### Prospective

```
DIY YEAR-ONE BUILD BUDGET

[One punchy hook line]

YOU vs. TRADITIONAL TEAM
Metric         | DIY with AI | Traditional Team | Delta
---------------|-------------|------------------|------
Year 1 Cost    | $X          | $X               | X%
Build Phase    | $X          | $X               | X%
Monthly Ops    | $X/mo       | N/A              | N/A
Build Timeline | X weeks     | X months         | Xx faster
Team Shape     | 1-2 people  | X people         | X fewer humans

KEY NUMBERS
DIY year-one cost: $X
Build-phase spend: $X
Monthly ops: $X/mo
Traditional team anchor: $X
Net savings vs traditional: $X

DIY BUILD COSTS
Component               | Basis                           | Cost
------------------------|---------------------------------|--------
[each component]        | subscription, usage, or implementation basis | $X
TOTAL YEAR 1            |                                 | $X

TRADITIONAL TEAM ANCHOR
Component               | Hours | Rate                 | Cost
------------------------|-------|----------------------|--------
[each component]        | X     | $X/hr [role]         | $X
TOTAL                   | X hrs |                      | $X

ASSUMPTIONS
Rates: US market 2025–2026
Monthly ops: recurring SaaS, AI subscriptions, and runtime token/API spend at the stated scale
Not included by default: hosting and infrastructure at scale, marketing and customer acquisition, legal entity formation and procurement paperwork, ongoing maintenance after launch
Confidence: [Low / Medium / High] — [one sentence on why]
```

### Modern Agency

```
MODERN AGENCY COST

[One punchy hook line]

DIY vs. MODERN AGENCY vs. TRADITIONAL
Metric            | DIY with AI | Modern Agency | Traditional Team
------------------|-------------|---------------|------------------
Year 1 / Quote    | $X          | $X-$X         | $X
Delivery Timeline | X weeks     | X weeks       | X months
Team Shape        | 1-2 people  | X people      | X people

KEY NUMBERS
Modern agency midpoint: $X
Quote range: $X-$X
DIY year-one cost: $X
Traditional team anchor: $X
Premium vs DIY: X%
Savings vs traditional: X%

MODERN AGENCY QUOTE BREAKDOWN
Component               | Owner | Hours | Rate          | Cost
------------------------|-------|-------|---------------|--------
[each component]        | role  | X     | $X/hr [role]  | $X
MIDPOINT TOTAL          |       | X hrs |               | $X

TEAM SHAPE
Role                    | Hours | Cost
------------------------|-------|--------
[each role]             | X     | $X

ASSUMPTIONS
Rates: US market 2025–2026
Included: discovery, design, engineering, QA, launch hardening, and short stabilization
Not included by default: hosting and infrastructure at scale, marketing and customer acquisition, legal entity formation and procurement paperwork, ongoing maintenance after launch
Confidence: [Low / Medium / High] — [one sentence on why]
```

## What to exclude (state in assumptions)

- Ongoing hosting/infrastructure costs
- Marketing and customer acquisition
- Legal entity formation
- Ongoing maintenance post-launch
- Third-party SaaS subscriptions (unless core to build)

## JSON output mode

If the user asks for JSON output (for the web tool, API integration, or data export), read `references/output-spec.md` and use the retrospective, prospective, or modern-agency variant that matches the mode.

## Tone

Be direct and factual. No fluff. No cheerleading. Let the numbers speak. Call out what's genuinely hard to estimate and why. Credibility over impressiveness.
