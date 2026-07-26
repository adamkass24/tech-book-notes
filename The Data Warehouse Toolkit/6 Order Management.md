---
date: 2026-07-26
type: reference
repo: tech-book-notes
origin: curated
threads: [learning, work]
decisions: []
open_loops: []
---

# Chapter 6 — Order Management (Kimball & Ross, *The Data Warehouse Toolkit*, 3rd ed.)

Condensed from an actual read of the chapter (previously this file was a pasted assistant
chat thread, not notes — rewritten 2026-07-26).

## Bus matrix

- Order management is a slice of the enterprise bus matrix: Date, Customer, Product, Sales
  Rep, Deal, Warehouse, Shipper across the process row Quoting -> Ordering -> Shipping to
  Customer -> Shipment -> Invoicing -> Receiving Payments -> Customer Returns.
- This chapter only develops the Order and Invoice rows of that matrix, plus an
  accumulating snapshot that spans the whole fulfillment pipeline.

## Order transactions (grain: one row per order line item)

- Dimensions: order date, requested ship date, product, customer, sales rep, deal. Facts:
  order quantity, extended gross/discount/net dollar amounts.
- **Fact normalization** — resist pivoting facts into a generic (measurement-type, amount)
  pair. It multiplies row count (10M rows x 4 facts becomes 40M rows x 1 fact) and makes
  cross-fact arithmetic (discount as % of gross) painful in SQL when the facts are split
  across rows. Only worth it when facts are sparse/high-cardinality (manufacturing test
  results) or the target is an OLAP cube that computes across any dimension anyway.
- **Dimension role playing** — one physical date dimension serving multiple logical roles
  (order date, requested ship date) via uniquely-labeled views/aliases, because SQL can't
  join two roles to the same physical table in the same query. On the bus matrix, note
  multiple roles inside a single cell rather than adding dimension columns.
- **Product dimension** — verbose (100+ descriptive attributes is normal), multiple
  hierarchies, flattened/denormalized rather than snowflaked. Needs a surrogate key remap
  from the operational product code, readable attributes in place of cryptic codes, and
  quality-checked values (misspellings and code variants silently fragment reports).
- **Customer dimension** — one row per ship-to location; can carry multiple independent
  hierarchies at once (geographic via ship-to, organizational via bill-to/parent). If a
  ship-to can map to more than one bill-to, either widen the grain to the ship-to/bill-to
  combination, or split into two dimensions linked through the fact table.
- **Sales rep vs. customer** — combine into one dimension only when the relationship is
  fixed, 1:1 or many:1, and time-invariant. Otherwise keep separate, especially if the
  customer dimension is huge or the rep participates independently in other fact tables. A
  **factless fact table** (rep-customer assignment, keyed by effective/expiration date) is
  the right tool for analyzing coverage/assignment history independent of actual orders.
- **Deal dimension** — same shape as a promotion dimension: terms, allowances, incentives.
  Combine into one dimension if correlated; split if the combinations would otherwise
  Cartesian-product the dimension. Under ~100K rows is generally tractable as one table.
- **Degenerate dimension** — order number (and optionally order line number) sits directly
  on the fact row with no join, since its real descriptive attributes have already been
  pushed into proper dimensions (date, customer, deal). No separate order-header dimension
  is needed just to hold it.
- **Junk dimensions** — bundle low-cardinality flags/indicators (payment type, order type,
  commission-credit flag) into one dimension rather than: dropping them, leaving them raw
  on the fact row, giving each its own FK, or reviving an order-header dimension to hold
  them. Build rows for combinations actually observed rather than the full theoretical
  Cartesian product when the combination space is large.
- **Two header/line patterns to avoid**: (1) replicating the operational order header
  wholesale as a dimension — it grows at the wrong rate relative to the fact table and
  forces every "about the order" query through an oversized dimension; (2) making the
  header itself a second fact table that the line-item fact table joins to by order number
  — same problem, just relocated.
- **Multiple currencies** — carry paired facts (local-currency amount + standardized
  corporate-currency amount, e.g. USD) rather than a column per currency. Add a currency
  dimension even when the transaction's country is known, since location doesn't guarantee
  currency. For open-ended "any currency to any currency" reporting, add a separate
  currency-conversion fact table keyed by date + source/destination currency.
- **Facts at different granularity** (e.g. an order-level shipping charge vs. line-level
  facts) — the fix is to **allocate** the header-level fact down to the line grain using
  whatever rule the business agrees on, not to mix granularities in one fact table and not
  to park the unallocated amount arbitrarily on the first or last line.

## Invoice transactions (grain: one row per invoice line item)

- Kimball's own framing: invoicing is often the best place to start a DW/BI project because
  it combines customers, products, and the components of profitability in one place.
- Adds warehouse and shipper dimensions, plus service-level tracking: a quantitative
  on-time counter/ship-date lag, and a qualitative, bucketed service-level dimension
  ("1 day early" / "on-time" / "2 days late").
- **Profit and loss (P&L) facts** — a literal waterfall on the fact row: extended gross ->
  less allowance -> less discount -> extended net (what the customer sees) -> less fixed
  mfg cost, variable mfg cost, storage cost, distribution cost -> **contribution** (the
  bottom line here, explicitly not true company profit since G&A isn't included). Because
  it lives inside the ordinary dimensional framework, customer/product/deal profitability
  all become "constrain and group," not bespoke reports. Caveat from the book: the cost
  facts are usually the hard part to source at this granularity — verify feasibility with
  the source systems before promising the full P&L view.
- **Audit dimension** — backroom ETL metadata (data-quality indicator, out-of-bounds
  indicator, amount-adjusted flag, cost-allocation version, currency-conversion version)
  attached via its own FK, so business users can filter/report "was this number suspect"
  with an ordinary dimensional query instead of digging through ETL logs.

## Accumulating snapshot for the order fulfillment pipeline

- Complements the transaction fact tables above when the question shifts from "what
  happened at each step" to "how is this order moving through the whole pipeline, and how
  fast."
- Grain: one row per order line item, but unlike a transaction fact table, **the row is
  revisited and updated in place** as the order moves through each pipeline stage (order ->
  backlog -> released to manufacturing -> finished goods -> shipped -> invoiced). That
  update-in-place behavior — not merely "has several dates" — is what actually defines an
  accumulating snapshot.
- Structurally: many role-played date FKs (one per milestone, including an Unknown/TBD date
  row for milestones not yet reached) plus quantity facts at each stage and precomputed
  **lag** columns (order-to-manufacturing-release, release-to-inventory,
  inventory-to-shipment, order-to-shipment).
- Best fit: short-lived processes with a definite start and end, especially when the unit
  moving through the pipeline has a durable identifier (VIN, serial number, lot number).
  Long-lived processes (bank accounts) belong in periodic snapshots instead.
- If pipeline dimensions carry type-2 attributes, keep the fact row pointed at the current
  surrogate key while the pipeline is active; once a row completes, it is not revisited for
  later type-2 changes.
- **Multiple units of measure** — store facts in one base unit plus a handful of conversion
  factors on the fact row itself, not buried in the product dimension (which would force
  users to multiply/divide by hand and drift out of sync as factors change over time).
  Deliver converted views to users; keep the physical row lean (base facts + conversion
  factors, not every fact times every unit).

## Why this is worth keeping

- The accumulating-snapshot pattern is the most directly reusable idea outside
  retail/manufacturing — any pipeline with discrete stages and a natural terminal state
  (order fulfillment, claims processing, a hiring pipeline) is a candidate.
- "Allocate, don't mix granularities" and the two header/line anti-patterns are the most
  common real-world mistakes to expect when reviewing a Kimball-style warehouse design.
