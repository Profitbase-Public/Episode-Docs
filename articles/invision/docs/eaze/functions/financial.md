# Financial Functions

## AMORLINC

Linear amortization of an asset for each accounting period (Excel-compatible).

```
AMORLINC(cost, date_purchased, first_period, salvage, period, rate [, basis])
```

| Parameter | Description |
|---|---|
| `cost` | Initial cost of the asset |
| `date_purchased` | Date of purchase |
| `first_period` | End date of the first period |
| `salvage` | Residual value at end of asset life |
| `period` | Period number to calculate depreciation for |
| `rate` | Depreciation rate |
| `basis` | Day-count basis (optional, default 0) |

## AMORLINCMTH

Variant of `AMORLINC` where `period` is supplied as a `Date` rather than a period number.

```
AMORLINCMTH(cost, date_purchased, first_period, salvage, period: Date, rate [, basis])
```
