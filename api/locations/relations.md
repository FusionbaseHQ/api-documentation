---
description: >-
  Drill into typed data slices — risks, crime, demographics, nearby businesses —
  for one location.
---

# Relations

Relations expose **typed data features** about a single location — flood and fire risks, crime statistics, more than 40 regional demographic and economic indicators, and the organizations registered at or near the address. Each slice is a separate relation identified by a stable `<RELATION_ID>`. You combine that ID with the `<ENTITY_ID>` (the `fb_entity_id` returned by Location Search or Location Entity) to fetch the slice for a specific location.

Browse the full set of location-bound relations in the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data) — they're grouped under their topical categories (Sicherheit, Bevölkerung, Wirtschaft, Naturkatastrophen, …) rather than under a single "Locations" header.

```
POST https://api.fusionbase.com/api/v2/relation/resolve/<RELATION_ID>/<ENTITY_ID>
```

***

#### Authentication

```http
X-API-KEY: YOUR_API_KEY
```

***

#### Path parameters

| Parameter       | Required | Description                                                                                                                                |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `<RELATION_ID>` | yes      | Stable identifier of the relation type. See Available location relations below.                                                            |
| `<ENTITY_ID>`   | yes      | The Fusionbase entity ID of the location. Get it from the Location Search endpoint — it's the `fb_entity_id` field on every search result. |

**Optional relation parameters**

Some relations accept additional parameters (for example, the regional-statistics relations can be narrowed to a specific reference year). Send them **nested inside a top-level `params` object** in the **request body**, with `Content-Type: application/json`:

```http
POST /api/v2/relation/resolve/<RELATION_ID>/<ENTITY_ID>
X-API-KEY: YOUR_API_KEY
Content-Type: application/json

{
  "params": { /* relation-specific params */ }
}
```

When a relation takes no parameters, the body can be omitted (or sent as an empty `{}` / `{"params": {}}`). Each relation's API Playground in the Data Hub lists the parameters it accepts.

***

#### Available location relations

Relation IDs are stable; the slugs in any example URLs are display-only and may be localized.

**Risk & safety**

| Relation                           | ID           | Description                                                                                                                                 |
| ---------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Flood & Water Protection Risks** | `9209128402` | Flood-risk zones (HQ100, HQ\_extrem) and water-protection areas overlapping the location. Source: federal and state environmental agencies. |
| **Fire-relevant businesses**       | `9209240732` | Businesses in the vicinity that handle fire-relevant materials or processes — chemical plants, large warehouses, paint shops, etc.          |
| **Crime statistics (detailed)**    | `9209216968` | Polizeiliche Kriminalstatistik at municipality / Kreis level. Cases per 100,000 inhabitants per category, with national-average comparison. |

**Discovery**

| Relation                 | Description                                                                                                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Nearby organizations** | Organizations registered within a geographic radius of the location. Useful for site-analysis and market-mapping workflows. See the catalog for the current relation ID. |

**Regional statistics (Destatis-derived)**

47 typed statistical relations expose Destatis indicators at the granularity the source publishes them (municipality, postal code, Kreis, etc.). They are grouped under the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data) by topic — Bevölkerung, Beschäftigung und Erwerbstätigkeit, Arbeitslosigkeit, Privateinkommen und private Schulden, Wirtschaft, Öffentliche Finanzen, Sozialleistungen, Bildung, Medizinische und soziale Versorgung, Bauen und Wohnen, Flächennutzung und Umwelt, Siedlungsstruktur, Verkehr und Erreichbarkeit, Sicherheit, Politik, Raumwirksame Mittel — and each carries its own stable relation ID listed on its catalog page.

A few examples (full list in the catalog):

| Topic                  | Example relation                                       | ID           |
| ---------------------- | ------------------------------------------------------ | ------------ |
| Unemployment           | **Arbeitslosenquote** (Unemployment rate)              | `9210180619` |
| Unemployment           | **Arbeitslosenstruktur** (Unemployment structure)      | `9210215445` |
| Population             | **Altersstruktur** (Age structure)                     | `9210215359` |
| Population             | **Bevölkerungsentwicklung** (Population development)   | `9210215409` |
| Construction & housing | **Bau- und Mietpreise** (Construction & rental prices) | `9210215367` |
| Economy                | **Wirtschaftliche Leistung** (Economic performance)    | `9210215377` |
| Education              | **Schulische Bildung** (School education)              | `9210215429` |
| Traffic                | **Erreichbarkeit** (Accessibility)                     | `9210215357` |

Each of these returns an observation series — usually one row per `reference_year` keyed on the location's postal code or Kreis — with numeric metrics and a `units` map (`%`, `EUR`, `per_km2`, `per_1000_inhabitants`, …).

***

#### Example requests

Fetch the **Flood & Water Protection Risks** relation for a Munich address (`fb_entity_id = b69ea5ed95c054a3ad9b3d75335386a7`):

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/9209128402/b69ea5ed95c054a3ad9b3d75335386a7' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

Fetch the **Crime statistics** relation for the same address:

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/9209216968/b69ea5ed95c054a3ad9b3d75335386a7' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

Fetch the **Arbeitslosenquote** (unemployment rate) regional statistic, narrowed to a specific reference year (note the `params` wrapper):

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/9210180619/b69ea5ed95c054a3ad9b3d75335386a7' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"params":{"reference_year":"2023"}}'
```

***

#### Response

All relation endpoints return the same envelope. The shape of `entity.value` varies per relation — see each relation's API Playground in the Data Hub for its exact schema.

**Envelope fields**

| Field                   | Description                                                                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `label`                 | Type of the top-level relation (e.g., `FLOOD_RISKS__AT_POINT`, `FIRE_RISKS`, `GERMAN_CRIME_STATISTICS`, `GERMAN_STAT__UNEMPLOYMENT_RATE`). |
| `entity_from`           | The location the relation was queried for — echoes your `fb_entity_id`.                                                                    |
| `entity.entity_type`    | Always `FEATURE` for typed location relations.                                                                                             |
| `entity.entity_subtype` | Specific feature subtype.                                                                                                                  |
| `entity.fb_semantic_id` | Stable identifier for the feature instance (typically `feature\|<subtype>\|location:<id>`).                                                |
| `entity.value`          | The actual payload. Shape depends on the relation.                                                                                         |

**Common payload shapes**

* **Flood & Water Protection Risks** (`9209128402`) — `value.flood_risks[]` with zone classification (`HQ100`, `HQ_extrem`) and `value.water_protection[]` with protection-area type (`drinkingWater`, `mineralSpring`).
* **Fire-relevant businesses** (`9209240732`) — array of nearby organizations classified as fire-relevant, with `distance_meters` and the business category.
* **Crime statistics** (`9209216968`) — array of crime-category records, each with `cases_per_100k`, `national_average`, `deviation_from_average`, and the year of measurement.
* **Nearby organizations** — array of organization references with `distance_meters`. Pivot into the Organization Entity endpoint via `fb_entity_id` for full company details.
* **Regional statistics** (`GERMAN_STAT__*`, IDs `9210*`) — `value.observations[]` keyed on `reference_year`, with named numeric metrics, `value.granularity` (e.g., `"Gemeinde"`, `"Kreis"`, `"postal_code"`), and a `value.units` map.

***

#### Tips

* **Cache by relation × entity.** Each `(relation_id, fb_entity_id)` pair is cacheable on the client. Use the `fb_semantic_id` from the response as a stable cache key.
* **Pair with Location Search.** Search first to resolve `fb_entity_id`, then fan out to the relations you need.
* **Pick the right resolution.** Building-level relations (flood-risk at point, fire risks, nearby organizations) need a building-level LOCATION entity (one with `entity_subtype: "LOCALITY"`); demographic and economic statistics resolve at municipality / Kreis / postal-code granularity — they accept building-level LOCATION IDs too and aggregate up to the right granularity automatically.
* **Pivot to organizations.** The Nearby organizations and Fire-relevant businesses relations return `fb_entity_id`s of companies — follow them into the Organization Entity and Organization Relations endpoints to drill into the company side.
* **Browse the catalog first.** The [Data Hub catalog](https://app.fusionbase.com/browse-data) lets you preview every location relation interactively before wiring it into your code. Each relation also has an API Playground page that shows the exact response schema for a real example location.
