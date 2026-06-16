# Logical Functions

## Boolean literals

```
TRUE()    -- returns true
FALSE()   -- returns false
NOT(<expr>)
```

## Conditional

```
IF(<condition>, <true-expr>, <false-expr>)
```

## Null handling

```
COALESCE(...args)                       -- first non-null argument
FIRSTNOTNULL(...args)                   -- alias for COALESCE

ISNULL(<expr>)                          -- true if null
ISNULL(<expr>, <replacement>)           -- returns replacement if null, else expr
ISNULLORZERO(<expr>)                    -- true if null or 0
ISNULLORZERO(<expr>, <replacement>)     -- returns replacement if null or 0, else expr
ISNULLOREMPTYSTR(<expr>)                -- true if null or ""

NZ(<expr>)                              -- returns 0 if null or empty, else expr
```

## Type checks

```
ISNUMBER(value)     -- true if the value is a numeric type
ISNUMERIC(value)    -- true if the value is, or can be converted to, a number
```

## Error handling

```
ISERROR(<expr>)                   -- true if expr throws an error
IFERROR(<expr>, <replacement>)    -- returns replacement if expr errors, else expr value
```
