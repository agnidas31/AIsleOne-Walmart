# AISLEONE CRM -- Portable Dashboard Demos

Standalone, sanitized snapshots of five dashboards from the `AISLEONE-CRM`
reporting suite, rebuilt for
demo purposes with zero backend, zero auth, zero live data connection.

A sixth dashboard (**Agent Assist**) was already built as a portable demo in
a prior session -- see `../agent-assist-demo/`. `index.html` in this folder
links out to it alongside the five new ones.

## Files

| File | Covers |
|---|---|
| `index.html` | Landing hub linking all six dashboard demos |
| `aht-savings-demo.html` | AHT Savings -- trend vs. prior year + scenario bands, verification rollout, milestone tracking |
| `aht-deepdive-demo.html` | AHT Deepdive -- fiscal-week trend, LOB/channel/partner breakdowns |
| `fixit-ops-demo.html` | Fixit Ops -- incident funnel, SLA trend, status/source mix, appeals, surveys |
| `qsr-dashboard-demo.html` | QSR Dashboard -- Billable Contacts, AHT, Transfer Rate, CSAT, CPO, RCR, Appeasements |
| `spark-dashboard-demo.html` | Spark Dashboard -- order volume, Force Complete rate, CPO, contact reasons |

Each file is **fully self-contained** -- one HTML file per dashboard, with
CSS and JS inlined. The only external dependency is Chart.js (and the
annotation plugin, for AHT Savings), pulled from a CDN. Open any file
directly via `file://` or double-click; no build step, no server.

## Data provenance & sanitization

Source: `AISLEONE-CRM` repo, frontend chart components under
`frontend/src/{AHT_Savings,AHT_Deepdive,Fixit_Ops,QSR_Dashboard,Spark_Dashboard}/`
and the backend's own built-in demo/fallback generator
(`backend/app/services/aht_service/_demo.py`, used whenever BigQuery is
unavailable -- already synthetic, no production numbers).

For every dashboard:

1. **Category labels kept, values regenerated.** Generic taxonomy (LOB names,
   incident status/source buckets, QSR metric names, Spark contact reasons)
   is descriptive, not confidential, and was kept as-is for realism.
2. **Every numeric value is freshly generated** with a fixed-seed
   pseudo-random generator (`mulberry32`, seeded per dashboard), not copied
   from the app's literal source values (e.g. the real AHT Savings module
   ships hard-coded monthly/weekly scenario arrays tied to a real projection
   -- this demo regenerates its own curves with a similar shape instead of
   reusing those numbers).
3. **Partner/vendor names genericized.** The AHT Deepdive module's demo
   fallback data references real external BPO partner names purely as fallback placeholder text -- this demo
   replaces them with `Partner A`–`Partner E` to avoid implying anything
   about real vendor relationships.
4. **Qualitative story preserved.** Trend directions, funnel drop-off shape,
   which category dominates, and the relationship between paired metrics
   (e.g. SLA compliance vs. escalation rate, CSAT vs. Transfer Rate) are all
   intentionally consistent and sign-safe, so the insights read correctly
   regardless of which way a given seed happened to jitter the numbers.

## What's covered per dashboard

- **AHT Savings** (3 tabs): AHT Savings (monthly + weekly trend vs. prior
  year with Conservative/Best projection bands), Verification Rollout Trend
  (daily % + 7-day rolling avg with rollout annotation), AHT Tracking
  (actuals vs. projections with milestone annotations).
- **AHT Deepdive** (3 tabs): Overview (fiscal-week trend vs. prior year),
  By Line of Business (AHT + volume bar/table), By Channel & Partner
  (channel mix doughnut + partner AHT comparison).
- **Fixit Ops** (3 tabs): Overview (funnel + weekly volume), Incidents
  (status/source mix, SLA trend, escalation trend), Appeals & Surveys
  (volume + overturn/response rate trends).
- **QSR Dashboard** (2 tabs): Overview (7-metric KPI grid + normalized
  index trend), Metric Trends (per-metric chart with daily/weekly toggle).
- **Spark Dashboard** (2 tabs): Exec Overview (orders, Force Complete, CPO,
  contact rate), Volume & Contact Reason (reason mix + contact-rate trend).

Every tab includes a **Strategic Insights** callout (narrative, numbers-
driven) and a **metric readout strip** (latest-period values at a glance),
matching the pattern established in the Agent Assist demo.

## Regenerating

Each file computes its dataset in-browser from a fixed seed -- to get a
different (but still internally consistent) snapshot, change the seed value
passed to `mulberry32(...)` near the top of the `<script>` block and reload.
