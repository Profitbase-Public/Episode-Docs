# Statistical Functions

```
AVERAGE(<range>)    AVERAGEA(<range>)
COUNT(<range>)      COUNTA(<range>)     COUNTBLANK(<range>)
MAX(<range>)        MAXA(<range>)
MIN(<range>)        MINA(<range>)
STDEV(<range>)      STDEVA(<range>)     STDEVP(<range>)     STDEVPA(<range>)
VAR(<range>)        VARA(<range>)       VARP(<range>)       VARPA(<range>)
```

`<range>` can be either a comma-separated argument list or an `ARRAY(...)`.

**`COUNT` vs `COUNTA`:** `COUNT` only counts numeric values. `COUNTA` counts all non-blank values including logical values (`true`/`false`). `COUNTBLANK` counts `null` / empty values.

The `A`-suffix variants (`AVERAGEA`, `MAXA`, `MINA`, `STDEVA`, `VARA`, …) include logical values in their calculation; the plain variants ignore them.
