---
name: dashboard-design
description: Build and update Allium Explorer dashboards with set_dashboard. Use when the user wants a dashboard created, edited, or laid out from Explorer queries.
---

# Dashboard design

The element schemas, layout minimums, and rendering rules are generated from the
live API models, so they must be fetched at use time rather than read from a
copy in this plugin.

## Instructions

1. Call `get_skill(name="dashboard-design")` and follow it. It is the source of
   truth for the `set_dashboard` tree-CRUD model, the element catalog, the
   12-column layout rules, and the design rules.
2. For a single element type, call
   `get_skill(name="dashboard-design", reference_title=<config.type>)` to get its
   exact config schema. Use the field names and casing it shows.
3. Call `search_terminal` first unless the request is clearly unrelated to public
   Terminal dashboards; if a relevant one exists, inspect it with
   `get_terminal_results` and use it as the baseline.
4. Build top-down with the IDs the server returns: dashboard > page > section >
   element. Then `read_dashboard` to verify every element bound its query.

Queries must exist before the dashboard is built — bind to `query_id`, never
hardcode a number into markdown.
