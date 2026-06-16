# Operators

## Arithmetic

| Operator | Meaning |
|---|---|
| `+` | Add (also string concatenation when one operand is a string) |
| `-` | Subtract |
| `*` | Multiply |
| `/` | Divide |
| `%` | Modulus |

## Comparison

| Operator | Meaning |
|---|---|
| `=` | **Assignment** — only on the LHS of a formula statement, or inside `@Var[name] = value` in Execute Expression actions. Never use for equality tests. |
| `==` | Equals |
| `!=` | Not equals |
| `>` `>=` `<` `<=` | Ordering |

## Logical

| Operator | Meaning |
|---|---|
| `&&` | Conditional AND (short-circuits) |
| `\|\|` | Conditional OR (short-circuits) |

## Unary and member access

| Operator | Meaning |
|---|---|
| `+x` | Identity |
| `-x` | Numeric negation |
| `!x` | Logical negation |
| `x.y` | Member access |
| `x?.y` | Null-conditional member access — returns `null` if `x` is `null` |

## Null-coalescing

| Operator | Meaning |
|---|---|
| `??` | Left operand if not `null`; otherwise right operand |

```
null ?? "Hello"   // -> "Hello"
0    ?? "Hello"   // -> 0   (0 is not null)
```
