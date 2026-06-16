
# Components

Components are the presentation elements that make up a KPI card. They are arranged in a row and
each renders some part of the card's resolved data and state. Components are **presentation only** —
they do not run their own query or carry their own states. They read from the card's single result
and the card's resolved state (see [Data and states](data-and-states.md)).

In the configuration XML, the element name equals the component name (for example `<Metric />`,
`<TrafficLight />`).

<br/>

## Properties shared by all components

Every component supports `HorizontalAlignment`:

<br/>

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right`. When omitted, the component flows at the start of the row (left) at its natural width; `left`, `center`, and `right` give it a cell that fills its share of the row and justifies its content accordingly. |

<br/>

Components that display card data also bind to a result column. Numeric components read
`ValueColumn`; text components read `TextColumn`:

<br/>

| Property | Allowed values |
|----------|----------------|
| `ValueColumn` | The result column read as the value, overriding the resolved value. Used by Metric, TrendDirection, and TrendBadge — and by Chart, to pick a column of its own series result. The named column must exist in the result set. |
| `TextColumn` | The result column read as the text, overriding the resolved text. Used by Text and TrendText. The named column must exist in the result set. |

<br/>

The purely state-driven components — **StatusBlock**, **Image**, and **TrafficLight** — render from
the card's resolved state and ignore `ValueColumn` / `TextColumn`.

<br/>

The text components **Metric** and **Text** additionally support:

<br/>

| Property | Allowed values |
|----------|----------------|
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `semibold`, `bold` |

<br/>

## Component reference

<br/>

### Metric

Displays the card's numeric value, formatted. Reads the card's resolved value (or its `ValueColumn`
override) and takes its color from the card's resolved state.

| Property | Allowed values |
|----------|----------------|
| `FormatString` | A .NET numeric format string, for example `N0`, `P0`, `#,##0.00`. Defaults to `#,##0`. |
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3`. Defaults to `heading1`. |
| `Weight` | `normal`, `semibold`, `bold`. Defaults to `semibold`. |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` | a column name from the card's result set |

```xml
<Metric ValueColumn="Revenue" FormatString="N0" Size="heading1" Weight="bold" HorizontalAlignment="center" />
```

<br/>

### Text

Displays a text value. Reads the card's resolved text (or its `TextColumn` override) and renders it as
text. Unlike the other components, `Text` has its **own** `Color` property rather than taking the
card's state color.

| Property | Allowed values |
|----------|----------------|
| `Color` | Any CSS color — a named color (`green`, `red`) or a hex value (`#333`). |
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3`. Defaults to `heading3`. |
| `Weight` | `normal`, `semibold`, `bold`. Defaults to `semibold`. |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `TextColumn` | a column name from the card's result set |

```xml
<Text TextColumn="Region" Color="#555" Size="sub" />
```

<br/>

### TrendText

Displays the card's text, colored by the card's resolved state. Use it when you want the value
itself to carry the state color. Its size and weight come from the card theme's defaults and are not
set on the component.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `TextColumn` | a column name from the card's result set |

```xml
<TrendText TextColumn="TrendSince" HorizontalAlignment="left" />
```

<br/>

### Chart

Renders a sparkline. Unlike the other components, the Chart runs its **own** SQL query — set in its
`SeriesSource` element — which returns the whole series (one value per row) to plot. This query is
separate from the card's `DataSource`, so each Chart on a card adds one more query.

| Property | Allowed values |
|----------|----------------|
| `SeriesSource` | A SQL query (child element) returning the series rows to plot. |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` | the column read from the `SeriesSource` result |

```xml
<Chart ValueColumn="Amount">
  <SeriesSource><![CDATA[SELECT Amount FROM FactSales ORDER BY PeriodId]]></SeriesSource>
</Chart>
```

<br/>

### TrendDirection

Shows an up or down arrow chosen purely from the **sign** of its value: a negative number shows the
down arrow, anything else (zero, positive, non-numeric) shows the up arrow. The arrow colors are
fixed in the icons and are not driven by state.

The two icons:

| Icon | Shown when |
|------|------------|
| ![Up trend arrow](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-trend-up.png) | the value is zero or positive |
| ![Down trend arrow](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-trend-down.png) | the value is negative |

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` | a column name from the card's result set |

```xml
<TrendDirection ValueColumn="Trend" HorizontalAlignment="left" />
```

<br/>

### TrendBadge

Shows a trend badge — increase, decrease, or "same". The bound column must contain one of the
tokens `increase`, `decrease`, or `same` (matched case-insensitively); any other or empty value
renders no icon. The data source owns the threshold logic that decides which token to emit.

The badge per token:

| Token | Icon |
|-------|------|
| `increase` | ![Increase badge](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-badge-increase.png) |
| `decrease` | ![Decrease badge](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-badge-decrease.png) |
| `same` | ![Same badge](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-badge-same.png) |

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` | a column name from the card's result set; its value must be `increase`, `decrease`, or `same` |

```xml
<TrendBadge ValueColumn="TrendToken" HorizontalAlignment="left" />
```

<br/>

### StatusBlock

A solid colored block whose background color comes from the card's resolved state. Use it as a bold,
text-free status indicator.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |

```xml
<StatusBlock HorizontalAlignment="left" />
```

<br/>

### Image

Displays the image from the card's resolved state. The state's `Image` value is resolved through the
**Image Library** when it is an `@images/<image-name>.png` reference; a raw image URL is also
accepted.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |

The image to show is set on the **state**, not the component. The value is usually an Image Library
reference of the form `@images/<image-name>.png` (for example `@images/trend-up.png`):

```xml
<States>
  <State Color="green" Image="@images/trend-up.png"><Condition><![CDATA[NumericValue > 25]]></Condition></State>
  <State Color="red" Image="@images/trend-down.png"><Condition><![CDATA[NumericValue <= 25]]></Condition></State>
</States>
<row>
  <Image HorizontalAlignment="center" />
</row>
```

`@images/` references an image stored in your solution's **Image Library** (web assets), which must
exist in the library to display. A raw image URL also works — any value that does not start with
`@images` is used as-is. Using an `@images/` reference is recommended so the image travels with the
solution. See [Data and states](data-and-states.md#images-and-the-image-library) for the full
explanation, and the [Button](../forms/formschemas/controls/button.md) control for the same
`@images/` convention elsewhere in InVision.

<br/>

### TrafficLight

A status icon (traffic-light style) chosen from the card's resolved state `Status`. Use it for a
compact status indicator. The icon is selected by the state's `Status` token — `Complete`,
`HalfWay`, or `EarlyStages`; if the resolved state has no (or an unrecognized) `Status`, no icon is
shown.

The icon per `Status` token:

| `Status` | Icon |
|----------|------|
| `Complete` | ![Complete status icon](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-status-complete.png) |
| `HalfWay` | ![HalfWay status icon](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-status-halfway.png) |
| `EarlyStages` | ![EarlyStages status icon](https://profitbasedocs.blob.core.windows.net/images/kpi-icon-status-earlystages.png) |

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |

```xml
<States>
  <State Status="Complete"><Condition><![CDATA[NumericValue >= 100]]></Condition></State>
  <State Status="HalfWay"><Condition><![CDATA[NumericValue >= 50]]></Condition></State>
  <State Status="EarlyStages"><Condition><![CDATA[NumericValue < 50]]></Condition></State>
</States>
<row>
  <TrafficLight />
</row>
```

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Developer reference](developer-reference.md)
