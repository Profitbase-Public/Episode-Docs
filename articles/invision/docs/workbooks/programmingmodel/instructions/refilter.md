# Refilter

`Refilter(...)` can be called in `Recalc` actions in Worksheets, Tables or SQL Reports to specify whether the datagrid should be refiltered when the Recalc action completes.  

By default, the datagrid is refiltered when a `Recalc` action completes. However, in some situations, you may want to disable this behavior.
However, in some situations, you want to turn this behavior off.   

For example, a user may click a hyperlink to edit row details in a popup **while the row is scrolled into view**. When the user confirms the edits and `Recalc` runs, you may not want the filter to be reapplied, **as this could cause the row (or its siblings) to scroll out of view.**

<br/>

```javascript
Refilter(value: boolean);
```

<br/>

**Example**
```javascript
Refilter(false);

// Conditionally refilter based on a custom variable
Refilter(_state.RowsChanged > 0);
```