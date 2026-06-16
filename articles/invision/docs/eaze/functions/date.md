# Date Functions

```
DATE(year [, month, day, hours, min, sec, ms])   -- constructs a JS Date
DATEVALUE(year [, ...])                          -- alias for DATE
DATE(<expression>)                               -- converts expression to Date

TODATE(<expression> [, inputFormat])             -- parses string to Date
                                                 -- defaults to ISO 8601 if no format given
NOW()                                            -- current date and time

FORMATDATE(<date>, <format>)                     -- formats date using momentjs format strings
TZUTC(<date>)                                    -- converts date to UTC
TZLocal(<date>)                                  -- converts date to local timezone
```

## Format strings

`FORMATDATE` uses **momentjs** format strings:

```
FORMATDATE(NOW(), "YYYY-MM-DD")         -- "2026-06-16"
FORMATDATE(NOW(), "DD.MM.YYYY")         -- "16.06.2026"
```

`TODATE` defaults to ISO 8601 — pass an explicit format for other input patterns:

```
TODATE("16.06.2026", "DD.MM.YYYY")
```

## Time Frame column dates

To read the underlying date of a Time Frame column (`P01`, `P02`, …), use the `@Property[]` expression — not a date function:

```
@Property[P03.Date]    -- the date represented by column P03
```

For caption-building helpers (`YearNum`, `MonthNum`, `WeekNum`, …), see [Time Frame column helpers](timeframe-dates.md).
