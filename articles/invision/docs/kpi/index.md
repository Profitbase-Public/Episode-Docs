
# KPI Cards

A **KPI card** is a compact, data-driven dashboard element that turns a query result into a visual
indicator — a metric, a sparkline, a trend indicator, a traffic light, a status block, an icon, or
any combination of these.

KPI cards are designed and configured in the **Designer**, and rendered for end users on
**Workbook** pages.

<br/>

![A KPI card rendered on a Workbook page](https://profitbasedocs.blob.core.windows.net/images/kpi-card-workbook.png)

<br/>

## How a KPI card works

A KPI card is built from three things:

- **One data source** — a single SQL query that runs once per card and returns one row.
- **One shared set of states** *(optional)* — conditional rules, evaluated top to bottom, where the
  first match decides the card's color, icon image, and status. A card may have no states at all.
- **A row of presentation components** — small visual elements (metric, text, sparkline, traffic
  light, etc.) that render the card's resolved data and state.

The query runs **once for the whole card**. Its result feeds every component on the card, and the
card's states are evaluated once to produce a single resolved color / image / status that the
components share. Components are **presentation elements only** — they don't carry their own states,
and (apart from the Chart sparkline, which runs its own small series query) they don't carry their
own query either. Each component simply chooses which result column to display (via `ValueColumn` /
`TextColumn`) and how to render it.

By default a component shows the card's resolved value or text; a component can override which
result column it reads (via `ValueColumn` / `TextColumn`), and any column in the result set — plus
the reserved `NumericValue` / `TextValue` columns — can be referenced by name in a state condition.
See [Data and states](data-and-states.md) for the full model.

<br/>

## In this section

- **[Creating KPI cards](creating-kpi-cards.md)** — create a card in the Designer, set its data
  source, add it to a Workbook page, and style it.
- **[Data and states](data-and-states.md)** — the single data source, column resolution, and the
  shared conditional states that drive the card's appearance.
- **[Components](components.md)** — reference for the nine presentation component types and the
  allowed values for every property.
- **[Developer reference](developer-reference.md)** — the XML configuration format, the data
  endpoint, import/export, and limits.

<br/>

## See Also

- [Workbooks](../workbooks.md)
- [Filters](../filters.md)
- [SQL Reports](../sqlreports.md)
