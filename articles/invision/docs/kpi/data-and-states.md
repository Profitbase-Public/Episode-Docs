
# Data and states

A KPI card has a card-level data source and per-component states. This article explains how the
query result is mapped to components and how each component's states drive its appearance.

<br/>

## The data source

A card has a card-level `DataSource` — a SQL query that runs **once per card** and returns one row —
which is the **default** result for every component. (If the query returns more than one row, only
the first row is used.)

```sql
SELECT SUM(Amount) AS NumericValue, 'Revenue YTD' AS TextValue FROM FactSales
```

A component can override this by declaring its **own** `DataSource` child element; when it does, that
component resolves its value, text, and states from its own result instead of the card's. When a
component leaves `DataSource` blank, it falls back to the card-level result. (The `Chart` component
is the exception: its `DataSource` returns the whole series and has no card-level fallback — see
[Components](components.md#chart).)

The query runs with the current Workbook filter and parameter context applied.

<br/>

## The card's value and text

From the result row the card resolves a single **value** (the primary number) and an optional
**text** label, which the components display:

- **Value** — if a component sets `ValueColumn`, that named column is used; otherwise, if the result
  has exactly one numeric column that column is used, and failing that the first column is used.
- **Text** — if a component sets `TextColumn`, that named column is used; otherwise, if the result
  has exactly one non-numeric column (other than the value column) that column is used, and failing
  that the first column that is not the value column is used.

See [Mapping columns to components](#mapping-columns-to-components-valuecolumn-and-textcolumn) below
for the per-component overrides.

## Reserved columns in conditions: NumericValue and TextValue

State conditions can reference two reserved columns in addition to the result-set columns:

- **`NumericValue`** — the resolved value, as a number.
- **`TextValue`** — the resolved text.

These let a condition test the card's value/text without knowing the underlying column name. If your
result set already contains a column named `NumericValue` or `TextValue`, that column is used
directly instead of the resolved value/text.

<br/>

## Mapping columns to components: ValueColumn and TextColumn

Components that display card data can override which result column they read. The components that
show a number read `ValueColumn` (Metric, TrendDirection, TrendBadge); the components that show text
read `TextColumn` (Text, TrendText). The purely state-driven components — StatusBlock, Image,
TrafficLight, and CardBorder — are driven by their own states and ignore both.

<br/>

**ValueColumn**

> The name of the column this component should read as its value, instead of the resolved value.
> If the named column does not exist in the result set, the card reports an error.

<br/>

**TextColumn**

> The name of the column this component should read as its text, instead of the resolved text.
> If the named column does not exist in the result set, the card reports an error.

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

## States: conditional color, image, and status

Each component can carry its **own** `States` — a list nested inside the component element, which is
optional; a component with no states simply renders without a state-driven color, image, or status.
A state is a condition plus the visual outcome to apply when that condition matches. The outcome
values are written as `Property` elements (`<Property name="Color" value="green" />`) inside a
`Properties` element, a sibling of `Condition`:

<br/>

**Condition**

> A boolean **JavaScript** expression evaluated against the component's single result row. The
> result columns are available by name, including the reserved `NumericValue` and `TextValue`
> values, plus any other column in the result set. Use JavaScript operators — `>`, `>=`, `===`,
> `!==`, `&&`, `||`, `!` — for example `NumericValue > 25`. A state with no (or an empty)
> `Condition` always applies as a base (see [evaluation](#two-layered-evaluation) below).
>
> The `<Condition>` element accepts an optional **`Async`** attribute. With `Async="true"` the
> condition runs as an asynchronous function and may `await` HTTP calls — `HttpGet(url)`,
> `HttpPost(url, { data })`, `HttpPut(url, { data })`, `HttpDelete(url)` — to derive its result, for
> example `Async="true"` with `NumericValue > (await HttpGet('/api/target')).value`.
> Without the attribute (the default) the condition runs synchronously and has no network access.
> Either way the condition must still produce a **boolean** — it decides which state matches; it
> cannot set the displayed value, color, image, or status (those remain the `Property` values).

<br/>

**Color** *(`<Property name="Color" value="…" />`)*

> The color applied when this state matches. Any CSS color works — a named color (`green`, `red`,
> `yellow`) or a hex value (`#1a9c4f`). It colors the component that owns the state (for example
> Metric, StatusBlock, or TrendText); on a `CardBorder` component it drives the card's border color.

<br/>

**Image** *(`<Property name="Image" value="…" />`)*

> An image applied when this state matches, used by the Image component. The value is either an
> **Image Library** reference of the form `@images/<image-name>.png` or a raw image URL (see
> [Images and the Image Library](#images-and-the-image-library) below).

<br/>

**Status** *(`<Property name="Status" value="…" />`)*

> A status token applied when this state matches, consumed by the TrafficLight component to choose
> its status icon. Allowed values are `Complete`, `HalfWay`, and `EarlyStages`.

<br/>

### Two-layered evaluation

Each component resolves its own states on the client, against its own result row, in two layers:

- **Base layer** — every state with no (or an empty) `Condition` **always applies**. Their `Color`,
  `Image`, and `Status` form a base; when several condition-less states exist, the first-declared
  value wins for each property independently.
- **Conditional overlay** — the **first** state whose condition is truthy overlays the base: each
  property it sets overrides the base, and any property it leaves unset falls through to the base.

If no conditional state matches, the base alone applies; if there is no condition-less state either,
the component renders without a state-driven color, image, or status.

Conditional states are evaluated **in declared order and short-circuit at the first match** — so an
`Async="true"` condition only issues its HTTP request if no earlier conditional state has
already matched. If a condition fails — a malformed expression, an invalid column name, or a
failed/rejected async request — the error **surfaces to the user and halts the card's state
evaluation**, rather than passing unnoticed. Write conditions that reference valid columns and
produce a boolean.

```xml
<States>
  <State>
    <Condition><![CDATA[NumericValue > 25]]></Condition>
    <Properties>
      <Property name="Color" value="green" />
      <Property name="Status" value="Complete" />
      <Property name="Image" value="@images/trend-up.png" />
    </Properties>
  </State>
  <State>
    <Condition><![CDATA[NumericValue <= 25]]></Condition>
    <Properties>
      <Property name="Color" value="red" />
      <Property name="Status" value="EarlyStages" />
      <Property name="Image" value="@images/trend-down.png" />
    </Properties>
  </State>
</States>
```

A condition-less state can supply defaults that a matching state then overrides — here the base
color is grey, and the conditional state turns it orange when it matches:

```xml
<States>
  <State>
    <Properties>
      <Property name="Color" value="grey" />
    </Properties>
  </State>
  <State>
    <Condition><![CDATA[NumericValue < Target && Region === 'North']]></Condition>
    <Properties>
      <Property name="Color" value="orange" />
    </Properties>
  </State>
</States>
```

An `Async="true"` condition can compare the value against data fetched from an API at render time:

```xml
<States>
  <State>
    <Condition Async="true"><![CDATA[NumericValue >= (await HttpGet('/api/targets/current')).target]]></Condition>
    <Properties>
      <Property name="Color" value="green" />
    </Properties>
  </State>
</States>
```

<br/>

## Images and the Image Library

The state `Image` value (rendered by the Image component) is most often an **Image Library**
reference of the form:

```
@images/<image-name>.png
```

`@images/` points at images stored in your solution's **Image Library** (web assets). At render
time the `@images` prefix is resolved to the deployed image path, so the named image must exist in
the Image Library for it to display. For example, `@images/trend-up.png` resolves to the
`trend-up.png` image in the library.

To use a library image: add it to the Image Library, then reference it by name as
`@images/<image-name>.png` in the state's `Image` property.

> A **raw image URL** is also accepted. Any value that does not start with `@images` is used as-is,
> so you can point at an external or absolute image URL directly. Using an `@images/` reference is
> recommended where possible, since it keeps the image inside the solution and travels with it on
> import/export. (The `@images/` convention matches the
> [Button](../forms/formschemas/controls/button.md) control's `Image` property.)

<br/>

## See Also

- [Components](components.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Developer reference](developer-reference.md)
- [Filters](../filters.md)
