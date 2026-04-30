# Functions

Functions in Form Schemas let you define reusable JavaScript logic that can be called from Event Handlers or from other Functions. This makes it possible to centralize business logic for a schema and avoid duplicating the same code across multiple event handlers.

## What functions are

A function configuration is converted directly into a JavaScript function and added as an instance method on the Form Runtime of the schema. Once defined, you can call it through the `functions` or `this` keyword from anywhere in the schema where script runs.

Functions support standard JavaScript syntax (whatever the browser supports natively — no TypeScript or Babel transpilation), accept comma-separated parameters, and can be marked `Async` to allow the use of `await`.

## When to use functions

Reach for a function when you find yourself writing the same logic in more than one Event Handler, or when an Event Handler is becoming long enough to be hard to read. Pulling the logic into a named function makes the schema easier to maintain and the intent clearer at the call site.

Functions are also the entry point for triggering Form Schema logic from outside the schema — for example, from a button click on the workbook. See [Calling functions from external events](callingfunctions.md) for that pattern.

## In this section

- [Functions](../functions.md) — full reference including properties, async functions, and detailed examples
- [Calling functions from external events](callingfunctions.md) — how to invoke a Form Schema function from other workbook components

## Videos

- [Form Schemas](../../../../videos/formschemas.md)
- [Functions](https://profitbasedocs.blob.core.windows.net/videos/Form%20Schema%20-%20Function.mp4)
- [Calling Functions from External Events](https://profitbasedocs.blob.core.windows.net/videos/Form%20schema%20-%20Calling%20Functions.mp4)