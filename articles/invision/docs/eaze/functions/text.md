# Text Functions

```
CONCAT(...args)                           -- concatenate strings: CONCAT("a","b","c") => "abc"
SUBSTRING(input, start [, length])        -- 0-based start index
SPLIT(input, delimiter)                   -- returns string[]
LEFT(<s>, <n>)                            -- first n characters
RIGHT(<s>, <n>)                           -- last n characters
LEN(<s>)                                  -- character count
REPLACE(<s>, <old>, <new>)
LOWER(<s>)
UPPER(<s>)
TRIM(<s>)                                 -- removes leading and trailing whitespace
TOSTRING(<value>)
TOSTRING(<value>, <formatString>)         -- numeraljs format for numbers, momentjs for dates
TONUMBER(<s>)                             -- parses string to number; returns null if not convertible
NEWLINE                                   -- the newline character (not a function call)
```

## Format strings

`TOSTRING` uses **numeraljs** format strings for numbers and **momentjs** format strings for dates — not Excel format syntax.

```
TOSTRING(12500, "0,0")              -- "12,500"
TOSTRING(DATE(2016,1,1), "YYYYMMDD") -- "20160101"
```
