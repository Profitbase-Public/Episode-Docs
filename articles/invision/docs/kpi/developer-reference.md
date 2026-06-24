
# Developer reference

This article documents the KPI card configuration format, how the data is resolved and served, the
data endpoint, and import/export. For the conceptual model, start with the
[KPI Cards overview](index.md).

<br/>

## Configuration XML

A KPI card is stored as a `KpiConfiguration`. It has an optional `ClassName` attribute, an optional
card-level `DataSource` (the fallback query for components that don't declare their own), an optional
card-level `CardBorder` element (a sibling of `DataSource`, driving the card's border color), and one
or more `Row` elements holding components. States are not declared at the top level — each component
(and the card-level `CardBorder`) carries its own optional `DataSource` and `States`.

```xml
<KpiConfiguration ClassName="kpi-theme-theme1">
  <DataSource><![CDATA[SELECT 50 AS NumericValue]]></DataSource>
  <CardBorder>
    <States>
      <State>
        <Condition><![CDATA[NumericValue > 25]]></Condition>
        <Properties>
          <Property Name="Color" Value="green" />
        </Properties>
      </State>
    </States>
  </CardBorder>
  <Row>
    <StatusBlock HorizontalAlignment="left">
      <States>
        <State>
          <Condition><![CDATA[NumericValue > 25]]></Condition>
          <Properties>
            <Property Name="Color" Value="green" />
          </Properties>
        </State>
        <State>
          <Condition><![CDATA[NumericValue <= 25]]></Condition>
          <Properties>
            <Property Name="Color" Value="red" />
          </Properties>
        </State>
      </States>
    </StatusBlock>
    <Image HorizontalAlignment="left">
      <States>
        <State>
          <Condition><![CDATA[NumericValue > 25]]></Condition>
          <Properties>
            <Property Name="Image" Value="@images/trend-up.png" />
          </Properties>
        </State>
        <State>
          <Condition><![CDATA[NumericValue <= 25]]></Condition>
          <Properties>
            <Property Name="Image" Value="@images/trend-down.png" />
          </Properties>
        </State>
      </States>
    </Image>
    <Metric HorizontalAlignment="left" Size="normal" Weight="normal" />
    <Text HorizontalAlignment="left" Size="normal" Weight="normal" />
    <TrendText HorizontalAlignment="left" />
    <TrendDirection HorizontalAlignment="left" />
    <TrendBadge HorizontalAlignment="left" />
    <StatusIndicator>
      <States>
        <State>
          <Condition><![CDATA[NumericValue > 25]]></Condition>
          <Properties>
            <Property Name="Status" Value="Complete" />
          </Properties>
        </State>
      </States>
    </StatusIndicator>
    <Chart ValueColumn="Amount"><DataSource><![CDATA[SELECT Amount FROM FactSales ORDER BY PeriodId]]></DataSource></Chart>
  </Row>
</KpiConfiguration>
```

The example above is a complete configuration — an optional `ClassName`, a card-level `DataSource`, a
card-level `CardBorder`, and one `Row` of components, several of which carry their own `States` —
showing every part of the format. A newly created card starts from a default template; you edit these
elements yourself.

### Elements

| Element | Meaning |
|---------|---------|
| `DataSource` | A SQL query. At card level it is the fallback query for components that don't declare their own; inside a component it is that component's own query. A scalar query runs once and returns one row. |
| `States` | A per-component list of `State` elements, resolved on the client (see [State resolution](#state-resolution)). Declared inside the component element, not at the top level. |
| `State` | A `Condition` child element plus a `Properties` element. A state with no (or empty) `Condition` always applies as a base. |
| `Condition` | The state's boolean JavaScript expression. Optional `Async="true"` attribute makes it an awaited async function with HTTP access (default `false`). |
| `Properties` | Holds the state's `Property` elements. Only emitted when at least one property is set. |
| `Property` | A `Name`/`Value` pair. `Name` is one of `Color`, `Image`, `Status` (matched case-insensitively). |
| `Row` | A row of components. A configuration may contain one or more rows. |
| `CardBorder` | An optional card-level element (a sibling of `DataSource`, not a row component). Carries its own `States`; the resolved `Color` sets the card's border color. See [CardBorder](#cardborder). |
| component elements | `Metric`, `Text`, `TrendText`, `Chart`, `TrendDirection`, `TrendBadge`, `StatusIndicator`, `StatusBlock`, `Image`. The element name equals the component type. |

`KpiConfiguration` carries an optional `ClassName` attribute (a complete CSS class name applied to
the card; omitted for the default `kpi-theme-theme1`).

`State` carries a `Condition` child element and a `Properties` child element; `Color`, `Image`, and
`Status` are `Property` elements (`<Property Name="Color" Value="green" />`).

Component attributes are described in [Components](components.md). Every component accepts
`HorizontalAlignment`. The data components read a column binding — `ValueColumn` (Metric,
TrendDirection, TrendBadge, and Chart's series column) or `TextColumn` (Text, TrendText) — while
StatusBlock, Image, and StatusIndicator are driven by their own `States` and ignore the
column bindings. `Metric` adds `FormatString`, `Size`, `Weight`; `Text` adds `Size`, `Weight`,
`Color`. Every component may also carry its own `DataSource` child element; for `Chart` the
`DataSource` returns the series rows to plot (and has no card-level fallback).

<br/>

### CardBorder

`CardBorder` is an optional **card-level** element — a direct child of `KpiConfiguration` and a
sibling of `DataSource`, declared before the `Row` elements. It is **not** a row component: a
`<CardBorder>` placed inside a `<Row>` is ignored.

It renders nothing of its own. Like a component, it carries an optional `DataSource` and a `States`
block; its resolved state `Color` sets the card's **border color**. The page part's **Border**
setting still controls whether a border is drawn at all (see
[Creating KPI cards](creating-kpi-cards.md#style-the-card)); `CardBorder` only changes the border
color when a state resolves a `Color`. `HorizontalAlignment` may be set but has no visual effect,
since the element renders nothing.

<br/>

## Column resolution

Each component resolves its value and text from its own result set — its own `DataSource` result if
it declares one, otherwise the card-level `DataSource` result. The card-level query runs once and is
the shared default; the resolution rules are the same in both cases:

- **Value** — if a component sets `ValueColumn`, that named column is used (an error is raised if it
  doesn't exist). Otherwise, if there is exactly one numeric column it is used; otherwise the first
  column is used.
- **Text** — if a component sets `TextColumn`, that named column is used (error if missing).
  Otherwise, if there is exactly one non-numeric column (other than the value column) it is used;
  otherwise the first column that is not the value column is used.

Numeric detection covers the standard numeric types (`byte`, `int`, `long`, `decimal`, `double`,
`float`, and their variants).

<br/>

## State resolution

Each component resolves its **own** states on the client, against its own result row — there is no
single card-level resolved state. Conditions are evaluated as **JavaScript** expressions; the
result columns are available by name, including the reserved `NumericValue` (numeric) and
`TextValue` (string) values (for example `NumericValue > 25`). Columns already named
`NumericValue`/`TextValue` in the result take precedence.

Resolution is two-layered:

1. **Base layer** — every state with no (or an empty) `Condition` **always applies**. Their
   `Color` / `Image` / `Status` form a base; when several condition-less states exist, the
   first-declared value wins for each property independently.
2. **Conditional overlay** — the **first** state whose `Condition` evaluates truthy overlays the
   base: each property it sets overrides the base, and any property it leaves unset falls through to
   the base.
3. If no conditional state matches, the base alone applies. If there is no condition-less state
   either, the component renders without a state-driven color, image, or status.
4. A condition that throws — a malformed expression, an invalid column name, or a rejected async
   request — surfaces as an error and halts evaluation, rather than being treated as a non-match.

Conditional states are evaluated **in declared order and short-circuit at the first match**. By
default a condition runs synchronously with no network access. Setting `Async="true"` on the
`<Condition>` builds it as an awaited async function over an HTTP-capable library, so it may `await`
`HttpGet(url)`, `HttpPost(url, { data })`, `HttpPut(url, { data })`, or `HttpDelete(url)` to derive
its result. Because evaluation short-circuits, an async state only issues its request if no earlier
conditional state matched; a rejected or throwing request surfaces as an error and halts evaluation.
In all cases a successful condition's outcome is just the **boolean** match — it never sets the rendered value,
color, image, or status (those stay the static `Property` values).

<br/>

## Execution

The card-level query runs **once per card** as the shared default. Each component then builds its
own data: a component with its own `DataSource` runs an additional query (each `Chart` always runs
its own series `DataSource`, with no card fallback), and every component resolves its own states on
the client. There is no single card-level resolved state — each component applies the color, image,
or status from its own resolved state (the card-level `CardBorder` element's resolved color drives the
card border). On the client, `@images/...` image values are resolved against the Image Library before
rendering; a raw image URL is used as-is.

<br/>

While a card is loading for the **first time**, the whole card shows a **loading skeleton** — a
shimmer placeholder in the card's footprint — instead of an empty box. The skeleton covers the entire
initial load: the card-level `DataSource` query and the client-side state resolution, including any
`Async="true"` condition's awaited HTTP calls. It clears once that load completes. It is a first-load
placeholder, not a per-component or per-condition indicator, and does not reappear when states
re-evaluate on later reloads.

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
- A scalar query returns **one row**; only the first row is used. Aggregate in SQL to produce a
  single value.
- A non-matching state condition simply doesn't apply; if a component has no matching conditional
  state (and no condition-less base state), it renders without a state-driven color, image, or status.
- A malformed condition or an invalid column name raises an error that surfaces to the user, rather
  than being silently ignored.
- `Image` values are usually Image Library references (`@images/<image-name>.png`), which must exist
  in the library; a raw image URL is also accepted (any value not starting with `@images` is used
  as-is).
- Each component with its own `DataSource` runs an extra query, and each `Chart` always runs its own
  series `DataSource` (in addition to the card's `DataSource`), so a card with several such
  components issues multiple queries.
- A component's `ValueColumn` / `TextColumn` must name a column that the query actually returns, or
  the card reports an error.
- `Async="true"` conditions issue HTTP requests during client-side render. Use them sparingly and
  rely on the first-match short-circuit to bound the calls; a slow or failing endpoint just makes
  that state non-matching (it never blocks the rest of the card).

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Components](components.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Packages](../package.md)
