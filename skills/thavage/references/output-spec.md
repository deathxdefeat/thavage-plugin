# AI Cost Estimator — JSON Output Spec

Use this spec when the user requests JSON output for the web tool, API, or data export. Return ONLY valid JSON — no markdown, no backticks, no explanation.

## Mode

Always include `"mode": "retrospective"`, `"mode": "prospective"`, or `"mode": "modern_agency"` and then use the matching schema.

## Retrospective Schema

```json
{
  "mode": "retrospective",
  "project_name": "string — short name for the project",
  "project_type": "software | physical | hybrid",
  "completion_pct": 0-100,
  "summary_line": "string — one punchy sentence describing what was built",
  "pre_ai": {
    "total_cost": 123456,
    "total_hours": 1234,
    "calendar_months": 12,
    "team_size": 5,
    "line_items": [
      {
        "label": "Component name",
        "hours": 100,
        "cost": 12500,
        "rate_basis": "$125/hr senior full-stack dev"
      }
    ]
  },
  "post_ai": {
    "total_cost": 1234,
    "total_hours": 12,
    "calendar_months": 1,
    "team_size": 1,
    "line_items": [
      {
        "label": "Component name",
        "hours": 2,
        "cost": 40,
        "rate_basis": "Claude Pro subscription prorated"
      }
    ]
  },
  "forecast_full": {
    "enabled": true,
    "pre_ai_total": 234567,
    "post_ai_total": 2345,
    "pre_ai_months": 18,
    "post_ai_months": 3
  },
  "reductions": {
    "cost_pct": 98,
    "time_pct": 91,
    "team_pct": 80
  },
  "speed_multiplier": 69,
  "roi": 4868,
  "hero_stat": "$257,947 saved",
  "hook_line": "What took a team of 8 two years now took one person 30 hours."
}
```

## Prospective Schema

```json
{
  "mode": "prospective",
  "project_name": "string — short name for the project",
  "project_type": "software",
  "summary_line": "string — one punchy sentence describing what will be built",
  "traditional_anchor": {
    "total_cost": 95000,
    "total_hours": 700,
    "calendar_months": 6,
    "team_size": 4,
    "line_items": [
      {
        "label": "Component name",
        "hours": 120,
        "cost": 15000,
        "rate_basis": "$125/hr senior full-stack dev"
      }
    ]
  },
  "mode2_additions": {
    "year_one_total": 6200,
    "build_phase_cost": 2400,
    "monthly_operations_cost": 320,
    "build_timeline_weeks": 10,
    "ai_subscription_monthly": 20,
    "saas_monthly": 180,
    "tools_stack": [
      {
        "service": "Supabase",
        "tier": "Free | Paid",
        "monthly_cost": 0
      }
    ],
    "diy_line_items": [
      {
        "label": "Hosting and database",
        "basis": "12 months of recurring usage at stated scale",
        "cost": 300
      }
    ]
  },
  "reductions": {
    "cost_pct": 93
  },
  "speed_multiplier": 6,
  "hero_stat": "$88,800 cheaper than hiring a team",
  "hook_line": "What a team of 4 would quote at $95K, you can build yourself for under $6.2K in year one."
}
```

## Modern Agency Schema

```json
{
  "mode": "modern_agency",
  "project_name": "string — short name for the project",
  "project_type": "software",
  "summary_line": "string — one punchy sentence describing what will be built",
  "traditional_anchor": {
    "total_cost": 95000,
    "total_hours": 700,
    "calendar_months": 6,
    "team_size": 4,
    "line_items": [
      {
        "label": "Component name",
        "hours": 120,
        "cost": 15000,
        "rate_basis": "$125/hr senior full-stack dev"
      }
    ]
  },
  "mode2_additions": {
    "year_one_total": 6200,
    "build_phase_cost": 2400,
    "monthly_operations_cost": 320,
    "build_timeline_weeks": 10
  },
  "modern_agency_quote": {
    "low_total": 85000,
    "mid_total": 104000,
    "high_total": 129000,
    "timeline_weeks": 14,
    "line_items": [
      {
        "label": "Component name",
        "hours": 60,
        "cost": 10500,
        "rate_basis": "$175/hr senior full-stack / ai engineer"
      }
    ],
    "team_shape": [
      {
        "role": "Senior Full-Stack / AI Engineer",
        "hours": 220,
        "cost": 38500
      }
    ]
  },
  "reductions": {
    "cost_pct": -58,
    "vs_traditional_cost_pct": -9,
    "premium_vs_diy_pct": 1577
  },
  "speed_multiplier": 5,
  "hero_stat": "$104,000 midpoint benchmark",
  "hook_line": "What you could build yourself for $6.2K in year one is still a six-figure partner-build engagement when you want a team on the hook."
}
```

## Schema Rules

- `forecast_full.enabled` = true only when `completion_pct` < 100
- `reductions` percentages are integers (0–100): `round((pre - post) / pre * 100)`
- `speed_multiplier` = `round(pre_ai.total_hours / post_ai.total_hours)`
- `roi` = `round((pre_ai.total_cost - post_ai.total_cost) / post_ai.total_cost)`
- `hero_stat` = formatted string of net savings (e.g. "$257,947 saved")
- `hook_line` = one sentence, loss-framed ("What would have cost…" or "What took…")
- All cost values are integers (no decimals)
- Retrospective `line_items` must have at least 3 items per section; aim for 6–12 for pre-AI
- Retrospective line items must have all 4 fields populated: `label`, `hours`, `cost`, `rate_basis`
- Prospective `diy_line_items` should cover build-phase and recurring cost categories and use `label`, `basis`, and `cost`
- Modern-agency `line_items` should cover the midpoint benchmark and use `label`, `hours`, `cost`, and `rate_basis`
