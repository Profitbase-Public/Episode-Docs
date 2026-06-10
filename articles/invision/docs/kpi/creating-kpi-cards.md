
# Creating KPI cards

This article walks through creating a KPI card in the Designer, setting its data source, placing it
on a Workbook page, and styling it. For the data model and component details, see
[Data and states](data-and-states.md) and [Components](components.md).

<br/>

## Create a KPI card in the Designer

KPI cards are solution objects. Create one from the Solution Explorer the same way you create other
objects, then open it to edit its configuration.

The card is edited as **XML** in the Designer — you author the card's `KpiConfiguration` directly:
its data source, its states, and the row of components. A new KPI card starts **empty** (no data
source, no states, no components), so you build it up yourself. A simple card looks like this:

```xml
<KpiConfiguration>
  <DataSource>SELECT SUM(Amount) AS VALUE, 'Revenue YTD' AS TEXT FROM FactSales</DataSource>
  <States>
    <State Color="green" Angle="45" Image="@images/trend-up.png"><Condition><![CDATA[VALUE > 0]]></Condition></State>
    <State Color="red" Angle="135" Image="@images/trend-down.png"><Condition><![CDATA[VALUE <= 0]]></Condition></State>
  </States>
  <row>
    <Metric Size="heading1" Weight="bold" />
    <TrendArrow />
  </row>
</KpiConfiguration>
```

From here you:

- set the card's [data source](data-and-states.md),
- add the [states](data-and-states.md#states-conditional-color-image-and-angle) that drive its
  color, icon, and angle,
- add the [components](components.md) that present the data.

For the complete XML format, see the [Developer reference](developer-reference.md).

## Set the card's data source

A KPI card has **one data source** — a single SQL query that runs once and returns one row. Every
component on the card reads from this one result.

Edit the `DataSource` to return the value(s) you want to show. The query should return the metric in
a column named `VALUE` and, optionally, a label in a column named `TEXT`:

```sql
SELECT SUM(Amount) AS VALUE, 'Revenue YTD' AS TEXT FROM FactSales
```

You can return additional columns and reference them by name in state conditions, or point a
component at a specific column with `ValueColumn` / `TextColumn`. See
[Data and states](data-and-states.md) for how columns are resolved.

> The query runs in the signed-in user's security context and returns a single row. Only the first
> row of the result set is used.

<br/>

## Add the card to a Workbook page

KPI cards are placed on Workbook pages as a page part. Add the KPI card page part to a page and
select the KPI card you created. The card renders live in the Workbook, running its query and
evaluating its states for the current user and filter context.

<br/>

![A KPI card on a Workbook page](https://profitbasedocs.blob.core.windows.net/images/kpi-card-workbook.png)

<br/>

## Style the card

Card-level styling is set on the Workbook page part:

<br/>

**Borders**

> Controls which of the card's borders are shown. Borders are enabled by default.

<br/>

**BorderRadius**

> The corner radius of the card, in pixels. Defaults to `5`.

<br/>

**Padding**

> The inner spacing between the card edge and its content. Each side (left, top, right, bottom) can
> be set independently; the default is `3`.

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
