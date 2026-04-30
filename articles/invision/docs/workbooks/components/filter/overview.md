# Filter

Filters are components that define the data context for other parts of a Solution — including Worksheets, Reports, Workflows, Data Flows, and Settings. Once a Filter is defined, any data context-aware component can use it by binding to one of its properties.

## What a filter is

A Filter is a reference to a table resource (Dimension, Fact, Setting, Data Store, or View) plus a configuration that describes how to read data from that resource. You add filters to a Workbook by dragging them from the Toolbox in the Workbook designer, then setting their properties and wiring them up through Workbook events and actions.

## What you can do with filters

Filters expose a number of options for controlling how they appear and behave:

- **Show and hide** — toggle filter visibility through the `Is Hidden` property, an `Is Hidden Expression`, or dynamically via `Show()`, `Hide()`, `ShowIf()`, and `HideIf()` instructions
- **Custom headings** — set a static `Header`, use a `Header Expression` for localized headings (commonly with the `Localize(...)` function), or change the heading at runtime via `SetHeader()`, `SetHeaderSuffix()`, and `ResetHeader()`
- **Slicing** — restrict a hierarchical filter to a specific subset of leaf-level members, useful when one filter should only show items relevant to the current data context

## In this section

- [Filter](../filter.md) — full reference, including how to add filters to a Workbook, configure their properties, wire up events and actions, and control visibility and headings
- [Filter slicing](filterslicing.md) — restrict a filter to a specific subset of items by calling `SetLeafLevelConstraints(...)` in the Load Data action

## See also

- [More about Filters](../../../filters/index.md)
- [Filtering Tables](../../../tables/filters.md)
- [Filtering Worksheets](../../../worksheets/filters.md)

## Videos

- [Filters](../../../../videos/filters.md)
- [Workbooks](../../../../videos/workbooks.md)