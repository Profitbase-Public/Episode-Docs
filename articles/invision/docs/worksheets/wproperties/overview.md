# Worksheet properties

Worksheet properties control how a Worksheet behaves and what end users can do with it. These properties cover the visual appearance of the grid, the rules for adding and editing rows, sorting and filtering options, and the context menu actions available on right-click.

## What you can configure

Properties on a Worksheet fall into a few groups:

- **Appearance** — toggle grid lines, the row selector, and the summary row to control how the grid looks
- **Editing rules** — control row addition through Add Row Settings, decide whether row locking is exposed to users, and constrain the input level required for editing
- **User interaction** — enable or disable column sorting and inline filtering for end users
- **Context menu** — choose which right-click actions are available (Select All, Export to Excel, Copy/Paste, Insert row, Distribute value, Factor multiplication, Reverse distribution, Edit comments, View changes, and more)

## Conditional behavior

Most context menu options support three states: always available (`Yes`), hidden (`No`), or conditional based on a JavaScript function. A conditional option lets you check the selected rows, the right-clicked column, or any Workbook variable, and decide at runtime whether the option should be enabled. This is useful for restricting actions like Distribute value to specific account types, or hiding View changes for rows where Change Tracking is not relevant.

## In this section

- [Worksheet properties](../wproperties.md) — full reference for all available properties, including detailed descriptions of each context menu action
- [Context menu options](contextmenuoptions.md) — how to write conditions that control when context menu options are available, including examples and the structure of the `args` parameter

## See also

- [Custom action conditions](../columnproperties/customactionconditions.md)