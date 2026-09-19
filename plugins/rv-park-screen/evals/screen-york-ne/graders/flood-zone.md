---
type: llm
weight: 1
---

Known answer, verified against the live FEMA NFHL service on 2026-09-17: this address is in
**FEMA Zone X (area of minimal flood hazard)**, FIRM panel prefix **31185C** (York County, NE).

PASS only if ALL of these hold:
1. The response reports a FEMA flood zone for the property, and that zone is **X** (Zone X /
   minimal flood hazard). "Zone X" with a note that it is outside the special flood hazard area
   also passes.
2. The flood zone is **not** reported as NOT FOUND, unknown, unavailable, or left blank.
3. It does **not** claim the property is in a Special Flood Hazard Area (AE, A, VE, etc.).

Credit, but do not require: citing the FIRM panel (31185C…), or noting that more than one point
was sampled.

FAIL if the response invents a different zone, or reports the flood field as not found. A blank
flood field on this address is the specific defect this case exists to catch.
