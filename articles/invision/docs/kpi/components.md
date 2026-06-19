
# Components

Components are the presentation elements that make up a KPI card. They are arranged in a row and
each renders some part of the card's data and its own resolved state. Each component can carry its
own optional `DataSource` and `States`: when a component leaves `DataSource` blank it falls back to
the card-level `DataSource` result, and each component resolves its own states independently (see
[Data and states](data-and-states.md)).

In the configuration XML, the element name equals the component name (for example `<Metric />`,
`<TrafficLight />`). A component's `DataSource` and `States` are child elements; all other settings
are attributes.

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

The purely state-driven components — **StatusBlock**, **Image**, **TrafficLight**, and
**CardBorder** — render from their own resolved state and ignore `ValueColumn` / `TextColumn`.

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

Displays the card's numeric value, formatted. Reads the resolved value (or its `ValueColumn`
override) and takes its color from its own resolved state.

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

Displays a text value. Reads the resolved text (or its `TextColumn` override) and renders it as
text. Unlike the other components, `Text` has its **own** `Color` attribute rather than taking a
state color.

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

Displays the card's text, colored by its own resolved state. Use it when you want the value
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

Renders a sparkline. The Chart's `DataSource` returns the whole series (one value per row) to plot,
rather than a single row. Unlike the scalar components, the Chart does **not** fall back to the
card-level `DataSource` — a blank `DataSource` yields no series — so each Chart on a card adds one
more query.

| Property | Allowed values |
|----------|----------------|
| `DataSource` | A SQL query (child element) returning the series rows to plot. No card-level fallback. |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` | the column read from the `DataSource` series result |

```xml
<Chart ValueColumn="Amount">
  <DataSource><![CDATA[SELECT Amount FROM FactSales ORDER BY PeriodId]]></DataSource>
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

A solid colored block whose background color comes from its own resolved state `Color`. Use it as a
bold, text-free status indicator.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |

```xml
<StatusBlock HorizontalAlignment="left">
  <States>
    <State>
      <Condition><![CDATA[Event.Data.NumericValue > 25]]></Condition>
      <Properties><Property name="Color" value="green" /></Properties>
    </State>
    <State>
      <Condition><![CDATA[Event.Data.NumericValue <= 25]]></Condition>
      <Properties><Property name="Color" value="red" /></Properties>
    </State>
  </States>
</StatusBlock>
```

<br/>

### Image

Displays the image from its own resolved state. The state's `Image` value is resolved through the
**Image Library** when it is an `@images/<image-name>.png` reference; a raw image URL is also
accepted.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |

The image to show is set on the component's **states**, via an `Image` property. The value is usually
an Image Library reference of the form `@images/<image-name>.png` (for example `@images/trend-up.png`):

```xml
<row>
  <Image HorizontalAlignment="center">
    <States>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue > 25]]></Condition>
        <Properties><Property name="Image" value="@images/trend-up.png" /></Properties>
      </State>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue <= 25]]></Condition>
        <Properties><Property name="Image" value="@images/trend-down.png" /></Properties>
      </State>
    </States>
  </Image>
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

A status icon (traffic-light style) chosen from its own resolved state `Status`. Use it for a
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
<row>
  <TrafficLight>
    <States>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue >= 100]]></Condition>
        <Properties><Property name="Status" value="Complete" /></Properties>
      </State>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue >= 50]]></Condition>
        <Properties><Property name="Status" value="HalfWay" /></Properties>
      </State>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue < 50]]></Condition>
        <Properties><Property name="Status" value="EarlyStages" /></Properties>
      </State>
    </States>
  </TrafficLight>
</row>
```

<br/>

### CardBorder

A non-visual component: it renders nothing of its own, but its resolved state `Color` sets the
card's **border color**. Add it to a row to drive the border from a condition. The page part's
**Border** setting still controls whether a border is drawn at all; `CardBorder` only changes its
color when a state resolves a `Color`.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` (has no visual effect, since the component renders nothing) |

```xml
<row>
  <CardBorder>
    <States>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue > 0]]></Condition>
        <Properties><Property name="Color" value="green" /></Properties>
      </State>
      <State>
        <Condition><![CDATA[Event.Data.NumericValue <= 0]]></Condition>
        <Properties><Property name="Color" value="red" /></Properties>
      </State>
    </States>
  </CardBorder>
</row>
```

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Developer reference](developer-reference.md)
