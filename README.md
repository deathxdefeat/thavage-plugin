# AI Cost Estimator — Claude Code Plugin

**What would your project have cost without AI?**

A Claude Code plugin that estimates what a traditional team would have charged to build any project, then compares it to what it actually cost using AI tools. Produces a full cost breakdown, timeline comparison, team size reduction, ROI calculation, and shareable report.

## What's included

| Component | Path | Purpose |
|---|---|---|
| **Skill** | `skills/ai-cost-estimator/SKILL.md` | Core estimation logic with locked rate table, calculation formulas, and output format |
| **JSON spec** | `skills/ai-cost-estimator/references/output-spec.md` | Canonical JSON schema for programmatic output |
| **Slash command** | `commands/cost-estimate.md` | `/cost-estimate` command for quick estimation from the terminal |

## Install

### Claude Code (recommended)

```bash
claude plugin install deathxdefeat/ai-cost-estimator-plugin
```

### Manual install

Clone into your project's `.claude/plugins/` directory:

```bash
git clone https://github.com/deathxdefeat/ai-cost-estimator-plugin.git .claude/plugins/ai-cost-estimator-plugin
```

### Claude.ai

Download the `.skill` file from the [Pre-AI Price Tag tool page](https://pre-ai-price-tag.vercel.app) and upload via Settings > Features.

## Usage

### Slash command

```
/cost-estimate
```

Scans your current project context and produces a full pre-AI vs post-AI cost comparison.

### Natural language

Just describe a project:

> "Estimate what this project would have cost without AI"

> "What's the traditional development cost for what I've built?"

> "Run a cost comparison on this codebase"

## Rate table (US Market, 2025–2026)

| Role | Rate/hr |
|---|---|
| Senior Full-Stack Developer | $125 |
| Mid-Level Developer | $95 |
| DevOps / Infrastructure | $110 |
| Data Engineer | $120 |
| Product Manager | $95 |
| UX/UI Designer | $85 |
| Researcher / Analyst | $75 |
| Compliance / Legal | $150 |
| Copywriter / Content | $85 |
| QA Engineer | $70 |

Rates are locked. No ranges. Conservative by default — hours round UP when uncertain.

## Output

The estimate includes:

- **Before vs. After table** — cost, hours, calendar time, team size with percentage reductions
- **Key numbers** — speed multiplier, ROI on AI tools, net savings, $/AI hour
- **Pre-AI line items** — 6–12 components with role-based rates
- **Post-AI line items** — AI tool costs + human oversight hours
- **Full project forecast** — projected total cost if project is partially complete
- **Assumptions** — rate basis, exclusions, confidence level

Request JSON output for programmatic use (follows the canonical schema in `references/output-spec.md`).

## Web tool

Try the interactive version at [pre-ai-price-tag.vercel.app](https://pre-ai-price-tag.vercel.app) — file uploads, visual comparisons, shareable reports.

## License

Apache-2.0 — See [LICENSE](LICENSE)

Built by [F3 Strategy](https://f3strategy.com)
