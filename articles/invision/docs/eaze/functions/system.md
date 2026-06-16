# System Functions

## ARRAY

Constructs an array from its arguments.

```
ARRAY(1, 4, "test")
```

Most statistical functions accept an `ARRAY(...)` in place of a comma-separated argument list.

## NEWID

Returns a new random 36-character GUID string (`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).

```
@Var[Name] = NEWID();
```

## Window

Returns the browser `window` object. Replaces the deprecated `ENVIRONMENT()`.

## EVAL

Dynamically evaluates a string expression at runtime. The string is parsed and executed as Eaze.

```
@Total[] = EVAL(@Formula[]);
-- If Formula = "@P01[] + @P02[]", this becomes: @Total[] = @P01[] + @P02[];
```

Useful for data-driven formulas where the calculation rule is stored in the data itself.

## JsonParse / JsonStringify

```
JsonParse(text)         -- parses a JSON string into a JavaScript value
JsonStringify(value)    -- serializes a JavaScript value to a JSON string
```

## ApiBase

Returns the base URL of the InVision web API. Useful for constructing API URLs in Workbook actions.

```
SetSrc(ApiBase() + "/api/db/objects/AssetLib/FileName?q={ID==\"WarningImg\"}&asFile=true");
```
