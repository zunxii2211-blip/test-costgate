# CostGate — One PR Demo

This is a single clean PR demo for a video.

The existing CostGate database has:
- 300,000 rows in `orders`
- a primary-key index on `orders.id`
- intentionally NO index on `orders.customer_id`

## Baseline

Use:

`baseline/queries/order_lookup.sql`

Commit it to `master`.

Suggested commit message:

`add customer order lookup`

## PR

Create a branch and replace the query with:

`pr/queries/order_lookup.sql`

Suggested branch:

`demo/order-lookup-regression`

Suggested commit message:

`introduce expensive order lookup`

Then open one PR against `master`.

## What the PR demonstrates

Before:

`WHERE id = 12345`

PostgreSQL can use the primary-key index.

After:

`WHERE customer_id = 12345`

There is no index on `customer_id`, so PostgreSQL has to scan the `orders` table.

This gives CostGate a very clear before/after performance regression to detect and report as an estimated database compute-cost increase.

Do NOT run `terraform apply`. No AWS infrastructure deployment is needed for this demo.
