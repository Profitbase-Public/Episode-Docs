# Numerics API

#### convertToNumber(value: string)

Converts a string to a number or NaN

```javascript
const str = "123.4"
const num = this.app.lib.numerics.convertToNumber(str);
```

<br/>

#### isNumber(value: any)

Returns true / false whether the value is a number
```javascript
const str = "123.4"

// Returns false
const isNumber = this.app.lib.numerics.isNumber(str);

// Returns true
isNumber = this.app.lib.numerics.isNumber(123);
```

<br/>

#### isNumeric(value: any)
Returns true if the value can be converted to a number, otherwise false.

```javascript
const str = "123.4"

// Returns true
const isNumeric = this.app.lib.numerics.isNumeric(str);

// Returns true
isNumeric = this.app.lib.numerics.isNumeric(123);

// Returns false
isNumeric = this.app.lib.numerics.isNumeric("a");
```