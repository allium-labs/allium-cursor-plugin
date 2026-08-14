---
name: explorer-visuals
description: Attach charts to Allium Explorer queries. Use when the user wants a chart, graph, pie, table, map, sankey, treemap, or value card on a query result.
---

# Explorer visuals

Each chart type has a JSON schema generated from the live API models, so it must
be fetched at use time rather than read from a copy in this plugin.

## Instructions

1. Call `get_skill(name="explorer-visuals")` for the chart catalog, the palettes,
   and the rules on picking a chart type.
2. For one chart type, call
   `get_skill(name="explorer-visuals", reference_title=<chart type>)` to get its
   exact JSON schema.
3. Create the visual with `create_explorer_visual`, binding it to the query whose
   fields you have already inspected.
