
# Developer reference

This article documents the KPI card configuration format, how the data is resolved and served, the
data endpoint, and import/export. For the conceptual model, start with the
[KPI Cards overview](index.md).

<br/>

## Configuration XML

A KPI card is stored as a `KpiConfiguration`. It has an optional `Theme` attribute, one
`DataSource`, a shared list of `States`, and one or more `row` elements holding components.

```xml
<KpiConfiguration Theme="theme1">
  <DataSource><![CDATA[SELECT 50 AS NumericValue]]></DataSource>
  <States>
    <State Color="green" Status="Complete" Image="@images/trend-up.png"><Condition><![CDATA[NumericValue > 25]]></Condition></State>
    <State Color="red" Status="EarlyStages" Image="@images/trend-down.png"><Condition><![CDATA[NumericValue <= 25]]></Condition></State>
  </States>
  <row>
    <StatusBlock HorizontalAlignment="left" />
    <Image HorizontalAlignment="left" />
    <Metric HorizontalAlignment="left" Size="normal" Weight="normal" />
    <Text HorizontalAlignment="left" Size="normal" Weight="normal" />
    <TrendText HorizontalAlignment="left" />
    <TrendDirection HorizontalAlignment="left" />
    <TrendBadge HorizontalAlignment="left" />
    <TrafficLight />
    <Chart ValueColumn="Amount"><SeriesSource><![CDATA[SELECT Amount FROM FactSales ORDER BY PeriodId]]></SeriesSource></Chart>
  </row>
</KpiConfiguration>
```

The example above is a complete configuration — an optional `Theme`, one `DataSource`, two `States`,
and one `row` of components — showing every part of the format. A newly created card starts from a
default template; you edit these elements yourself.

### Elements

| Element | Meaning |
|---------|---------|
| `DataSource` | The single SQL query for the card. Runs once and returns one row. |
| `States` | A list of `State` elements, evaluated first-match-wins. |
| `State` | A `Condition` element plus `Color`, `Image`, and `Status` attributes. |
| `row` | A row of components. A configuration may contain one or more rows. |
| component elements | `Metric`, `Text`, `TrendText`, `Chart`, `TrendDirection`, `TrendBadge`, `TrafficLight`, `StatusBlock`, `Image`. The element name equals the component type. |

`KpiConfiguration` carries an optional `Theme` attribute (a type-scale theme name; omitted for the
default theme).

`State` carries `Condition` (a child element), and the attributes `Color`, `Image`, and `Status`.

Component attributes are described in [Components](components.md). Every component accepts
`HorizontalAlignment`. The data components read a column binding — `ValueColumn` (Metric,
TrendDirection, TrendBadge, and Chart's series column) or `TextColumn` (Text, TrendText) — while
StatusBlock, Image, and TrafficLight render from the resolved state and ignore the column bindings.
`Metric` adds `FormatString`, `Size`, `Weight`; `Text` adds `Size`, `Weight`, `Color`; `Chart` adds
a `SeriesSource` query element.

<br/>

## Column resolution

The card runs its query once and resolves columns from the single result row:

- **Value** — if a component sets `ValueColumn`, that named column is used (an error is raised if it
  doesn't exist). Otherwise, if there is exactly one numeric column it is used; otherwise the first
  column is used.
- **Text** — if a component sets `TextColumn`, that named column is used (error if missing).
  Otherwise, if there is exactly one non-numeric column (other than the value column) it is used;
  otherwise the first column that is not the value column is used.

Numeric detection covers the standard numeric types (`byte`, `int`, `long`, `decimal`, `double`,
`float`, and their variants).

<br/>

## State evaluation

States are resolved **once per card** against the result row:

1. A single-row table is built containing every result column, plus the reserved `NumericValue`
   (numeric) and `TextValue` (string) columns, which carry the resolved value/text. Columns already
   named `NumericValue`/`TextValue` in the result take precedence.
2. Each state's `Condition` is evaluated in order as a filter expression over that row (comparisons,
   `AND` / `OR` / `NOT`, `LIKE`, `IN (…)`).
3. The **first** state whose condition matches wins; its `Color` / `Image` / `Status` become the
   card's resolved state.
4. A condition that throws (malformed expression, unknown column) is caught and treated as
   non-matching, so evaluation continues to the next state.

<br/>

## Execution

The card's query runs **once per card**, the card-level state is resolved
once, and then the per-component data is built. Each component type takes the state properties
relevant to it (for example Metric, StatusBlock, and TrendText take the color; TrafficLight takes
the status; Image takes the image). The Chart component runs its own `SeriesSource` query in
addition to the card query, so a card with charts issues one query per chart on top of the card
query. On the client, `@images/...` image values are resolved against the Image Library before
rendering; a raw image URL is used as-is.

<br/>

## Import and export

A KPI card is a solution object (`KpiEntity`) whose `Configuration` is the `KpiConfiguration`
serialized as XML (see above). Cards travel with the solution through the normal
[package](../package.md) import/export, so no card-specific export step is required.

<br/>

## Limits and gotchas

- A KPI card only loads when a **Load Data** action targets it from the Workbook page's events (the
  same [interaction model](../workbooks/programmingmodel/interactionmodel.md) used by every other
  component). Without it, the card renders empty.
- The query returns **one row**; only the first row is used. Aggregate in SQL to produce a single
  value.
- A malformed or non-matching state condition is silently skipped — if no state matches, the card
  renders without a state color, image, or status.
- `Image` values are usually Image Library references (`@images/<image-name>.png`), which must exist
  in the library; a raw image URL is also accepted (any value not starting with `@images` is used
  as-is).
- Each `Chart` component runs its own `SeriesSource` query in addition to the card's `DataSource`,
  so a card with multiple charts issues multiple queries.
- A component's `ValueColumn` / `TextColumn` must name a column that the query actually returns, or
  the card reports an error.

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Components](components.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Packages](../package.md)
