# Thavage — Claude Code Plugin

**What would your project have cost without AI?**

A Claude Code plugin that now supports three flows:
- retrospective benchmarking for work you already built with AI
- prospective DIY year-one budgeting for planned software builds
- modern-agency benchmarking for what an AI-native partner would likely quote

It produces conservative cost anchors, line-item breakdowns, timeline comparisons, and shareable planning artifacts using the same locked rate table as thavage.com.

## What's included

| Component | Path | Purpose |
|---|---|---|
| **Skill** | `skills/thavage/SKILL.md` | Shared estimation logic for retrospective, prospective, and modern-agency modes |
| **JSON spec** | `skills/thavage/references/output-spec.md` | Canonical JSON schema for programmatic output |
| **Slash commands** | `commands/cost-estimate.md`, `commands/plan-build.md`, `commands/agency-benchmark.md` | `/cost-estimate` for completed work, `/plan-build` for planned builds, `/agency-benchmark` for modern-agency quotes |

## Install

### Claude Code (recommended)

```bash
claude plugin install deathxdefeat/thavage-plugin
```

### Manual install

Clone into your project's `.claude/plugins/` directory:

```bash
git clone https://github.com/deathxdefeat/thavage-plugin.git .claude/plugins/thavage-plugin
```

### Claude.ai

Download the `.skill` file from [Thavage](https://thavage.com) and upload via Settings > Features.

## Usage

### Slash command

```
/cost-estimate
```

Scans your current project context and produces a full pre-AI vs post-AI cost comparison.

### Prospective planning command

```
/plan-build
```

Scans the same project context and produces a DIY year-one build budget plus a traditional-team anchor for a planned software build.

### Modern-agency benchmark command

```
/agency-benchmark
```

Scans the same project context and produces a realistic low/mid/high quote range for an AI-native product partner, alongside DIY and traditional anchors.

### Natural language

Just describe a project:

> "Estimate what this project would have cost without AI"

> "What's the traditional development cost for what I've built?"

> "Run a cost comparison on this codebase"

## Locked rate table (US Market, 2025–2026)

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

Retrospective estimates include:

- **Before vs. After table** — cost, hours, calendar time, team size with percentage reductions
- **Key numbers** — speed multiplier, ROI on AI tools, net savings, $/AI hour
- **Pre-AI line items** — 6–12 components with role-based rates
- **Post-AI line items** — AI tool costs + human oversight hours
- **Full project forecast** — projected total cost if project is partially complete
- **Assumptions** — rate basis, exclusions, confidence level

Prospective planning estimates include:

- **DIY year-one total** — build-phase spend plus 12 months of recurring ops
- **Traditional team anchor** — a conservative quote benchmark using the same rate table
- **DIY line items** — subscriptions, runtime costs, and implementation assumptions
- **Timeline comparison** — AI-assisted build timeline versus traditional delivery
- **Assumptions** — rate basis, exclusions, confidence level, and scale assumptions

Modern-agency estimates include:

- **Quote range** — low, midpoint, and high benchmark values
- **DIY vs agency vs traditional comparison** — three-lens decision framing
- **Quote breakdown** — line items, owners, hours, and midpoint benchmark
- **Team shape** — role mix, time, and phase emphasis
- **Assumptions** — inclusions, exclusions, delivery premium, and confidence

Request JSON output for programmatic use. The spec now supports both retrospective and prospective variants in `references/output-spec.md`.

## Web tool

Try the interactive version at [thavage.com](https://thavage.com) — methodology, examples, use cases, comparisons, file uploads, visual comparisons, Modern Agency Cost benchmarking, and shareable reports.

## Privacy Policy

This plugin collects no user data. It consists entirely of local Markdown instruction files that run within your Claude session. No network requests, no telemetry, no data transmission to F3 Strategy or any third party.

The companion web tool at thavage.com collects anonymous submission data for benchmark analysis. For complete privacy information, see our privacy policy: https://thavage.com/privacy

### Data Collection
- **Plugin/Skill**: Zero data collected. Fully local.
- **Web tool**: Anonymous project descriptions and estimate results only. No names, emails, or personally identifiable information.
- **Third parties**: Web tool sends descriptions to Anthropic's API for estimation and stores anonymous results in Supabase. Data is never sold.
- **Retention**: Anonymous web submissions retained indefinitely for benchmarking. Plugin stores nothing.

## License

Apache-2.0 — See [LICENSE](LICENSE)

Built by [F3 Strategy](https://f3strategy.com)
