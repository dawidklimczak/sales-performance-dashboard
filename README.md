# Sales Performance Dashboard

A Streamlit analytics dashboard that turns a large historical call-center/sales order dataset (tens of thousands of orders) into interactive views of agent performance.

## Problem

Raw order and call-conversation exports (order value, call duration, agent, team, customer/agent demographics, timestamps) sat in large CSV dumps that nobody could easily query for patterns — e.g. whether experience level, team, or time of day correlates with order value, or whether specific agent/customer pairings perform differently. The dashboard makes those questions answerable interactively instead of requiring a one-off analysis every time someone asks.

## Solution & Architecture

- Pandas-based ETL layer that normalizes a messy real-world export: European CSV formatting (semicolon separators, comma decimals, `windows-1250` encoding), currency strings with "zł" suffixes, mixed date/time formats — all coerced into clean numeric/datetime columns with fallbacks so a malformed row doesn't crash the whole load.
- Derived features computed once at load time (experience buckets, day-of-week, conversation length in minutes) so every downstream chart can reuse them.
- Interactive visualizations (Plotly + Matplotlib/Seaborn) segmenting order value and volume by agent experience, team, gender pairing, day/hour, and more — built as a Streamlit app so a non-technical stakeholder can explore the data themselves instead of requesting a new chart each time.
- Two implementation variants exist in the project (a "full" and a simplified "easy" version), reflecting an iteration toward a leaner, faster-to-load dashboard for day-to-day use.

## Key Product Decisions

- **Defensive data loading over strict schemas** — because the source export format was inconsistent and out of the tool's control, the loader is written to degrade gracefully (warn and coerce) rather than fail hard on the first malformed row, since a partially-broken dashboard is still more useful than a crash on 55k rows of business data.
- **Self-serve exploration over static reports** — the dashboard exposes the underlying dimensions (experience, team, gender, time) as interactive filters rather than shipping a fixed set of pre-baked charts, so new questions don't require new code.

## Outcome

Replaced ad-hoc, one-off spreadsheet analysis of sales/call performance data with a reusable, interactive dashboard that any stakeholder can query themselves.

## Preview

<!-- screenshot placeholder — added separately -->

## Note

Source code is kept in a private repository; happy to share on request.
