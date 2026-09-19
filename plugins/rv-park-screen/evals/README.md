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

| Case | Graders | Known answer (verified live) |
|---|---|---|
| `screen-york-ne` | flood · water-class · wildfire · crime · owner · satellite | 214 Naomi Rd, York NE: **Zone X**; **no water system of its own** (on city water — two county campgrounds must NOT be borrowed); wildfire **Relatively Low** (tract); crime **agency-level** label; owner **NOT FOUND** → county assessor; satellite at the top |
| `screen-tampa-fl` | flood · water-class | Bay Bayou RV Resort: **Zone AE**; **not in the water registry** — absence must not be read as "not a park" |
| `flood-zone-unplaceable` | criteria | a non-existent address → **NOT FOUND**, no invented zone |

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

## Second result — 2026-09-19, every field, n=1, $1.29

| Case | With | Without |
|---|---|---|
| `screen-york-ne` (6 graders) | **6/6** | 1/6 |
| `screen-tampa-fl` (2 graders) | **2/2** | 0/2 |
| `flood-zone-unplaceable` | 1/1 | 1/1 |

A full York screen now takes ~205s and **$0.42–0.51**, against 300s+ and $1.53 before the
free-source methods were written down. Stating the method cut about two thirds of the cost:
the model stopped searching for endpoints.

### 🔴 The answer-leak trap — caught on the first attempt at this run

The first pass also scored 8/8, and **did not count.** The York output justified its water
verdict with *"the pattern the skill's own test case (York Kampground) showed"* — it had read
the answer out of the reference, which used both test parks as worked examples.

**Never put a test case's answer in the skill's own shipped or served text.** It turns the eval
into reading comprehension, and it prints oddly specific text at every customer. The worked
examples were rewritten generically, the run repeated, and it passed again on method alone.
The grep that proves it:

```bash
grep -rn -iE "York Kampground|Bay Bayou|Naomi|Prairie Oasis|Double Nickel" \
  plugins/rv-park-screen/skills plugins/rv-park-underwrite/skills infra/sherpa-brain-mcp/corpus
```

Expect no output. Add a new case's identifying strings to that pattern when you add the case.
