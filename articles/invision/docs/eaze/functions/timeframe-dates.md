# Time Frame Column Caption Helpers

These functions are available in column-caption expressions for Time Frame columns. They return values derived from the column's reference date.

All functions have `Start` / `End` variants. Numeric variants prefixed with `N` return `int32`. String variants apply default 2-digit zero-padding.

## Year

```
YearNum()  NYearNum()
YearNumStart()  NYearNumStart()  YearNumEnd()  NYearNumEnd()
```

## Month

```
MonthNum([digits])  NMonthNum()
MonthNumStart([digits])  NMonthNumStart()
MonthNumEnd([digits])    NMonthNumEnd()
MonthNameStart()  MonthNameEnd()
```

## Week

```
WeekNum([digits])  NWeekNum()
WeekNumStart([digits])  NWeekNumStart()
WeekNumEnd([digits])    NWeekNumEnd()
```

## Day of week

```
DayOfWeekName()  DayOfWeekNameStart()  DayOfWeekNameEnd()
DayOfWeekNum()   NDayOrWeekNum()
DayOfWeekNumStart()  NDayOrWeekNumStart()
DayOfWeekNumEnd()    NDayOrWeekNumEnd()
```

## Day of month

```
DayOfMonthNum([digits])  NDayOfMonthNum()
DayOfMonthNumStart([digits])  NDayOfMonthNumStart()
DayOfMonthNumEnd([digits])    NDayOfMonthNumEnd()
```

## Date range

```
DateStart()  DateEnd([format])
```

## Caption examples

Given a Time Frame Reference Date of 1 January 2021:

```
YearNum() + MonthNum()                              -- "202101"
YearNum() + " " + MonthNum()                        -- "2021 01"
YearNum() + "-P" + MonthNum()                       -- "2021-P01"
MonthNameStart().Substring(0,3) + ". " + YearNum()  -- "Jan. 2021"
```
