# Math / Trig Functions

```
ABS(<n>)                    -- absolute value
SIGN(<n>)                   -- -1, 0, or 1
ROUND(<n> [, digits])       -- round to digits decimal places (default 0)
CEILING(<n>)                -- round up to nearest integer
FLOOR(<n>)                  -- round down to nearest integer

SUM(...args)                -- sum of arguments (see note below)
MOD(<n>, <divisor>)         -- remainder

POW(<base>, <exp>)          -- base ^ exp
EXP(<n>)                    -- e ^ n
LN(<n>)                     -- natural logarithm
LOG(<n> [, base])           -- logarithm (default base 10)
LOG10(<n>)                  -- base-10 logarithm

SIN(<n>)  COS(<n>)  TAN(<n>)
ASIN(<n>) ACOS(<n>) ATAN(<n>) ATAN2(<y>, <x>)

SQRT(<n>)
PI()                        -- π
RAND()                      -- random number in [0, 1)
NUM_MIN()                   -- minimum representable Number value
```

> **2026.2:** `SUM` now handles `NaN` correctly. If your solution had a workaround for incorrect `NaN` propagation, it can be removed once the deployment is on 2026.2 or later.
