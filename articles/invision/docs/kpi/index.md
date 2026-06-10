
# KPI Cards

A **KPI card** is a compact, data-driven dashboard element that turns a single query result into a
visual indicator — a metric, a trend arrow, a traffic light, a status block, an icon, or any
combination of these. KPI cards let solution builders surface a key number and its status at a
glance, without writing front-end code.

KPI cards are designed and configured in the **Designer**, and rendered for end users on
**Workbook** pages.

<br/>

![KPI card in the Designer](https://profitbasedocs.blob.core.windows.net/images/kpi-card-designer.png)

<br/>

## How a KPI card works

A KPI card is built from three things:

- **One data source** — a single SQL query that runs once per card and returns one row.
- **One shared set of states** — conditional rules, evaluated top to bottom, where the first match
  decides the card's color, icon image, and angle.
- **A row of presentation components** — small visual elements (metric, text, trend arrow, traffic
  light, etc.) that render the card's resolved data and state.

The query runs **once for the whole card**. Its result feeds every component on the card, and the
card's states are evaluated once to produce a single resolved color / image / angle that the
components share. Components are **presentation elements only** — they don't carry their own query
or their own states. Each component simply chooses which result column to display (via
`ValueColumn` / `TextColumn`) and how to render it.

The reserved result columns `VALUE` and `TEXT` are the defaults that components read; a component
can override which column it reads, and any column in the result set can be referenced by name in a
state condition. See [Data and states](data-and-states.md) for the full model.

<br/>

## In this section

- **[Creating KPI cards](creating-kpi-cards.md)** — create a card in the Designer, set its data
  source, add it to a Workbook page, and style it.
- **[Data and states](data-and-states.md)** — the single data source, column resolution, and the
  shared conditional states that drive the card's appearance.
- **[Components](components.md)** — reference for the eight presentation component types and the
  allowed values for every property.
- **[Developer reference](developer-reference.md)** — the XML configuration format, the data
  endpoint, import/export, and limits.

<br/>

## See Also

- [Workbooks](../workbooks.md)
- [Filters](../filters.md)
- [SQL Reports](../sqlreports.md)
