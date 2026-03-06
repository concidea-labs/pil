# PIL Quickref
**PIL = PENTACLE Internal Language — Steno for LLMs**

PIL compresses context losslessly. Any LLM can read and write it.
Rule: English only. One concept per line. No prose.

---

## Core Syntax

```
entity.attribute: value              # property / state
entity.attribute: v1, v2, v3        # multiple values
trigger -> action                    # if -> then rule
trigger -> action!                   # blocking (synchronous)
a |> b |> c                          # pipeline
```

## Boolean Operators

```
A AND B -> action                    # both must be true
A OR B -> action                     # at least one
NOT A -> action                      # negation
A XOR B -> action                    # exactly one
```

## Comparison Operators

```
value == x   value != x
value > x    value >= x
value < x    value <= x
value ~= "pattern*"                  # wildcard match
```

## Set Operators

```
x IN [a, b, c]
x NOT IN [a, b, c]
ALL [a, b, c] -> done
ANY [a, b, c] -> alert
NONE [a, b] -> proceed
```

## Temporal Operators

```
action AT 07:00
action EVERY 30min
action WITHIN 30min
loop UNTIL condition
a THEN b                             # sequential
a AFTER b                            # a only when b complete
```

## Annotations

```
task @priority(1)                    # 1=highest
action @cost(0.01EUR)
task @timeout(30min)
action @retry(3)
```

## References

```
ctx:name      skill:name      instance:name
src:name      dst:name
```

## Constraint Levels

```
FIXED: never: x                      # absolute, never override
BOUNDED: name {
  min: x
  max: y
}                                    # range with hard limits
GOAL: target(expr) @weight(0.8)     # soft goal with weight
FREE: ref                            # LLM decides
```

---

## Session Conventions

These keys are standard — use them consistently:

```
decided: X NOT Y                     # architectural decision
decided: X FOR purpose               # tool/tech choice
never: action IN context             # hard constraint
pattern: trigger -> response         # recurring pattern
learned: what worked / what didn't
status: component (state)            # done / in-progress / blocked
open: question or unresolved item
produced: filename (description)
```

---

## Examples

```pil
# User preferences
alex.style: direct, no:smalltalk
alex.lang: de, not:technical_terms
alex.reports: prose, not:bullets

# Budget rule
budget.usage_pct >= 75 AND (tasks.active == 0) -> ask(alex)!
budget.used >= budget.limit -> stop() |> alert(alex)!

# Scheduled pipeline
scan EVERY 30min -> filter(profile:news) |> analyze::relevance() |> route()

# Domain expiry
domain.expires WITHIN 14d -> renew() @retry(3)

# Session summary
decided: sqlite NOT turso
decided: websocket NOT polling
never: blocking_calls IN event_loop
status: parser.ts (done), evaluator.ts (done), scheduler.ts (open)
learned: OneDrive causes file locks — use C:\KITools as workdir
open: ANTHROPIC_API_KEY not set for LLM tools
```

---

## Skill Block

```pil
skill: domain-monitor {
  version: 1.0
  requires: [ctx:api-key, ctx:config]
  never: [not:exceed-budget, not:delete-unconfirmed]
  exposes: [scan, analyze, report]
  cost: 10
  instance: cron("*/30 * * * *")
}
```

---

## How to Write PIL

1. Replace "we decided to use X instead of Y" -> `decided: X NOT Y`
2. Replace "never do X in context Y" -> `never: X IN Y`
3. Replace "task Z is finished" -> `status: Z (done)`
4. Replace "we noticed that X works better when Y" -> `learned: X when Y`
5. Compress lists with commas, not bullets
6. One line = one fact. No explanations.

## How to Read PIL

Read each line as a complete statement.
`decided: X NOT Y` = "we chose X, rejected Y, don't reopen this"
`never: X IN Y` = hard constraint, treat like a test that must pass
`open: X` = unresolved, may need attention
`status: X (done)` = completed, no further action needed
