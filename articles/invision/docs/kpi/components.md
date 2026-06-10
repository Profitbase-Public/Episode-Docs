
# Components

Components are the presentation elements that make up a KPI card. They are arranged in a row and
each renders some part of the card's resolved data and state. Components are **presentation only** —
they do not run their own query or carry their own states. They read from the card's single result
and the card's resolved state (see [Data and states](data-and-states.md)).

In the configuration XML, the element name equals the component name (for example `<Metric />`,
`<TrafficLight />`).

<br/>

## Properties shared by all components

Every component supports these properties:

<br/>

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right`. Omit to use the default alignment. |
| `ValueColumn` | The result column this component reads as its value, overriding the resolved `VALUE` column. The named column must exist in the result set. |
| `TextColumn` | The result column this component reads as its text, overriding the resolved `TEXT` column. The named column must exist in the result set. |

<br/>

The text components (**Metric**, **Text**, **TrendText**, **TrendIndicator**) additionally support:

<br/>

| Property | Allowed values |
|----------|----------------|
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `bold` |

<br/>

## Component reference

<br/>

### Metric

Displays the card's numeric value, formatted. Reads the `VALUE` column (or its `ValueColumn`
override) and takes its color from the card's resolved state.

| Property | Allowed values |
|----------|----------------|
| `FormatString` | A .NET numeric format string, for example `N0`, `P0`, `#,##0.00`. |
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `bold` |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<Metric ValueColumn="Revenue" FormatString="N0" Size="heading1" Weight="bold" HorizontalAlignment="center" />
```

<br/>

### Text

Displays a text value. Reads the `VALUE` column (or its `ValueColumn` override) and renders it as
text. Unlike the other components, `Text` has its **own** `Color` property rather than taking the
card's state color.

| Property | Allowed values |
|----------|----------------|
| `Color` | Any CSS color — a named color (`green`, `red`) or a hex value (`#333`). |
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `bold` |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<Text ValueColumn="Region" Color="#555" Size="sub" />
```

<br/>

### TrendArrow

A directional arrow icon whose **color** comes from the card's resolved state and whose **rotation**
comes from the state's `Angle`. Use states to point the arrow up, down, or sideways and to color it.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<TrendArrow HorizontalAlignment="left" />
```

<br/>

### StatusBlock

A solid colored block whose background color comes from the card's resolved state. Use it as a bold,
text-free status indicator.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<StatusBlock HorizontalAlignment="left" />
```

<br/>

### Image

Displays the image from the card's resolved state. The state's `Image` value is resolved through the
**Image Library**, so it must be an `@images/<image-name>.png` reference.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

The image to show is set on the **state**, not the component. The value is an Image Library
reference of the form `@images/<image-name>.png` (for example `@images/trend-up.png`):

```xml
<States>
  <State Color="green" Image="@images/trend-up.png"><Condition><![CDATA[VALUE > 25]]></Condition></State>
  <State Color="red" Image="@images/trend-down.png"><Condition><![CDATA[VALUE <= 25]]></Condition></State>
</States>
<row>
  <Image HorizontalAlignment="center" />
</row>
```

`@images/` references an image stored in your solution's **Image Library** (web assets). The named
image must exist in the library for it to display. **Raw URLs are not supported** — use an
`@images/<image-name>.png` reference. See
[Data and states](data-and-states.md#images-and-the-image-library) for the full explanation, and the
[Button](../forms/formschemas/controls/button.md) control for the same `@images/` convention
elsewhere in InVision.

<br/>

### TrafficLight

A colored indicator (traffic-light style) whose color comes from the card's resolved state. Use it
for a compact green / amber / red status.

| Property | Allowed values |
|----------|----------------|
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<TrafficLight />
```

<br/>

### TrendText

Displays the card's value as text, colored by the card's resolved state. Use it when you want the
number itself to carry the state color.

| Property | Allowed values |
|----------|----------------|
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `bold` |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<TrendText Size="heading2" Weight="bold" />
```

<br/>

### TrendIndicator

Combines a trend arrow with a value/label. The arrow's color and rotation come from the card's
resolved state (`Color` and `Angle`); the text shows the `TEXT` value if present, otherwise the
`VALUE`. The arrow can be placed on either side of the text.

| Property | Allowed values |
|----------|----------------|
| `ArrowPosition` | `left`, `right` (`right` places the arrow after the text) |
| `Size` | `sub`, `normal`, `heading1`, `heading2`, `heading3` |
| `Weight` | `normal`, `bold` |
| `HorizontalAlignment` | `left`, `center`, `right` |
| `ValueColumn` / `TextColumn` | a column name from the card's result set |

```xml
<TrendIndicator ArrowPosition="right" HorizontalAlignment="left" />
```

<br/>

## See Also

- [Data and states](data-and-states.md)
- [Creating KPI cards](creating-kpi-cards.md)
- [Developer reference](developer-reference.md)
