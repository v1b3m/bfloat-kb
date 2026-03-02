# Unknown Bug Investigation

## Description
Error appearing in the Convex embedded dashboard iframe:

```
[Error] Encountered unexpected state in data page: status: Exhausted, numRowsInTable: 0, numRowsRead: 0, isLoading: false – "warning"
```

Stack trace points to `data-37b9022019467a8c.js` and `framework-a6339a3b935b453e.js` — both Convex dashboard internals.

## Progress
- [x] Capture error details
- [x] Investigate whether related to Monaco layout loop fix
- [x] Determine if actionable on our side

## Decisions
- **Not related to Monaco layout loop**: The Monaco bug was an infinite `layout()` recursion causing stack overflow. This is a React state warning in Convex's data page component.
- **Not actionable by us**: The bug lives entirely in Convex's dashboard code — their data page hits an unexpected `Exhausted` pagination state with zero rows.

| | Monaco layout loop (fixed) | This bug |
|---|---|---|
| **Error** | `Maximum call stack size exceeded` | `Unexpected state in data page: Exhausted` |
| **Source** | Monaco editor v0.54.0 `layout()` recursion | Convex data page component |
| **Nature** | Infinite recursion → stack overflow | Warning-level React state inconsistency |
| **Impact** | Crashes parent app, kills agent streams | Likely cosmetic edge case |
| **Actionable?** | Yes (CSS containment + sandbox — applied) | No — Convex internal |

## Outcome
**No action needed.** This is a Convex dashboard internal issue where a data query returns `status: Exhausted` with zero rows, and their React code logs a warning. Could be worth reporting to Convex if it causes visible UI glitches.