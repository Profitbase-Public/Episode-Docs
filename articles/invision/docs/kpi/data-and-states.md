
# Data and states

A KPI card has a single data source and a single, shared set of states. This article explains how
the query result is mapped to components and how states drive the card's appearance.

<br/>

## The data source

Each card has **one** `DataSource` — a single SQL query that runs **once per card** and returns one
row. The same result feeds every component on the card. (If the query returns more than one row,
only the first row is used.)

```sql
SELECT SUM(Amount) AS VALUE, 'Revenue YTD' AS TEXT FROM FactSales
```

The query runs in the signed-in user's security context, with the current Workbook filter and
parameter context applied.

<br/>

## Reserved columns: VALUE and TEXT

Two column names are reserved:

- **`VALUE`** — the card's primary number. Components that show a number read this column.
- **`TEXT`** — an optional text label.

If your result set already contains columns named `VALUE` and `TEXT`, those are used directly.
Otherwise, the card resolves them by convention:

- **VALUE** resolves to the named column if one exists; if not, and the result has exactly one
  numeric column, that column is used; otherwise the first column is used.
- **TEXT** resolves to the named column if one exists; if not, and the result has exactly one
  non-numeric column (other than the value column), that column is used; otherwise the first column
  that is not the value column is used.

<br/>

## Mapping columns to components: ValueColumn and TextColumn

Every component can override which result column it reads:

<br/>

**ValueColumn**

> The name of the column this component should read as its value, instead of the resolved `VALUE`
> column. If the named column does not exist in the result set, the card reports an error.

<br/>

**TextColumn**

> The name of the column this component should read as its text, instead of the resolved `TEXT`
> column. If the named column does not exist in the result set, the card reports an error.

<br/>

This lets a single query feed several components from different columns. For example, a query
returning `Revenue`, `Margin`, and `Region` can drive one Metric bound to `Revenue` and another
bound to `Margin`:

```sql
SELECT 1200000 AS Revenue, 0.18 AS Margin, 'North' AS Region
```

```xml
<row>
  <Metric ValueColumn="Revenue" FormatString="N0" />
  <Metric ValueColumn="Margin" FormatString="P0" />
  <Text ValueColumn="Region" />
</row>
```

<br/>

## States: conditional color, image, and angle

A card has one shared list of **states**. A state is a condition plus the visual outcome to apply
when that condition is the first to match:

<br/>

**Condition**

> A boolean expression evaluated against the card's result row. Uses `DataTable.Select` syntax (the
> same expression syntax used elsewhere in InVision). The reserved columns `VALUE` and `TEXT` are
> available, and so is any other column in the result set, referenced by name.

<br/>

**Color**

> The color applied when this state matches. Any CSS color works — a named color (`green`, `red`,
> `yellow`) or a hex value (`#1a9c4f`). It drives the color of components such as Metric, Text,
> TrendArrow, StatusBlock, TrafficLight, TrendText, and TrendIndicator.

<br/>

**Image**

> An image applied when this state matches, used by the Image component and state-driven icons. The
> value is an **Image Library** reference of the form `@images/<image-name>.png` (see
> [Images and the Image Library](#images-and-the-image-library) below).

<br/>

**Angle**

> An integer rotation in degrees (for example `45`, `135`, `270`), applied to rotatable visuals such
> as the TrendArrow and TrendIndicator. Defaults to `0`.

<br/>

### First-match-wins evaluation

States are evaluated **in declared order**, and the **first** state whose condition matches the
card's result row wins. Its `Color`, `Image`, and `Angle` become the card's resolved state and are
shared by every component that consumes them. If no state matches, the card renders without a
state-driven color, image, or angle.

A malformed condition — or one that references a column that doesn't exist — is **skipped silently**
(it simply doesn't match) and evaluation continues with the next state. This means a typo in one
condition won't break the card; it just never matches.

```xml
<States>
  <State Color="green" Angle="45" Image="@images/trend-up.png">
    <Condition><![CDATA[VALUE > 25]]></Condition>
  </State>
  <State Color="red" Angle="135" Image="@images/trend-down.png">
    <Condition><![CDATA[VALUE <= 25]]></Condition>
  </State>
</States>
```

Conditions can reference other result columns too:

```xml
<State Color="orange">
  <Condition><![CDATA[VALUE < Target AND Region = 'North']]></Condition>
</State>
```

<br/>

## Images and the Image Library

The state `Image` value (and the Image component) uses an **Image Library** reference, not a raw
URL. The form is:

```
@images/<image-name>.png
```

`@images/` points at images stored in your solution's **Image Library** (web assets). At render
time the `@images` prefix is resolved to the deployed image path, so the named image must exist in
the Image Library for it to display. For example, `@images/trend-up.png` resolves to the
`trend-up.png` image in the library.

To use an image: add it to the Image Library, then reference it by name as `@images/<image-name>.png`
in the state's `Image` attribute.

> **Raw URLs are not supported** for KPI images. Use an `@images/<image-name>.png` reference. (This
> matches how the [Button](../forms/formschemas/controls/button.md) control's `Image` property
> works.)

<br/>

## See Also

- [Components](components.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Developer reference](developer-reference.md)
- [Filters](../filters.md)
