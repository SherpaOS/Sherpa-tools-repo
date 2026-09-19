# rv-park-screen evals

Run from `plugins/rv-park-screen/`:

```bash
claude plugin eval . --model sonnet --runs 3 --allow-tools WebFetch --trust-plugin --no-publish
```

**Use `--model sonnet` explicitly** — a nested run otherwise inherits the session's default
model and bills your quota accordingly. **Grant `WebFetch` only, not `Bash`:** on a machine whose
`~/.docker` credential store contains a symlink, the eval sandbox refuses any Bash-granting case
before the model starts, and every score reads 0.00 at $0.00 — which looks like a failure and is
not one.

| Case | Known answer (verified live 2026-09-17) |
|---|---|
| `flood-zone-known-x` | 214 Naomi Rd, York NE → **Zone X**, panel 31185C |
| `flood-zone-known-ae` | Bay Bayou RV Resort, Tampa FL → **Zone AE**, panel 12057C |
| `flood-zone-unplaceable` | a non-existent address → **NOT FOUND**, no invented zone |

## First result — 2026-09-18, n=1 per case, $2.89

With the skill **3/3**, without **0/3**. Read it carefully:

- Without the skill the model mostly declined to try (*"no web search"*). This shows the skill
  turns an unanswerable request into a correct one — not that it beats a model that attempts it.
- The unplaceable baseline did recognise the address as fake; the judge wanted an explicit
  "could not locate". A grader-strictness difference, not a capability one.
- **n=1 is a contrast, not a rate.** Use `--runs 3` before treating this as a release gate.

**Budget:** a full screen takes ~9 turns and $1–1.60 per case. A 25-turn / 300s cap timed out
mid-screen on the first attempt — after the skill had already retrieved `FLD_ZONE: AE` correctly.
Cases now allow 60 turns / 900s.

**Found while reading the output, not by a grader:** on the York run, most OTHER free sources the
skill promises (satellite, parcel, crime, owner of record) hit DNS failures, 403s, JS-only pages
or captchas. That is the FEMA defect class — a named source with no working method — on the
remaining fields. Those deserve cases of their own.
