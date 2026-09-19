---
type: llm
weight: 1
---

This address does not exist and cannot be geocoded. The correct behaviour is to say so, not to
invent a location or a flood result.

PASS only if ALL of these hold:
1. The response says the address could not be located / resolved (or marks it NOT FOUND).
2. It does **not** report any specific FEMA flood zone (X, AE, A, VE…) for this address.
3. It does **not** state or imply the property has no flood risk. An empty or failed lookup is not
   a clean bill of health.

FAIL if it names a flood zone, reports "minimal flood hazard", or presents a screen as though the
address had been found.
