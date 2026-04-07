# Search Strategy — How the Agent Finds Jobs

## Overview

The agent uses two complementary methods for each company:
1. **Direct career page fetch** — hits the company's own job board URL
2. **Web search fallback** — uses search engines to surface listings on LinkedIn, Greenhouse, Lever, etc.

This combination maximizes coverage: direct fetches are fast and reliable for open career pages, while web search catches listings on third-party platforms (LinkedIn, Greenhouse, Lever) that the company also posts to.

---

## Why Not Scrape LinkedIn / Glassdoor Directly?

LinkedIn and Glassdoor are technically public but actively block automated access:
- They detect non-human traffic and return CAPTCHAs or empty results
- Job search results often require a logged-in session
- Their Terms of Service prohibit automated scraping

**Workaround:** Search engines (Google/Bing) index LinkedIn job pages and surface them as public URLs. A web search query like:
```
site:linkedin.com/jobs "Data Scientist" "Experimentation" "New York"
```
returns LinkedIn job URLs that are accessible without login. This captures ~70-80% of LinkedIn postings indirectly.

---

## Career Page URLs by Company

| Company | Direct Career Page URL |
|---|---|
| Meta | https://www.metacareers.com/jobs?q=data+scientist |
| Google | https://careers.google.com/jobs/results/?q=data+scientist+experimentation |
| Amazon | https://www.amazon.jobs/en/search?base_query=data+scientist+experimentation |
| Apple | https://jobs.apple.com/en-us/search?search=data+scientist |
| Netflix | https://jobs.netflix.com/search?q=data+scientist |
| Airbnb | https://www.airbnb.com/careers/departments/data-science |
| Uber | https://www.uber.com/us/en/careers/list/?query=data+scientist |
| Lyft | https://careers.lyft.com/jobs?q=data+scientist |
| Spotify | https://careers.spotify.com/jobs?q=data+scientist |
| Stripe | https://stripe.com/jobs/search?q=data+scientist |
| Databricks | https://databricks.com/company/careers |
| OpenAI | https://openai.com/careers |
| Anthropic | https://www.anthropic.com/careers |
| DoorDash | https://careers.doordash.com/jobs?q=data+scientist |
| HubSpot | https://www.hubspot.com/careers/jobs?q=data+scientist |
| Duolingo | https://jobs.lever.co/duolingo |
| TikTok | https://careers.tiktok.com/position?keywords=data+scientist |
| Microsoft | https://jobs.careers.microsoft.com/global/en/search?q=data+scientist |

---

## Filtering Logic

The agent filters candidates by:

1. **Title match** — must contain one of: Data Scientist, Product Data Scientist, Decision Scientist, Growth Data Scientist, Quantitative Analyst, Senior Data Scientist
2. **Content match** — job description must reference at least one of: A/B test, experimentation, causal inference, product analytics, experiment design
3. **Level match** — Senior / L4 / L5 (excludes intern, entry-level, director+)
4. **Location match** — NY, DC, Boston, Miami, SF, Seattle, LA, or Remote

---

## Deduplication Logic

Before writing each daily report, the agent:
1. Reads the last 3 daily report files from `daily_jobs/`
2. Extracts all Application Links already seen
3. Extracts all (Job Title, Company) pairs already seen
4. Skips any new job that matches on either criterion

This ensures a job posted on Monday doesn't re-appear on Tuesday's report.

---

## Time Window

Each run covers: **3pm ET (previous day) → 3pm ET (today)**

This 24-hour rolling window ensures jobs posted in the evening (3pm–midnight) are not missed — a gap that would exist if the window were simply "last calendar day."

---

## Output

Results are written to `daily_jobs/YYYY-MM-DD.md` with up to 30 jobs (15 per coast) in a 7-column table:

| Job Title | Company | Location | Salary Range | Date Posted | Application Link | Why It Matches |
|---|---|---|---|---|---|---|
