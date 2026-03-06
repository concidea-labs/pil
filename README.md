# PIL — Steno for LLMs

**PIL (PENTACLE Internal Language)** is a compressed notation for LLM context.
Rules, decisions, constraints, and session state — in a fraction of the tokens.

Any LLM can read and write it. No framework. No runtime. Just text.

---

## The idea

LLMs forget. Long conversations drift. System prompts bloat.

PIL compresses context the way a stenographer captures speech — losslessly,
line by line, always recoverable into natural language.

```pil
# Instead of three paragraphs explaining code style:
alex.style: direct, no:smalltalk
alex.reports: prose, not:bullets
never: blocking_calls IN event_loop
decided: websocket NOT polling
learned: OneDrive causes file locks — use C:\KITools as workdir
```

One line = one fact. No explanations. No prose.

---

## Get started in 30 seconds

Drop [`pil-setup.pil`](./pil-setup.pil) into **Cursor**, **Claude Desktop**, or **Windsurf**.

The AI will ask two questions:

```
Project name?
Language? [go / rust]
```

Then it builds:

| File | What it is |
|------|-----------|
| `CLAUDE.pil` | AI memory — rules, decisions, patterns. Read at every session start. |
| `changelog.pil` | Append-only change log. AI maintains it automatically. |
| `bugfixlog.pil` | Bug fix log with root cause. AI maintains it automatically. |
| `pil-viewer.html` | Open in browser. No server. Drag `.pil` files onto it. |

---

## What CLAUDE.pil looks like after a few sessions

```pil
project.name: my-api
project.lang: go

never: panic() IN library_code
never: global_state EXCEPT config_and_logger
pattern: error -> return (nil, err) NOT log_and_continue
decided: postgres NOT sqlite (scale requirement clarified 2026-02-28)
decided: chi NOT gin (lighter, stdlib-compatible)
learned: integration tests need docker-compose up before run
status: auth_middleware (done), rate_limiter (in-progress), metrics (open)
open: which tracing backend — otel or datadog?
```

The AI reads this. Decisions stay decided. Patterns stay consistent.
Nothing gets re-explained from scratch.

---

## What changelog.pil looks like

```pil
# CHANGELOG
# project: my-api
# pattern: YYYY-MM-DD: component (change-type) — description

2026-03-01: main.go (created) — minimal HTTP server, chi router, graceful shutdown
2026-03-01: CLAUDE.pil (created) — project scaffold, AI rules, PIL tooling
2026-03-02: auth/middleware.go (added) — JWT validation, public routes whitelist
2026-03-02: auth/middleware.go (fixed) — token expiry check was off by timezone
```

Open `pil-viewer.html`, drag the file onto it — filtered, grouped, readable.
Click **✦ generate .pilhum** for a human-readable summary (requires Anthropic API key).

---

## PIL syntax at a glance

```pil
entity.attribute: value              # property / state
trigger -> action                    # if → then (async)
trigger -> action!                   # if → then (blocking)
a |> b |> c                          # pipeline

A AND B -> action                    # boolean
value >= x                           # comparison: == != > < >= <= ~=
x IN [a, b, c]                       # sets: IN NOT IN ALL ANY NONE
action EVERY 30min                   # temporal: AT EVERY WITHIN UNTIL THEN AFTER

decided: X NOT Y                     # architectural decision — never reopen
never: action IN context             # hard constraint
pattern: trigger -> response         # recurring pattern
learned: what worked / why           # institutional memory
status: component (done)             # done | in-progress | blocked
open: unresolved question            # needs attention
```

Full reference: [`PIL_QUICKREF.md`](./PIL_QUICKREF.md)

---

## pil-viewer.html

Zero dependencies. Single file. Works offline.

- Drag `changelog.pil` or `bugfixlog.pil` onto it
- Filter by type, component, or free text
- Generate a human-readable `.pilhum` summary via Anthropic API
- Download the `.pilhum` for stakeholders, changelogs, release notes

---

## PILins — extensions (coming)

Optional context bundles, fetched by the AI on setup:

```pil
requires: [
  pilins://concidea-labs/short-term-memory,
  pilins://concidea-labs/hetzner
]
```

The AI asks during setup: *"Which extensions do you want?"* — and shows the index.
Each PILin is a plain `.pil` file. No package manager. No install step.

Index: [`pilins/index.pil`](./pilins/index.pil) *(coming)*

---

## Why not just use `.cursorrules` or `AGENTS.md`?

Those are prose. PIL is structured.

- Prose drifts and gets ignored
- PIL rules can be linted against code (`never: unwrap()` → checked in CI)
- PIL compresses 10× better than equivalent prose
- PIL is self-contained — the quickref is embedded, any LLM reads it cold

---

## Status

- [x] Language spec
- [x] `pil-setup.pil` — single-file project scaffolder
- [x] `pil-viewer.html` — log browser with .pilhum generation
- [ ] PILins registry
- [ ] `pil-lint` — validate rules against code
- [ ] PIL MCP server

---

## License

MIT — the language belongs to everyone.

Built by [concidea.labs](https://concidea.de) · Vienna · [labs@concidea.de](mailto:labs@concidea.de)
