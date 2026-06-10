
# Developer reference

This article documents the KPI card configuration format, how the data is resolved and served, the
data endpoint, and import/export. For the conceptual model, start with the
[KPI Cards overview](index.md).

<br/>

## Configuration XML

A KPI card is stored as a `KpiConfiguration`. It has one `DataSource`, a shared list of `States`,
and one or more `row` elements holding components.

```xml
<KpiConfiguration>
  <DataSource>SELECT 50 AS VALUE</DataSource>
  <States>
    <State Color="green" Angle="45" Image="@images/trend-up.png"><Condition><![CDATA[VALUE > 25]]></Condition></State>
    <State Color="red" Angle="135" Image="@images/trend-down.png"><Condition><![CDATA[VALUE <= 25]]></Condition></State>
  </States>
  <row>
    <TrendArrow HorizontalAlignment="left" />
    <StatusBlock HorizontalAlignment="left" />
    <Image HorizontalAlignment="left" />
    <Metric HorizontalAlignment="left" Size="normal" Weight="normal" />
    <Text HorizontalAlignment="left" Size="normal" Weight="normal" />
    <TrafficLight />
    <TrendIndicator HorizontalAlignment="left" />
  </row>
</KpiConfiguration>
```

The example above is a complete configuration — one `DataSource`, two `States`, and one `row` of
components — showing every part of the format. A newly created card starts empty; you add these
elements yourself.

### Elements

| Element | Meaning |
|---------|---------|
| `DataSource` | The single SQL query for the card. Runs once and returns one row. |
| `States` | A list of `State` elements, evaluated first-match-wins. |
| `State` | A `Condition` element plus `Color`, `Image`, and `Angle` attributes. |
| `row` | A row of components. A configuration may contain one or more rows. |
| component elements | `Metric`, `Text`, `TrendArrow`, `StatusBlock`, `Image`, `TrafficLight`, `TrendText`, `TrendIndicator`. The element name equals the component type. |

`State` carries `Condition` (a child element), and the attributes `Color`, `Image`, and `Angle`
(integer, omitted when `0`). There is no `Type` attribute on a state.

Component attributes are described in [Components](components.md). All components share
`HorizontalAlignment`, `ValueColumn`, and `TextColumn`; `Metric` adds `FormatString`, `Size`,
`Weight`; `Text` adds `Size`, `Weight`, `Color`; `TrendIndicator` adds `ArrowPosition`.

<br/>

## Column resolution

The card runs its query once and resolves columns from the single result row:

- **VALUE** — if a component sets `ValueColumn`, that named column is used (an error is raised if it
  doesn't exist). Otherwise: a result column literally named `VALUE` is used; failing that, if there
  is exactly one numeric column it is used; otherwise the first column is used.
- **TEXT** — if a component sets `TextColumn`, that named column is used (error if missing).
  Otherwise: a result column literally named `TEXT` is used; failing that, if there is exactly one
  non-numeric column (other than the value column) it is used; otherwise the first column that is
  not the value column is used.

Numeric detection covers the standard numeric types (`byte`, `int`, `long`, `decimal`, `double`,
`float`, and their variants).

<br/>

## State evaluation

States are resolved **once per card** against the result row:

1. A single-row table is built containing every result column, plus the reserved `VALUE` (numeric)
   and `TEXT` (string) columns. Columns already named `VALUE`/`TEXT` in the result take precedence.
2. Each state's `Condition` is evaluated in order using `DataTable.Select`-style expression syntax.
3. The **first** state whose condition matches wins; its `Color` / `Image` / `Angle` become the
   card's resolved state.
4. A condition that throws (malformed expression, unknown column) is caught and treated as
   non-matching, so evaluation continues to the next state.

<br/>

## Execution

The card's query runs **once per card** (with a 60-second timeout), the card-level state is resolved
once, and then the per-component data is built. Each component type takes the state properties
relevant to it (for example StatusBlock and TrafficLight take the color; TrendArrow and
TrendIndicator take color and angle; Image takes the image). On the client, `@images/...` image
values are resolved against the Image Library before rendering.

<br/>

## Import and export

A KPI card is a solution object (`KpiEntity`) whose `Configuration` is the `KpiConfiguration`
serialized as XML (see above). Cards travel with the solution through the normal
[package](../package.md) import/export, so no card-specific export step is required.

<br/>

## Limits and gotchas

- The query returns **one row**; only the first row is used. Aggregate in SQL to produce a single
  value.
- The query runs in the signed-in user's security context, so data-access permissions apply.
- A malformed or non-matching state condition is silently skipped — if no state matches, the card
  renders without a state color, image, or angle.
- `Image` values must be Image Library references (`@images/<image-name>.png`); **raw URLs are not
  supported**, and the referenced image must exist in the library.
- A component's `ValueColumn` / `TextColumn` must name a column that the query actually returns, or
  the card reports an error.

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Components](components.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Packages](../package.md)
