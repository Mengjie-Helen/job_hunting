# Job Hunting — Mengjie Zhang (Helen)

Automated daily job search system targeting **Data Scientist / Experimentation** roles at top tech companies.

## Target Profile
- **Role:** Data Scientist (Experimentation / Product Analytics), L4/L5
- **Focus:** A/B testing, causal inference, experiment design, product analytics
- **Salary:** $180k+ total comp
- **Locations:** New York, DC, Boston, Miami, San Francisco, Seattle, LA, Remote

## Repo Structure

```
job_hunting/
├── README.md                  # This file
├── work_log.md                # Project progress log
├── agent/
│   ├── prompt.md              # Full prompt used by the scheduled agent
│   └── search_strategy.md    # How the search works: companies, URLs, logic
└── daily_jobs/
│   └── YYYY-MM-DD.md         # Daily job report (auto-generated)
```

## How It Works

A scheduled Claude Code agent runs daily at **3pm ET**, searches 26 company career pages and the web, filters for matching roles, deduplicates against the last 3 days, and writes a markdown report to `daily_jobs/`.

See [`agent/prompt.md`](agent/prompt.md) for the exact instructions the agent follows, and [`agent/search_strategy.md`](agent/search_strategy.md) for a breakdown of the search logic.

## Target Companies

| East Coast | West Coast |
|---|---|
| Amazon (HQ2 Arlington) | Meta (Menlo Park) |
| Google NYC | Google (Mountain View) |
| Meta NYC | Apple |
| Spotify | Netflix |
| TikTok | Airbnb |
| Stripe | Uber |
| Robinhood | Lyft |
| Duolingo | DoorDash |
| Microsoft NYC/DC | Instacart |
| HubSpot | OpenAI |
| Wayfair | Anthropic |
| Klaviyo | Cohere |
| Toast | Scale AI |
| | Databricks |
| | Palantir |
| | Microsoft (Redmond) |
