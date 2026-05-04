# Built-in (Flow) overview

The **Built-in** category contains the core building blocks of every Flow — the actions that handle variables, control flow, loops, sub-flows, error handling, and extensibility. Unlike the integration-specific categories that connect Flow to external systems, these actions are the fabric of the Flow language itself: they decide what runs, in what order, with which data, and what happens when things go wrong.

<br/>

## Variables and data

Declare individual variables with [Declare variable](./declare-variable.md), or several at once with [Declare variables](./declare-variables.md). Assign new values during execution using [Set variable](./set-variable.md), and adjust numeric values in place with [Increment value](./increment-value.md) and [Decrement value](./decrement-value.md). [Convert](./convert.md) translates between data types — string to number or date, JSON to a custom type, byte arrays, streams, and collections — and [Unescape unicode characters](./unescape-unicode-characters.md) decodes escaped sequences such as `s\u00f8r` into `sør`.

<br/>

## Decisions and error handling

Branch a flow on a condition with [If](./if.md) for two outcomes, or [If-Else](./if-else.md) for multiple. Wrap a section of a flow in [Try-Catch](./try-catch.md) to handle exceptions — for example, sending a notification email when something fails. Use [Throw exception](./throw-exception.md) to raise your own error with a custom message, or [Rethrow exception](./rethrow-exception.md) inside a catch block to surface the original error after handling it.

<br/>

## Loops and iteration

[Foreach](./foreach.md) iterates over a list of items, and [Await Foreach](./await-foreach.md) does the same for an asynchronous stream. [While](./while.md) repeats while a condition holds. Inside any loop, [Break](./break.md) exits the loop early, and [Continue](./continue.md) skips to the next iteration. For working with database results specifically, [DataReader iterator](./datareader-iterator.md) walks through records one by one, and [DataReader chunker](./datareader-chunker.md) splits a large reader into manageable batches — useful when memory limits prevent processing millions of rows in one pass. [Yield return](./yield-return.md) and [Yield break](./yield-break.md) build custom iterators that produce values lazily.

<br/>

## Sub-flows and reuse

[Run Flow](./run-flow.md) executes another flow as a sub-process and waits for it to finish, optionally using its return value. [Start Flow](./start-flow.md) launches another flow but continues immediately without waiting — useful for fire-and-forget work where the parent doesn't need the result. [Return](./return.md) ends the current flow and optionally passes a value back to the caller, and [Get Startup argument](./get-startup-argument.md) reads data passed in by the caller at the start. [Restart Flow](./restart-flow.md) ends the current execution and schedules the same flow to resume later — typically used to wait out long pauses, such as rate-limit windows from external services. For custom logic that doesn't fit other actions, [Function](./function.md) lets you write C# directly.

<br/>

## Extensions and hooks

These actions support building modular, extensible flows. [Extension Entry](./extension-entry.md) defines a reusable block of business logic with parameters and an optional return type, callable from elsewhere in the flow. [Execute object method](./execute-object-method.md) invokes a method on an object — including calling an Extension Entry on the current flow. [Hook](./flow-hook.md) defines a placeholder where custom logic can be plugged in without modifying the surrounding flowchart, and [Hook handler](./flow-hook-handler.md) implements that logic in an extension flowchart. Together, these enable shipping a standard flow that customers can extend without breaking the original.

<br/>

## Diagnostics and timing

[Log](./log.md) writes a message to the Flow log for diagnostics or audit purposes, and [Wait](./wait.md) pauses execution for a specified interval before continuing.

<br/>

## Other

[Remove InVision object from cache](./remove-invision-object-from-cache.md) clears a cached InVision object — used in [Calculation Flows](../profitbase-invision/calculation-flow/overview.md) when you need to force a fresh load of a Lookup table, Auto transaction, Distribution key, or Dimension.
