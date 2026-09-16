---
name: call-graph
description: Use when showing call graphs, execution flows, or architecture traces. Enforces consistent plain-text output and Effect A/E/R graph thinking.
---

# Call Graph

Use this skill when the answer includes a call graph, execution flow,
or architecture trace. Load it before answering, then follow it exactly.

## Output format (mandatory)

- Plain text only, no rendered diagrams.
- Indented `→` arrows for hierarchy.
- `ts` code block.
- Production and Tests as separate sections when they differ.
- Include call graphs in project overviews, architecture summaries, and code explanations.

Production:

```ts
HTTP handlers
  → ComponentA
    → ComponentA.layerX
      → ComponentB
        → ComponentC
```

Tests:

```ts
HTTP handlers
  → ComponentA
    → componentMemoryLayer
      → ComponentA.layer
        → ComponentB.layerMemory
```

## Thinking pipeline (X → Graph → Effect<A, E, R>)

Read the problem. Draw the data flow as a call graph. Write code that IS the graph.
If the code doesn't match the graph, the implementation is wrong.

1. Shapes: records, branded IDs, variants, tagged errors. Define nouns before verbs.
2. A first: happy path as `F1(A) -> F2(A) -> F3(A)`. This graph IS program structure.
3. Cardinality: one-shot (`Effect`) or many over time (`Stream`). Mark it on the graph.
4. E second: mark break points as Retry (transient), Escape Hatch (fallback), or Die (defect only).
5. R third: mark what each node needs. `R=never` means runnable. Missing R is a compile error.
6. Boundary: `unknown → trusted` via Schema at edges only. Trust everything inside.
7. Behavior: `.pipe()` wraps nodes (retry, timeout, log, trace, cache) without changing the graph.
8. Scope: acquire/release tied to lifecycle, even on error/interrupt.
9. Test: swap R, same graph shape. Hidden dependencies or world-mocks mean the node is too big.
10. Code: `Effect.gen` body = A, `.pipe()` after gen = complete E enumeration.

Pipeline questions: shapes? happy path? one-shot or flow? where can it break?
what does each node need? where does untrusted data enter? what wraps nodes?
what needs cleanup? does swapping R still work? does code separate A from E?

## E scoping at graph layers

Each layer scopes its own E before passing up:

```ts
Services
  → Auth
    → Handlers
```

A service catches `SqlError` → `DatabaseError`. Auth catches `DatabaseError` →
`AuthError`. Handler catches `AuthError` → RPC errors. Each pipe enumerates what
IT received, never implementation errors from deeper layers.

## Divergent strategies (only exception)

When two effects in the same gen body need different E handling (one fails hard,
one falls back), handle E inline in the gen body. Mark it clearly. This is rare.
Otherwise keep all error handling out of gen.

Sources:
- https://gist.github.com/r17x/90eb2f7be93932b5693753aedb09c01a
  - DESIGN_THINKING.md: X → Graph → Effect<A,E,R> pipeline (§1–§10, gen=A / pipe=E)
  - ECALL_GRAPH_IN_YOUR_AGENTS.md: plain-text indented → output format
