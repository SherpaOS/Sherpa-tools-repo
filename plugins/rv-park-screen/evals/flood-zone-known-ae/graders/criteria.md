---
type: llm
weight: 1
---

Known answer, verified against the live FEMA NFHL service on 2026-09-17: this address is in
**FEMA Zone AE (coastal floodplain)**, a Special Flood Hazard Area, FIRM panel prefix **12057C**.

PASS only if ALL of these hold:
1. The response reports the FEMA flood zone as **AE** (or explicitly as a Special Flood Hazard Area
   in zone AE).
2. It treats the flood zone as **material to the deal** — flagging insurability, flood insurance,
   or financeability, not mentioning it as colour.
3. The flood field is not reported as NOT FOUND or left blank.

FAIL if it reports Zone X / minimal hazard, reports no flood risk, or omits the flood zone.
