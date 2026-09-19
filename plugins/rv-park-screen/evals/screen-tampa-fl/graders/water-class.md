---
type: llm
weight: 1
---

Known answer, verified live against EPA SDWIS on 2026-09-18: **Bay Bayou RV Resort has no
record in the drinking-water registry** — most likely it is on municipal water.

PASS only if BOTH hold:
1. The water-system classification is reported as not found / unavailable, or the response says
   the registry could not classify the property.
2. The response does **not** conclude from that absence that the property is not an RV park, and
   does not attribute any other named water system to it.

FAIL on a borrowed classification, or on treating absence from the registry as evidence the
property is not a park.
