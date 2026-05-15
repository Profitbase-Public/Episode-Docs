# InVision — Eaze (Cell Calculations & Expression Language)

A consolidated reference for **Eaze**, the formula/expression language used by Profitbase InVision for cell calculations, grid rule mechanics, and many Workbook instruction parameters.

> _Source breadcrumbs:_ `articles/invision/docs/worksheets/calculations/*`, `articles/invision/docs/tables/calculations/*`, `articles/invision/docs/workbooks/programmingmodel/instructions/*`, plus changelog notes (2025.5 JS code-behind, 2026.2 `SUM`/`NaN` fix).

---

## 1. What Eaze is

Eaze is InVision's domain-specific formula language for **cell calculations in data grids** — Worksheets, Tables (input grids/Settings), and SQL Reports rendered as tables. It is not a general-purpose programming language; it is purpose-built for "address cells, write expressions, recalculate."

A typical Eaze formula:

```
@<target> = <expression>;
```

- **Left-hand side** — address of the cell(s) to write to (`@Column[row-selector]`).
- **Right-hand side** — an expression that produces the value to assign.
- Each statement terminates with `;`. Multiple statements run in order.

```
@Amount[] = Price * Qty;
```

Eaze also appears in **non-grid** surfaces — wherever the docs say *"boolean | Eaze expression"*. Many Workbook instructions (`EnableIf`, `ExecuteIf`, `Validate`, `ShowToastNotification`, `OpenBrowserWindow`, `ToggleCssClass`, `ConfigureInvocation`, …) accept Eaze.

### 1.1 Where Eaze is used

| Surface | Use |
|---|---|
| Worksheet **Calculations** | Cell formulas (`@Col[] = …`) on Data Store columns |
| Worksheet **Cell Validation** rules | Target + Statement expressions |
| Worksheet **Cell Read Only** rules | Target + Statement expressions |
| Worksheet **Row/Cell Styling** | `SETROWSTYLE` / `SETCELLSTYLE` with Eaze conditions |
| **Table** calculations (input-table object) | Same as Worksheet |
| **SQL Report** tabular output | Same Eaze grammar for derived columns |
| **Workbook action instructions** | Parameters typed `boolean \| Eaze expression` |

> **Not Eaze:** Filter / Caption / Header / Is-Hidden / Visibility expressions in the Workbook designer are **C# directive expressions** (`Directive(...)`, `Localize(...)`, `@Object[...]`, `@Context.ObjectAlias`). Different language, different evaluator — don't conflate them.

### 1.2 Eaze, JS code-behind, and the Calculation Instance Factory

Three extensibility paths coexist:

1. **Eaze** — canonical language for in-grid cell calculations and Workbook instruction conditions.
2. **Spreadsheet code-behind** *(added 2025.5)* — write JavaScript directly inside Worksheets, Tables, and SQL (tabular) Reports for calculations, business logic, and custom rows. Removes the need for a separate JS Solution object.
3. **Calculation Instance Factory** — register a custom JavaScript "calculation service" the spreadsheet calls from Eaze (e.g. `this.my.calculator.add(...)`). Use for stateful, cross-sheet, or library-backed math. See §10.

From 2025.5, **Workbook action handlers** can also be authored in JavaScript instead of Eaze.

#### Eaze → JavaScript action-handler cheatsheet

| Concept | Eaze | JavaScript handler |
|---|---|---|
| Event payload | `@Event.Data.customerId` | `Event.Data.customerId` |
| Workbook variable (legacy `@Var[…]`) | `@Var[My Var]` | `this.appVariables["@Var[My Var]"]` |
| Workbook variable (new) | `AppVariables.Factor` | `this.app.variables.get("Factor")` |
| Variable creation | Created on first assignment | Must be declared in the Variables dialog first |
| Async actions (`WebApi`, `Execute Web Function`, `Execute SQL Script`) | Synchronous call | `await Execute(...)` required |

```js
// Eaze
SetParamValue("@param1", @Event.Data.customerId);
_sqlScriptResponse = Execute("ExecSqlScriptFromWorkbook");

// JavaScript equivalent
SetParamValue("@param1", Event.Data.customerId);
_sqlScriptResponse = await Execute("ExecSqlScriptFromWorkbook");
```

Eaze remains the canonical, documented language for **in-grid** calculations.

---

## 2. Cell addressing

Eaze targets cells via column name plus an optional row-selector predicate.

### 2.1 Syntax

```
@<column>[]                  -- all cells in <column>
@<column>[<predicate>]       -- cells in <column> for rows matching <predicate>
```

The predicate runs per row against the grid's bound data set and must return `true`/`false`. Predicates may reference hard-coded literals, other columns from the current row context, Eaze functions, and logical operators.

```
@Amount[] = 10;                                       -- every Amount cell
@Amount[AccountID == "N-3000"] = 10;                  -- only matching rows
@Amount[AccountID == "2000" && ProjectID == "N-500"]
@Amount[AccountID == "2000" && LEFT(ProjectID,1) == "N"]
```

### 2.2 Cross-spreadsheet addressing

Reference cells in another spreadsheet in the same Workbook with the `!` separator. Use `LHS()` to correlate the current row to a row on the other sheet.

```
@Amount[] = @Qty[] * @Price list!Price[ProductID == LHS().ProductID];
```

Reads `Price` from the spreadsheet named `Price list`, matched to the current Left-Hand-Side row's `ProductID`.

### 2.3 Time Frame column dates

For Time Frame columns (`P01`, `P02`, …) read the column's date metadata via the reserved `@Property[]` expression:

```
@Property[P03.Date]      -- the actual date represented by column P03
```

### 2.4 Custom-row addressing

Custom rows added via `ADDROW*` need an `ObjectId` (commonly `-1`) to be addressable for styling, validation, or read-only rules:

```
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

---

## 3. Keywords & execution context

| Keyword | Meaning |
|---|---|
| `true` / `false` | Boolean literals |
| `null` | Null reference |
| `this` | Current execution context (see below) |

### `this` properties

| Property | Type | Description |
|---|---|---|
| `this.Rows` | `any[]` | All rows in the **current** spreadsheet |
| `this.DataSets` | `{[id:string]: any[]}` | All rows in **every** spreadsheet loaded in the Workbook, keyed by id |
| `this.Variables` | `{[id:string]: any}` | All Workbook variables |
| `this.LhsRow` | `object` | The row on the left-hand side of `=` currently being processed |
| `this.LhsColumn` | `string` | The column name on the left-hand side of `=` currently being processed |

`this` is also where Calculation-Instance-Factory services are exposed — e.g. `this.my.calculator.add(...)`. See §10.

---

## 4. Operators

### Arithmetic

| Op | Meaning |
|---|---|
| `+` | Add (also string concat when one operand is a string) |
| `-` | Subtract |
| `*` | Multiply |
| `/` | Divide |
| `%` | Modulus |

### Comparison

| Op | Meaning |
|---|---|
| `=` | **Assignment.** Only on the LHS of a formula (or `@Var[name] = value` in Execute Expression actions). Not equality. |
| `==` | Equals |
| `!=` | Not equals |
| `>` `>=` `<` `<=` | Ordering |

### Logical

| Op | Meaning |
|---|---|
| `&&` | Conditional AND (short-circuits) |
| `\|\|` | Conditional OR (short-circuits) |

### Unary & member access

| Op | Meaning |
|---|---|
| `+x` | Identity |
| `-x` | Numeric negation |
| `!x` | Logical negation |
| `x.y` | Member access |
| `x?.y` | Null-conditional member access (`null` if `x` is `null`) |

### Null-coalescing

| Op | Meaning |
|---|---|
| `??` | Left operand if not null, otherwise right operand |

```
null ?? "Hello"   // -> "Hello"
0    ?? "Hello"   // -> 0   (0 is not null)
```

---

## 5. Built-in function library

Eaze ships an Excel-flavored standard library. Names are case-conventional as in the docs (commonly UPPERCASE for the spreadsheet library; mixed-case for system/styling/row helpers).

### 5.1 Quick index

| Family | Functions |
|---|---|
| Logical | `TRUE`, `FALSE`, `NOT`, `IF`, `COALESCE`/`FIRSTNOTNULL`, `ISNULL`, `ISNULLORZERO`, `ISNULLOREMPTYSTR`, `ISNUMBER`, `ISNUMERIC`, `ISERROR`, `IFERROR`, `NZ` |
| Math / Trig | `ABS`, `SIGN`, `ROUND`, `CEILING`, `FLOOR`, `SUM`, `MOD`, `POW`, `EXP`, `LN`, `LOG`, `LOG10`, `SIN`/`COS`/`TAN`/`ASIN`/`ACOS`/`ATAN`/`ATAN2`, `SQRT`, `PI`, `RAND`, `NUM_MIN` |
| Statistical | `AVERAGE`/`AVERAGEA`, `COUNT`/`COUNTA`/`COUNTBLANK`, `MAX`/`MAXA`, `MIN`/`MINA`, `STDEV`/`STDEVA`/`STDEVP`/`STDEVPA`, `VAR`/`VARA`/`VARP`/`VARPA` |
| Text | `CONCAT`, `SUBSTRING`, `SPLIT`, `LEFT`, `RIGHT`, `LEN`, `REPLACE`, `LOWER`, `UPPER`, `TRIM`, `TOSTRING`, `TONUMBER`, `NEWLINE` |
| Date | `DATE`, `DATEVALUE`, `TODATE`, `NOW`, `FORMATDATE`, `TZUTC`, `TZLocal` + Time-Frame helpers (§5.5) |
| Financial | `AMORLINC`, `AMORLINCMTH` |
| System | `ARRAY`, `NEWID`, `Window`, `EVAL`, `JsonParse`, `JsonStringify`, `ApiBase` |
| Misc | `LHS`, `LHSVALUE`, `tmpl_foreach_operand` |
| Row collection | `ADDROWFIRST`/`ADDROWLAST`/`ADDROWAFTER`/`ADDROWBEFORE`, `IsSummaryRow`, `IsRowLocked`, `GetCallContextRow` |
| Styling | `SETROWSTYLE`, `SETCELLSTYLE` |

### 5.2 Logical

```
TRUE()                                 -- returns true
FALSE()                                -- returns false
NOT(<expr>)
IF(<cond>, <true-expr>, <false-expr>)

COALESCE(...args)                      -- first non-null arg
FIRSTNOTNULL(...args)                  -- alias for COALESCE

ISNULL(<expr>)                         -- true if null
ISNULL(<expr>, <replacement>)          -- replacement if null, else expr
ISNULLORZERO(<expr>)                   -- true if null or 0
ISNULLORZERO(<expr>, <replacement>)
ISNULLOREMPTYSTR(<expr>)               -- true if null or ""

ISNUMBER(value)                        -- true if numeric type
ISNUMERIC(value)                       -- true if value is or converts to a number

ISERROR(<expr>)                        -- true if expr errors
IFERROR(<expr>, <replacement>)         -- replacement if expr errors

NZ(<expr>)                             -- 0 if null/empty, else expr
```

### 5.3 Math / Trig

```
ABS, SIGN, ROUND, CEILING, FLOOR
SUM, MOD
POW, EXP, LN, LOG, LOG10
SIN, COS, TAN, ASIN, ACOS, ATAN, ATAN2
SQRT, PI()
RAND()
NUM_MIN()                              -- min value of the Number type
```

> **2026.2:** `SUM` now handles `NaN` correctly (bug fix). If your solution previously worked around this, the workaround can be removed once the deployment is on 2026.2 or later.

### 5.4 Statistical

```
AVERAGE, AVERAGEA
COUNT, COUNTA, COUNTBLANK
MAX, MAXA, MIN, MINA
STDEV, STDEVA, STDEVP, STDEVPA
VAR,   VARA,   VARP,   VARPA
```

`COUNT` only counts numbers; `COUNTA` counts all logical values; `COUNTBLANK` counts nulls. Most accept either a comma-separated argument list or an `ARRAY(...)`.

### 5.5 Text

```
CONCAT(...t)                                  -- "a"+"b"+"c" => "abc"
SUBSTRING(input, start [, length])
SPLIT(input, delimiter)                       -- returns string[]
LEFT, RIGHT, LEN, REPLACE
LOWER, UPPER
TRIM
TOSTRING(value)
TOSTRING(value, formatString)                 -- numeraljs for numbers, momentjs for dates
TONUMBER(s)                                   -- null if not convertible
NEWLINE                                       -- the newline character
```

```
TOSTRING(DATE(2016,1,1), "YYYYMMDD")          -- "20160101"
```

### 5.6 Date

```
DATE(year [,month,day,hours,min,sec,ms])      -- JS Date
DATEVALUE(year [,...])                        -- same
DATE(expression)
TODATE(expression [, inputFormat])            -- defaults to ISO 8601
NOW()
FORMATDATE(date, format)                      -- momentjs format strings
TZUTC(date)
TZLocal(date)
```

#### Time-Frame column date helpers

Used in column-caption expressions for Time Frame columns. All have `Start`/`End` variants plus an `N`-prefixed `int32` variant where applicable. The string forms apply default 2-digit formatting.

```
DateStart() / DateEnd() / DateEnd(format)
YearNum() / NYearNum() / YearNumStart() / NYearNumStart() / YearNumEnd() / NYearNumEnd()
MonthNum() / NMonthNum() / MonthNum(digits)
MonthNumStart() / NMonthNumStart() / MonthNumStart(digits)
MonthNumEnd()   / NMonthNumEnd()   / MonthNumEnd(digits)
MonthNameStart() / MonthNameEnd()
WeekNum() / NWeekNum() / WeekNumStart() / NWeekNumStart() / WeekNumStart(digits)
WeekNumEnd() / NWeekNumEnd() / WeekNumEnd(digits)
DayOfWeekName() / DayOfWeekNameStart() / DayOfWeekNameEnd()
DayOfWeekNum()  / NDayOrWeekNum() / DayOfWeekNumStart() / NDayOrWeekNumStart() / DayOfWeekNumEnd() / NDayOrWeekNumEnd()
DayOfMonthNum() / NDayOfMonthNum() / DayOfMonthNumStart() / NDayOfMonthNumStart() / DayOfMonthNumStart(digits)
DayOfMonthNumEnd() / NDayOfMonthNumEnd() / DayOfMonthNumEnd(digits)
```

Caption examples for a Time Frame Reference Date of Jan 1 2021:

```
YearNum() + MonthNum()                              -- "202101"
YearNum() + " " + MonthNum()                        -- "2021 01"
YearNum() + "-P" + MonthNum()                       -- "2021-P01"
MonthNameStart().Substring(0,3) + ". " + YearNum()  -- "Jan. 2021"
```

### 5.7 Financial

```
AMORLINC(cost, date_purchased, first_period, salvage, period, rate [, basis])
AMORLINCMTH(cost, date_purchased, first_period, salvage, period:Date, rate [, basis])
```

### 5.8 System

```
ARRAY(...args)                  -- ARRAY(1, 4, "test")
NEWID()                         -- 36-char GUID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Window()                        -- browser window object (replaced ENVIRONMENT())

EVAL(expression : string)       -- dynamically evaluates a string expression
JsonParse(text)                 -- JSON string -> JS value
JsonStringify(value)            -- JS value -> JSON string

ApiBase()                       -- base URL of the InVision web API
```

`EVAL` is powerful for data-driven calculations:

```
@Total[] = EVAL(@Formula[]);
-- If Formula = "@P01[] + @P02[]", this becomes @Total[] = @P01[] + @P02[];
```

`ApiBase()` is handy for building API URLs in Workbook actions:

```
SetSrc(ApiBase() + "/api/db/objects/AssetLib/FileName?q={ID==\"WarningImg\"}&asFile=true");
```

### 5.9 LHS helpers and AST templating

```
LHS()                                        -- the object on the left side of '='
LHS().<prop>                                 -- access a property of the LHS row
LHSVALUE(propName : string)                  -- value of LHS row's property by name
                                             -- LHSVALUE("AccountID") => LHS account id

tmpl_foreach_operand(formula, operandTemplate)
```

`tmpl_foreach_operand` builds an AST by substituting operand tokens into a template, then feeding it to `EVAL`. Substitution tokens:

- `#LHSCOL` — name of the LHS column
- `#OP` — current operand token from the formula parameter

```
-- If Formula = "L01 + L02" and LHS column is "Total":
tmpl_foreach_operand(Formula, "#LHSCOL[LineID == \"#OP\"]")
-- AST equivalent of: @Total[LineID == "L01"] + @Total[LineID == "L02"]

@Total[LineID == "L03"] = EVAL(tmpl_foreach_operand(Formula, "#LHSCOL[LineID == \"#OP\"]"));
-- Becomes:              @Total[LineID == "L03"] = @Total[LineID == "L01"] + @Total[LineID == "L02"];
```

### 5.10 Row collection

These mutate the bound grid data set in place.

```
ADDROWFIRST(jsonObject)                      -- add at top
ADDROWLAST(jsonObject)                       -- add at bottom
ADDROWAFTER(addressExpr, jsonObject)         -- after first row matching addressExpr
ADDROWBEFORE(addressExpr, jsonObject)        -- before first row matching addressExpr

IsSummaryRow(row)                            -- true if row is the summary row
IsRowLocked(row)                             -- true if row is locked
GetCallContextRow()                          -- the row Eaze is currently evaluating
```

Custom rows need an `ObjectId` (commonly `-1`) to be addressable:

```
ADDROWFIRST({"ObjectId": -1, "AccountID": "F100", "ProductID": "P-001", "Total": 0, "P01": -3000});
```

### 5.11 Cell & row styling

```
SETROWSTYLE(rowAddressExpr, cssClass : string)
SETCELLSTYLE(cellAddressExpr, columnName : string, cssClass : string)
```

```
SETROWSTYLE(ProductID == "XYZ", "ProductGroup1");
SETROWSTYLE(STARTSWITH(ProductID, "F"), "StyleA");
SETCELLSTYLE(Total < 1000, "Total", "RedCell");

-- Style the summary row
SetRowStyle(IsSummaryRow(GetCallContextRow()), "css-class");
```

---

## 6. Workbook variables (`AppVariables` and `@Var[…]`)

Workbook variables enter Eaze through `AppVariables`. Two notations coexist; pick by name shape.

### 6.1 Reading

```
AppVariables.Factor                  -- property access (standard names only)
AppVariables.getValue("Factor")      -- function access
```

Use `getValue("…")` for any name containing whitespace or `@Var[…]` syntax — JS property access can't reach those:

```
AppVariables.getValue("@Var[My Variable]");
```

### 6.2 `@Var[…]` notation

`@Var[name]` is the form for Workbook variables whose names contain whitespace — typical for variables auto-generated for Data Flow, Form Element, and Script components, where InVision wraps the name. For your own variables, prefer standard Eaze names.

```
-- assign (only in Execute Expression actions)
@Var[My Factor] = 12.4;
@Var[Last Row Clicked] = @Event.Data;
@Var[Last Row Clicked].ProductID = @Event.Data.ProduktID;

-- read
CONCAT("Selected: ", @Var[Last Row Clicked].ProductName);
OpenBrowserWindow(CONCAT("http://api.company.com/appendix/", @Event.Data.ProsjektID));

-- clear
@Var[Name] = null;

-- generate a unique id
@Var[Name] = NEWID();
```

### 6.3 Recalc after a variable change

Modifying an AppVariable from outside the grid (e.g. from a Form Element's `SelectionChanged` event) does **not** automatically re-run grid formulas that reference it. Trigger the Worksheet/Setting `RecalcAction` explicitly.

---

## 7. `@Event` and `Filter()` references

### 7.1 `@Event`

Inside an action's instructions list, `@Event` exposes the event payload. Common shape (e.g. for context-menu events on TableView):

```
@Event.Data.<Field>          -- fields on the clicked row
@Event.Sender.ColumnName     -- column that was clicked
@Event.Sender.ActionName     -- menu action name
@Event.Selection.Rows        -- selected rows array
@Event.Selection.Cells       -- selected cells
```

In JavaScript action handlers, use `Event` (no `@`). See §1.2.

### 7.2 `Filter()`

Filter values are accessible through `Filter(group, name)`. Its result exposes `SelectedValue` with `Id`, `ColumnName`, `Description`, `IsLeaf`, `Level`.

```
SetParamValue("@MyParam", Filter("MainFilters", "Department").SelectedValue.Id);
_departmentName = Filter("Page1Filters", "Department").SelectedValue.Description;
EnableIf(Filter("Filters", "Department").SelectedValue.IsLeaf);
```

---

## 8. Validation, read-only, and styling rules

All three Worksheet rule families share the same shape: **Name**, **Disabled**, **Target** (cell-address Eaze expression), **Statement** (boolean Eaze expression).

### 8.1 Cell Validation rules

- **Rule Type:** `ValidationError` (red border, blocks save when run from a Workbook context) or `ValidationWarning` (yellow border).
- **Target:** cells to flag if Statement returns `false`.
- **Statement:** returns `true` if the cell is valid, `false` otherwise.

`Validate(terminateOnValidationFailed : boolean | Eaze expression)` runs the rules from a `SaveData` action. Validation errors do **not** automatically prevent subsequent actions — they only stop the save for that Worksheet. To gate a whole flow on validation, build an Action Group that runs `Validate` for all Worksheets first.

### 8.2 Conditional Cell Read-Only rules

```
Target:    @Qty[]
Statement: @Amount[] > 100
-- Qty becomes read-only on rows where Amount > 100

Target:    @Qty[]
Statement: @PBRowIdentity[] != -1
-- Qty becomes read-only on rows already persisted to the database
```

### 8.3 Row/Cell styling

Defined as Eaze statements run on data load / recalc; see §5.11.

---

## 9. Custom queries and column compatibility

When a Worksheet uses a **Custom Query** for loading data, the returned columns must match the original Data Store layout by name, type, and count. To support cell comments, include any `_PBComment` columns from the Data Store. None of this changes Eaze syntax — but it changes which columns are addressable.

---

## 10. Calculation Instance Factory (JS escape hatch)

Configure via XML in the spreadsheet designer when standard Eaze isn't enough:

```xml
<ComputeInstanceFactory>
  <Instance Name="my.calculator" FactoryFunction="myLib.createCalculator">
    <FactoryFunctionArguments>
      <SqlScript Id="@Object[someSqlScript].Id" />
    </FactoryFunctionArguments>
  </Instance>
</ComputeInstanceFactory>
```

- `Instance/@Name` — name on `this` your formulas use (e.g. `this.my.calculator.add(...)`).
- `Instance/@FactoryFunction` — global (`window`) function returning the calc-service instance.
- `FactoryFunctionArguments/SqlScript` — SQL Scripts whose result sets are passed as factory args, in declaration order (currently only data-returning SQL Scripts).

**When to reach for it:** cross-spreadsheet data not loaded into the Workbook, conditionals too complex for Eaze, math outside the Eaze library, stateful calculation across cell changes, one calc service per spreadsheet.

**Implementation options:**

1. **JavaScript Solution Object** — for small/simple services.
2. Build externally (TypeScript / VS Code / Node / Jasmine etc.) and deploy to `/Scripts/plugins`. List load order in `/Scripts/plugins/plugins.json`:
   ```json
   { "files": ["3rdparty-lib1.js", "3rdparty-lib2.js", "mycalc-lib.js"] }
   ```

From 2025.5 onward, **spreadsheet code-behind** (inline JS directly in Worksheets/Tables/SQL Reports) is the lighter-weight alternative for the same scenarios — no separate Solution object needed.

---

## 11. Idiomatic patterns

### Derived column
```
@Amount[] = Price * Qty;
```

### Slice
```
@Amount[ProductID == "XP-5000"] =
    @Price[ProductID == "XP-5000"] * @Qty[ProductID == "XP-5000"];
```

### Cross-sheet lookup
```
@Amount[] = @Qty[] * @Price list!Price[ProductID == LHS().ProductID];
```

### Workbook variable
```
@Amount[] = @TotalForce[] * AppVariables.Factor;
```

### Conditional read-only by saved state
```
Target:    @Qty[]
Statement: @PBRowIdentity[] != -1
```

### Style the summary row
```
SetRowStyle(IsSummaryRow(GetCallContextRow()), "summary-css");
```

### Add a custom row
```
ADDROWFIRST({"ObjectId": -1, "AccountID": "F100", "ProductID": "P-001", "Total": 0, "P01": -3000});
SETROWSTYLE(ObjectId == -1, "CustomRowStyle");
```

### Dynamic formula via `EVAL` + template
```
@Total[LineID == "L03"] = EVAL(tmpl_foreach_operand(Formula, "#LHSCOL[LineID == \"#OP\"]"));
```

### Recalc after a Form input changed an AppVariable
In the Form Element's `SelectionChanged` event, invoke the Worksheet/Setting's `RecalcAction`.

### Workbook instructions taking Eaze conditions
```
EnableIf(Filter("Filters", "Department").SelectedValue.Level >= 2 && _accountId == "A3000");
ExecuteIf(IsHostPageActive() && HasFilterChanged());
ToggleCssClass("highlight", X == 250);
ShowToastNotification("Update", CONCAT(_count, " items were updated"));
OpenBrowserWindow(CONCAT("http://mysite/api/invoices?id=", SelectedOrder.OrderId));
```

---

## 12. Gotchas & rules of thumb

- **`=` is assignment.** Use `==` for equality. `=` only appears on the LHS of a formula or in `@Var[name] = value` inside Execute Expression actions.
- **`@Var[…]` is only assignable in Execute Expression actions.** Reading is unrestricted.
- **AppVariable changes don't auto-trigger recalc.** Call `RecalcAction`.
- **`COUNT` ignores non-numbers; `COUNTA` counts logical values.** Pick the right one.
- **`TOSTRING(value, format)`** uses numeraljs for numbers and momentjs for dates — not Excel format strings.
- **`TODATE(string)` defaults to ISO 8601** — pass an explicit input format for anything else.
- **For Time Frame columns, prefer `@Property[ColName.Date]`** over parsing column metadata yourself.
- **Custom rows need `ObjectId`** (often `-1`) to be addressable for styling, validation, or read-only rules.
- **Cross-spreadsheet addressing requires `LHS()`** to correlate the current LHS row with rows on the other sheet.
- **In JS action handlers, Eaze `@Event` / `@Var[…]` become `Event` / `this.appVariables["@Var[…]"]`** and async actions need `await`. See §1.2.
- **Filter / Caption / Header / Is-Hidden expressions in Workbook designer fields are C# directives, not Eaze** — they use `Directive(...)`, `Localize(...)`, `@Object[...]`, `@Context.ObjectAlias`.
- **`SUM` and `NaN`:** fixed in 2026.2. Older releases mis-handled `NaN` propagation; remove any workaround once on 2026.2+.

---

## 13. Doc map

All paths relative to `articles/invision/docs/` in the Profitbase docs.

| Topic | Source |
|---|---|
| Eaze overview | `worksheets/calculations/eaze.md`, `tables/calculations/eaze.md` |
| Cell addressing | `worksheets/calculations/celladressing.md`, `tables/calculations/celladressing.md` |
| Keywords (`this`, etc.) | `worksheets/calculations/keywords.md`, `tables/calculations/keywords.md` |
| Arithmetic / Comparison / Logical / Other binary ops | `worksheets/calculations/{arithmeticop,comparisonop,logicalop,otherbop}.md` (and `tables/calculations/` siblings) |
| Logical / Math / Statistical / Text / Date / Financial / System / Misc functions | `worksheets/calculations/{logicalfunc,mathfunc,statisticfunc,textfunc,datefunc,financialfunc,sysfunc,miscfunc}.md` (and `tables/calculations/` siblings) |
| Time Frame column date helpers | `tables/colcaptions.md`, `worksheets/colcaptions.md` |
| Row collection (`ADDROW*`, `IsSummaryRow`, `IsRowLocked`) | `worksheets/calculations/rowcollfunc.md`, `tables/calculations/rowcollfunc.md` |
| Row/Cell styling functions | `worksheets/calculations/cellnrowstylfunc.md`, `tables/calculations/cellnrowstylfunc.md` |
| AppVariables | `worksheets/calculations/appvariables.md`, `tables/calculations/appvariables.md` |
| `@Property[col.Date]` | `worksheets/calculations/usingdate.md`, `tables/calculations/usingdate.md` |
| Calculation Instance Factory | `worksheets/calculations/calcinstfactory.md`, `tables/calculations/calcinstfactory.md` |
| Cell Validation | `worksheets/cellvalidation.md` |
| Conditional Cell Read-Only | `worksheets/conditcellreadonly.md` |
| Row Styling (page-level) | `worksheets/rowstyling.md`, `tables/rowstyling.md` |
| Workbook `@Var[…]` | `workbooks/programmingmodel/instructions/var.md` |
| Workbook instructions accepting Eaze | `workbooks/programmingmodel/instructions/*.md` (e.g. `enableif.md`, `executeif.md`, `cssclass.md`, `showtoastnotification.md`, `openbrowserwindow.md`, `validate.md`, `configinvocation.md`) |
| JS code-behind / Eaze → JS migration (2025.5) | `changelog/changelog25_5.md` |
| `SUM`/`NaN` fix (2026.2) | `changelog/changelog26_2.md` |
