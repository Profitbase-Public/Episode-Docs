
# Creating KPI cards

This article walks through creating a KPI card in the Designer, setting its data source, placing it
on a Workbook page, and styling it. For the data model and component details, see
[Data and states](data-and-states.md) and [Components](components.md).

<br/>

## Create a KPI card in the Designer

KPI cards are solution objects. Create one from the Solution Explorer the same way you create other
objects, then open it to edit its configuration.

The card is edited as **XML** in the Designer — you author the card's `KpiConfiguration` directly:
its data source, its states, and the row of components. A new KPI card starts from a **default
template** that you edit to fit your needs — or you can replace it with one of the
[starter templates](#starter-templates) below. A simple card looks like this:

```xml
<KpiConfiguration>
  <DataSource><![CDATA[SELECT SUM(Amount) AS NumericValue, 'Revenue YTD' AS TextValue FROM FactSales]]></DataSource>
  <States>
    <State Color="green" Status="Complete"><Condition><![CDATA[NumericValue > 0]]></Condition></State>
    <State Color="red" Status="EarlyStages"><Condition><![CDATA[NumericValue <= 0]]></Condition></State>
  </States>
  <row>
    <Metric Size="heading1" Weight="bold" />
    <TrafficLight />
  </row>
</KpiConfiguration>
```

From here you:

- set the card's [data source](data-and-states.md),
- add the [states](data-and-states.md#states-conditional-color-image-and-status) that drive its
  color, icon image, and status,
- add the [components](components.md) that present the data.

For the complete XML format, see the [Developer reference](developer-reference.md).

<br/>

## Starter templates

The tool has no built-in design helpers, so the quickest way to build a card in one of the two
supported designs is to **copy one of the configurations below into your card and adapt it** —
swap the mock `DataSource` query for your own and rename the columns to match. Both templates use
inline `VALUES` data so they render immediately, before you connect a real query.

<br/>

### Default design

The default design (no `Theme`): a title, a metric with a sparkline chart, and a trend direction
arrow with a caption.

<br/>

![KPI card — default design](https://profitbasedocs.blob.core.windows.net/images/kpi-example-income.png)

<br/>

```xml
<KpiConfiguration>
  <DataSource><![CDATA[
SELECT
    'Income | Estimated' AS Title,
    (SELECT SUM(Amount) FROM (VALUES (1,10),(2,14),(3,9),(4,18),(5,22)) AS s(Id, Amount)) AS TotalIncome,
    4 AS Trend,
    '4.0 since last period' AS TrendSince;
]]></DataSource>
  <row>
    <Text TextColumn="Title" />
  </row>
  <row>
    <Metric ValueColumn="TotalIncome" />
    <Chart ValueColumn="Amount">
      <SeriesSource><![CDATA[
SELECT Amount FROM (VALUES (1,10),(2,14),(3,9),(4,18),(5,22)) AS t(Id, Amount) ORDER BY Id
]]></SeriesSource>
    </Chart>
  </row>
  <row>
    <TrendDirection ValueColumn="Trend" />
    <Text TextColumn="TrendSince" />
  </row>
</KpiConfiguration>
```

<br/>

### Status design (theme2)

The `theme2` design: a title, a metric with a traffic-light status icon and a trend badge, and a
muted caption. The `States` evaluate `TotalRevenue` and drive the traffic light's status icon.

<br/>

![KPI card — status design](https://profitbasedocs.blob.core.windows.net/images/kpi-example-revenue.png)

<br/>

```xml
<KpiConfiguration Theme="theme2">
  <DataSource><![CDATA[
SELECT
    'Revenue, NOK' AS Title,
    (SELECT SUM(Amount) * 1000 FROM (VALUES (1,10),(2,14),(3,9),(4,18),(5,22)) AS s(Id, Amount)) AS TotalRevenue,
    'increase' AS Trend,
    'Goal value: 1 500 000' AS TrendSince
FROM (
    SELECT
        MAX(CASE WHEN rn = 1 THEN Amount END) AS Latest,
        MAX(CASE WHEN rn = 2 THEN Amount END) AS Previous
    FROM (
        SELECT Amount, ROW_NUMBER() OVER (ORDER BY Id DESC) AS rn
        FROM (VALUES (1,10),(2,14),(3,9),(4,18),(5,22)) AS t(Id, Amount)
    ) ordered
    WHERE rn <= 2
) last2;
]]></DataSource>
  <States>
    <State Status="Complete">
      <Condition><![CDATA[TotalRevenue >= 50000]]></Condition>
    </State>
    <State Status="HalfWay">
      <Condition><![CDATA[TotalRevenue > 10000 AND TotalRevenue < 50000]]></Condition>
    </State>
    <State Status="EarlyStages">
      <Condition><![CDATA[TotalRevenue <= 10000]]></Condition>
    </State>
  </States>
  <row>
    <Text TextColumn="Title" />
  </row>
  <row>
    <Metric ValueColumn="TotalRevenue" />
    <TrafficLight />
    <TrendBadge ValueColumn="Trend" />
  </row>
  <row>
    <Text Color="#02080D80" TextColumn="TrendSince" />
  </row>
</KpiConfiguration>
```

<br/>

## Set the card's data source

A KPI card has **one data source** — a single SQL query that runs once and returns one row. Every
component on the card reads from this one result.

Edit the `DataSource` to return the value(s) you want to show. The query should return the metric in
a column named `NumericValue` and, optionally, a label in a column named `TextValue`:

```sql
SELECT SUM(Amount) AS NumericValue, 'Revenue YTD' AS TextValue FROM FactSales
```

You can return additional columns and reference them by name in state conditions, or point a
component at a specific column with `ValueColumn` / `TextColumn`. See
[Data and states](data-and-states.md) for how columns are resolved.

> Only the first row of the result set is used.

<br/>

## Choose a theme (optional)

A card can opt into a named type-scale **theme** with the optional `Theme` attribute on
`<KpiConfiguration>`. The theme name maps to a `.kpi-theme-*` CSS class that sets the card's font
sizes and weights. When omitted, the card uses the default theme.

```xml
<KpiConfiguration Theme="theme1">
  ...
</KpiConfiguration>
```

<br/>

## Add the card to a Workbook page

KPI cards are placed on Workbook pages as a page part. Add the KPI card page part to a page and
select the KPI card you created. The card renders live in the Workbook, running its query and
evaluating its states for the current user and filter context.

<br/>

> **Wire up Load Data.** Like every Workbook component, a KPI card does nothing until its
> [interaction model](../workbooks/programmingmodel/interactionmodel.md) is set up. The card will
> **not load** unless a **Load Data** action targets it from the Workbook page's events — the same
> page events used by Worksheets, Reports, and other components. If no Load Data action runs for the
> card, it renders empty.

<br/>

![A KPI card placed on a Workbook page in the Designer](https://profitbasedocs.blob.core.windows.net/images/kpi-card-designer.png)

<br/>

## Style the card

Card-level styling is set on the Workbook page part:

<br/>

**Border**

> Controls whether a border is drawn around the card. The border is enabled by default.

<br/>

**Border Radius**

> The corner radius of the card, in pixels. Defaults to `3`.

<br/>

**Padding**

> The inner spacing between the card edge and its content. Each side (left, top, right, bottom) can
> be set independently; the default is `24` left, `12` top, `24` right, `12` bottom.

<br/>

Within the card, each component's horizontal placement is controlled by its `HorizontalAlignment`
property (`left`, `center`, or `right`). See [Components](components.md) for per-component
properties.

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Components](components.md)
- [Developer reference](developer-reference.md)
- [Workbooks](../workbooks.md)
