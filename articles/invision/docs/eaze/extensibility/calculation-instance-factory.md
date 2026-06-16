# Calculation Instance Factory

The Calculation Instance Factory lets you register a custom JavaScript calculation service that Eaze formulas can call as `this.<name>.<method>(...)`. Use it when standard Eaze isn't enough: stateful calculations, cross-spreadsheet data not loaded into the Workbook, or math backed by an external library.

## Configuration

Add a `<ComputeInstanceFactory>` block to the spreadsheet designer XML:

```xml
<ComputeInstanceFactory>
  <Instance Name="my.calculator" FactoryFunction="myLib.createCalculator">
    <FactoryFunctionArguments>
      <SqlScript Id="@Object[someSqlScript].Id" />
    </FactoryFunctionArguments>
  </Instance>
</ComputeInstanceFactory>
```

| Attribute | Description |
|---|---|
| `Instance/@Name` | Path on `this` your formulas use — e.g. `this.my.calculator` |
| `Instance/@FactoryFunction` | A function on the global `window` object that returns the service instance |
| `FactoryFunctionArguments/SqlScript` | SQL Scripts whose result sets are passed as positional arguments to the factory function (currently only data-returning scripts are supported) |

## Using the service in formulas

```
@Total[] = this.my.calculator.add(@P01[], @P02[]);
```

## When to reach for it

- Cross-spreadsheet data that is not loaded into the Workbook
- Conditional logic too complex for Eaze
- Math functions outside the Eaze standard library
- Stateful calculations that span multiple cell changes
- One shared calc service per spreadsheet

## Implementation options

**Option 1 — JavaScript Solution Object** for small, self-contained services.

**Option 2 — External build** (TypeScript / Node / Jasmine etc.) deployed to `/Scripts/plugins`. Declare load order in `/Scripts/plugins/plugins.json`:

```json
{ "files": ["3rdparty-lib1.js", "3rdparty-lib2.js", "mycalc-lib.js"] }
```

## Lighter-weight alternative (2025.5+)

From InVision 2025.5, [JavaScript code-behind](js-code-behind.md) lets you write JS inline directly in a Worksheet, Table, or SQL Report — without a separate Solution object or factory configuration.
