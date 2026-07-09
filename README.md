# sales-insights — sample data product (demo)

A small, self-contained **dbt (fabricspark) + dlt** data product used to exercise the
**data-ops (Fix)** agent from Vibedata Studio. It models a fictional commerce
`orders` feed into a monthly revenue mart (`mart_sales`), with dlt-style run/audit
tables. There is **no real data and no secrets** here — warehouse coordinates are
supplied at runtime via env (`.env.template` shows the shape).

## Layout
- `intents/sales-insights/` — the product **intent** + **design** (the contract the agent reasons against)
- `models/staging/` — `stg_orders` (revenue-bearing filter) + `sources.yml`
- `models/marts/` — `mart_sales` (monthly revenue rollup)
- `ingestion/pipelines/orders/` — the dlt pipeline stub
- `profiles.yml` — sandbox (`dev`) fabricspark profile (auth via the Studio credential broker)
- `.env.template` — Studio-shape env (workspace/lakehouse IDs are injected at runtime)

## Branches — bind a Studio domain to one, then ask the data-ops agent
- **`main`** — the healthy deployed product. Good for **EXPLAIN / freshness** questions
  (e.g. *"Is `sales_mart` up to date as of this morning?"*).
- **`defect/low-march-revenue`** — a shipped bug: `stg_orders` drops revenue-bearing
  orders, so a month's `total_revenue` reads low. Drives the **code-change → PR** flow
  (e.g. *"Why is total_revenue for March too low?"*).
- **`defect/currency-drift`** — an undeclared source drift inflates revenue. Drives the
  same fix flow (e.g. *"revenue looks inflated since the feed changed — find and fix it"*).

> The defect branches reproduce fully only when the **warehouse also carries the
> matching data** — the code alone declares the defect; the symptom shows once the
> corresponding source data is loaded into the lakehouse/warehouse the domain points at.

Generated from the vd-data-engineering eval fixtures.
