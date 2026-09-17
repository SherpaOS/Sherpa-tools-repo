<!-- GENERATED COPY — DO NOT EDIT.
     Source: shared/rv-deal-fields.md
     Regenerate: python3 scripts/sync_rv_fields.py
     CI fails the build if this file drifts from the source. -->

# RV / MHP deal fields — ONE canonical list, surfaced at two depths

> **⚠️ CANONICAL SOURCE. Do not edit the copies.**
> This file is the single source of truth for every question the RV funnel asks.
> `scripts/sync_rv_fields.py` copies it into each plugin that needs it, and CI fails the
> build if a copy has drifted. Edit **this** file, run the script, commit both.

## Why one list

The screen's *"what's still open"* and the deep underwrite's intake interview are **the same
list at two depths**. Written twice, they drift — and the drift is invisible, because each
skill looks correct on its own. Written once, they cannot.

- **SHAPE** — what a bird dog can get from a phone call. Roughly 60–70% of the full list.
- **TRUTH** — what only documents can settle. The underwriter's half.

**The bird dog establishes the SHAPE of the deal. The underwriter establishes its TRUTH.**
The seam is visible in one row: the bird dog asks *whether a P&L exists*; the underwriter
asks *for the P&L*.

## How to use this

- **Screen (free)** — ask nothing the free lookups already answered. Everything still empty
  in SHAPE becomes the call sheet, each with its `why` line. Never invent a value; an
  unfilled field is **NOT FOUND**, which is the script for the call, not a failure.
- **Deep underwrite (paid)** — walk SHAPE then TRUTH, **one question per turn**. Every
  question accepts three answers: the answer, **"I don't have it"** (a fact about the
  *deal* — record it and proceed under a stated rule), and **"I don't know"** (a fact about
  the *owner* — explain the term in a sentence and say where to find it).
- **`why` is the teaching.** Show it. Over a few deals the bird dog stops needing the
  reasoning and only needs the checklist — which is the goal.
- **`key`** is the stable identifier for the structured (JSON/CSV) output. **Never rename a
  key** — downstream imports bind to it.

## FREE — fillable without contacting anyone

These are pulled, not asked. If a pull fails, mark `NOT FOUND` and move on — never pay to
fill at screen stage, never guess.

| key | field | source | why it matters |
|---|---|---|---|
| `imagery_satellite` | Satellite image | imagery provider | **Goes at the top of every deliverable.** First verification step — a wrong satellite image is worse than none. |
| `asset_class` | RV park vs MHP vs mixed | EPA SDWIS system class (TNCWS→RV, CWS→MHP) | Decides which playbook applies and whether it qualifies for an RV-only mandate. Route, never discard. |
| `pad_count_proxy` | Approx. site count | SDWIS connections + national compile | A proxy, **not** a verified count — always confirm with the seller. Sets the size class. |
| `parcel_size` | Acreage | county parcel data where public | Land per site tells you whether expansion is even physically possible. |
| `flood_zone` | FEMA flood zone | FEMA NFHL via the ArcGIS endpoint — **see "FEMA flood zone — the working method" below** | **AE/VE changes insurability and financeability** — a genuine pass signal, not colour. |
| `fire_hazard` | Wildfire hazard class | USFS Wildfire Hazard Potential | Insurance cost and carrier availability in the West. |
| `crime_area` | Violent / property rate | FBI Crime Data Explorer | ⚠️ **Agency-level (city/county), NOT address-level.** Label it as such or it reads as wrong. |
| `owner_name` | Owner of record | county recorder where public | Who you are actually calling. Often an LLC — the human behind it is the next question. |
| `owner_contact` | Phone / mailing address | public records where available | Frequently NOT FOUND at screen stage. That is expected. |

## SHAPE — the bird dog's call sheet (60–70%)

Everything here is answerable on one phone call with a seller or broker.

| key | question | why it matters |
|---|---|---|
| `asking_price` | What are they asking? | Without it there is no cap rate and nothing to reprice against. |
| `site_count` | How many sites total? | The single biggest driver of value. The proxy is a guess until they confirm it. |
| `site_mix` | How many full-hookup vs water-electric vs dry vs MHP pads vs cabins? | **Dry sites rent for roughly a third of full-hookup.** A 100-site park that is half dry is not a 100-site park. |
| `rents_by_type` | Current rent for each type — nightly, weekly, monthly? | Mixed-term parks hide their real revenue in the blend. Ask per type or the number is meaningless. |
| `occupancy_mix` | What share is annual/long-term vs seasonal vs overnight? | Annual is bankable income; transient is a business. **Lenders treat them completely differently.** |
| `occupancy_rate` | How full, and in which months? | Seasonal parks are 90% full for four months. An annual average hides that. |
| `utilities_who_pays` | Who pays electric, water, sewer, trash — park or tenant? | **Can swing NOI 20%+.** Park-paid electric on annuals is the classic margin killer. |
| `water_source` | Well or municipal? | A private well is a regulated public water system with testing obligations — and it is why SDWIS knows the park exists. |
| `sewer_type` | Septic, lagoon, or municipal? | **Septic or lagoon is the most common deal-killer** — replacement runs six figures and caps expansion. |
| `infrastructure_age` | How old are the electric pedestals, water lines, sewer? | 30-amp-only pedestals cannot serve modern rigs. Rewiring is the hidden capex. |
| `other_income` | Storage, propane, laundry, vending, cabins, boat/RV storage? | Often the value-add thesis. **Ask early — sellers rarely volunteer it.** |
| `expansion_room` | Vacant pads, or raw land to expand into? | The cheapest NOI in the deal. Also triggers the zoning question — many parks are legal non-conforming and expanding forfeits it. |
| `management` | Owner-operated, on-site manager, or third-party? | Owner-operated means the P&L has no management expense, so the stated NOI is overstated for a buyer. |
| `reason_for_selling` | Why are they selling? | **The creative-finance read.** Tired landlord, health, estate, or partnership split each imply a different structure. |
| `seller_financing` | Would they consider carrying paper? | Ask on the FIRST call. It reframes every number that follows. |
| `docs_exist` | Do they have a P&L, T-12, or rent roll? **(yes/no — do not request yet)** | **The seam.** Yes → underwritable. No → you are buying on a story, price accordingly. |

## TRUTH — the underwriter's half (documents, not conversation)

Never asked by the screen. The deep underwrite requests these once the shape says pursue.

| key | field | why it matters |
|---|---|---|
| `t12_actuals` | Trailing-12 income statement | The only thing that turns claimed revenue into collected revenue. |
| `rent_roll` | Rent roll with terms and delinquency | Reveals concessions, long-vacant sites, and who is actually paying. |
| `expense_detail` | Full expense line items | Seller expense ratios are optimistic almost without exception. |
| `tax_bill` | Current property tax bill | **Reassessment on sale can double it.** Underwrite the post-sale number, never the seller's. |
| `utility_bills` | 12 months of utility bills | The only way to verify who really pays what. |
| `payroll` | Payroll and management contract | Normalizes an owner-operated P&L to a buyer's cost. |
| `capex_history` | Capex and deferred maintenance | Separates a value-add from a money pit. |
| `permits_licenses` | Operating permits, water-system and septic permits | A lapsed permit is a closing condition, sometimes a deal-killer. |
| `environmental` | Any Phase I / known contamination | Fuel tanks and old dumping are real on rural park land. |

## Structured output

Both skills emit **readable markdown and structured JSON/CSV**, always. Structured records
use these `key` values, with one of: the value, `"NOT FOUND"` (pull failed or unavailable),
`"NOT PROVIDED"` (asked, seller did not have it), or `null` (not yet asked).

Those three are **not interchangeable** — the difference between *nobody asked*, *the data
does not exist*, and *the seller would not say* is often the most informative thing in the
file.


## FEMA flood zone — the working method (added 2026-09-17)

**Do not let the model find its own endpoint.** Until this section existed, both RV skills
named the source (*"FEMA NFHL, address-level"*) and gave no way to call it. Every run
re-derived the URL, most landed on the widely-cited `hazards.fema.gov/gis/nfhl/…` — which
is **dead (404, verified 2026-09-17)** — and `flood_zone` came back `NOT FOUND` for every
customer, on a field the skill advertises. A named source with no method is not a method.

**1. Geocode the address to lat/lon.** Start with the US Census geocoder
(`geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=…&benchmark=Public_AR_Current&format=json`).
**It frequently misses rural addresses — exactly the ones RV parks have.** On a miss, fall
back to OpenStreetMap Nominatim. If BOTH fail, the answer is `NOT FOUND`; never flood-check
an address you could not place.

**2. Query the live NFHL layer** (free public GIS, no key, no account):

```
https://hazards.fema.gov/arcgis/rest/services/public/NFHL/MapServer/28/query
  ?geometry={"x":<LON>,"y":<LAT>}&geometryType=esriGeometryPoint&inSR=4326
  &spatialRel=esriSpatialRelIntersects
  &outFields=FLD_ZONE,ZONE_SUBTY,DFIRM_ID&returnGeometry=false&f=json
```

Note `arcgis`, **not** `gis`. Layer 28 is the flood-hazard area layer.

**3. Sample more than one point on a large rural parcel.** A park can span several hundred
metres and cross a zone boundary; the centroid alone can read `X` while a corner reads `AE`.
Query the parcel point plus four points ~300 m N/S/E/W. **Any `AE`/`VE` in the set is the
answer** — report the worst zone found and say how many points were sampled.

**4. Read the result honestly.** `features: []` means *no mapped flood-hazard polygon at
that point* — report `Zone X (no mapped SFHA)` only when the service actually returns it,
and `NOT FOUND` when the service errors or returns nothing usable. **An empty response is
not a clean bill of health.**

**5. Cite it.** Give `FLD_ZONE`, the FIRM panel (`DFIRM_ID`) and its effective date, so a
lender or insurer can check the same record. A flood answer without its panel is unusable
in diligence.

> Verified live 2026-09-17: the `gis/nfhl` path returns **404**; the `arcgis/rest` path
> returns **200** with populated `FLD_ZONE`. Originally diagnosed by Wes Pipes while
> underwriting York Kampground, where the field kept returning empty.
