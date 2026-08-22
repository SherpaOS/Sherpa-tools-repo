---
name: rv-park-screen
pack_version: 0.7.0
description: >-
  Screen ONE RV park, MHP or campground from an ADDRESS — no numbers required. Pulls what
  is knowable for free (satellite image, RV-vs-MHP class, pad-count proxy, parcel, FEMA
  flood zone, wildfire hazard, area crime, owner of record where public), then hands back
  the call sheet: exactly what to ask the seller or broker, and why each answer matters.
  Use when the user has a single property — "screen 123 Main St", "what about the park at
  [address]", "is this one worth a call", "I drove past a park today" — including when they
  have no numbers at all. A single specific property is ALWAYS a screen. Finding or
  prospecting parks ACROSS a county, city or market is not this skill — that is
  rv-park-scan. Running NOI, cap rate, DSCR or an offer matrix is not this skill either —
  that needs numbers and belongs to rv-park-quick-underwrite. Free sources only; anything
  unavailable is marked NOT FOUND, never guessed.
argument-hint: "<street address, or park name + city/state>"
---

> **Claims in this skill are governed by `sherpa-core/references/proof-protocol.md` where
> that pack is installed.** This skill ships standalone in the free tier, so it must work
> without it: **the rule that matters here is that a fact you did not retrieve is NOT
> FOUND, never inferred.**

# RV Park Screen

You are screening **one property, from an address**, for someone who is about to call a
seller or broker. They are not underwriting it. They may have no numbers at all — that is
normal and is not a blocker.

**Your job is not a verdict. It is a call sheet.**

They should finish this able to (a) speak about the property without having googled it,
and (b) **know exactly what to ask so the file comes back complete.** Where bird dogs
actually lose deals is coming back with a price and no site count, then having to go again.

**The questions are the deliverable.** The data you retrieve is the context that makes the
questions sharp.

---

## Step 1 — Ask before you pull

Ask **once**, in one line: *"Anything you already know about it — asking price, site count,
who you spoke to?"*

Take whatever comes. Accept "no" instantly and move on — **do not interview them here.**
This exists so you never ask later for something they told you at the start.

## Step 2 — Run the free lookups

**FREE SOURCES ONLY.** Never pay, never call a paid enrichment, never guess. A lookup that
fails is `NOT FOUND` — which is not a failure, it is a line on the call sheet.

Work from `references/rv-deal-fields.md` — the FREE table there is the canonical list. In
order:

1. **Satellite image — FIRST, always at the top of the output.** It is the verification
   step: the reader confirms with their own eyes that this is a park before reading
   anything else. **A wrong satellite image is worse than none** — if you cannot confidently
   resolve the address, say so rather than showing a neighbouring parcel.
2. **RV vs MHP** — EPA SDWIS system class. `TNCWS` (transient non-community) → **RV park /
   campground**. `CWS` (community) → **MHP / long-term**. Mixed is common and worth saying.
   This decides which playbook applies and whether it qualifies for an RV-only mandate.
3. **Pad-count proxy** — service connections from the registry. **State it as a proxy every
   single time.** It is an input to the conversation, never a number to underwrite on.
4. **Parcel size** where the county publishes it.
5. **FEMA flood zone** — National Flood Hazard Layer, address-level. **`AE` or `VE` changes
   insurability and financeability** — that is a genuine pursue/pass signal, not colour.
6. **Wildfire hazard** — USFS Wildfire Hazard Potential. Drives insurance cost and carrier
   availability in the West.
7. **Area crime** — FBI Crime Data Explorer. ⚠️ **This is AGENCY-level (city or county), not
   address-level, and you must say so on the line itself.** No free national address-level
   crime data exists. Reported without that label, the first reader who checks it against
   local knowledge concludes the tool is broken.
8. **Owner of record** — county recorder where public. Frequently `NOT FOUND`. Expected.

## Step 3 — The call sheet

Everything in the SHAPE table of `references/rv-deal-fields.md` that you did **not** fill —
minus anything they told you in Step 1 — becomes the list.

**Each question carries its one-line `why`.** That is the teaching, and it is the point:
over a few deals they stop needing the reasoning and only need the checklist.

Order by what unblocks underwriting fastest — **asking price, site count and mix, rents,
occupancy, who pays utilities, water/sewer.** Then the rest.

Ask **whether a P&L or T-12 exists — yes/no. Do not request it.** That is the seam: the
bird dog establishes the *shape* of the deal, the underwriter establishes its *truth*.

**Then offer, in one line:** *"Want me to draft the email to the seller or broker?"*
Only offer this if an outbound path is actually configured — see Step 4. **Never send
anything without being asked.**

## Step 4 — Deliver, and offer to submit

**Emit BOTH, every time:**

- **Readable** — markdown they can read, paste, or take to a call.
- **Structured** — JSON using the `key` values from `references/rv-deal-fields.md`. Each
  field is the value, `"NOT FOUND"` (pull failed or unavailable), `"NOT PROVIDED"` (asked,
  seller did not have it), or `null` (not yet asked). **These are not interchangeable** —
  *nobody asked*, *the data does not exist*, and *the seller would not say* are three
  different facts.

The difference between an email someone pastes into a CRM and an email that imports.

**Then offer the destination, resolved per operator — never to the publisher:**

```
nothing (DEFAULT)   hand it back; they route it themselves
email               one configured address
CRM webhook         their own inbound webhook
closer              a named role, if configured
```

**If nothing is configured, that is the correct end state.** Hand back the packet and stop.
**Never name a default recipient, and never imply their deal should go anywhere in
particular** — the person using this may be working with another leader, or on their own
deals.

---

## Boundaries — this skill owns ONE input

| They give you | Skill |
|---|---|
| a county, city, region, ZIP | **rv-park-scan** — geography in, list out |
| **ONE address or park name** | **this skill** — even with no numbers |
| numbers (asking, sites, rents) | **rv-park-quick-underwrite** — NOI, cap, DSCR, offer matrix |
| seller documents (P&L, rent roll) | **rv-park-deep-underwrite** |
| a normalized NOI | **rv-park-value-add-stack** |

**A single specific property is always a screen — never a market scan — even when no
numbers came with it.** That gap is what previously sent a bare address into a county-wide
scan.

**This skill never runs the numbers.** No NOI, no cap rate, no DSCR, no offer. If they hand
you an asking price and a site count and want the math, that is
`rv-park-quick-underwrite` — say so and hand it over.

## Rules

- **NOT FOUND is a real answer.** Never fill a gap with a plausible number. A screen whose
  gaps are honest is worth more than one that reads complete and is partly invented.
- **The pad count is a proxy. Say so every time it appears.**
- **Crime is agency-level. Label it on the line.**
- **Directional only — not financial advice**, and nothing here is a substitute for
  verifying with the seller and the county.
