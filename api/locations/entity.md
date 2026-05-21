---
description: >-
  Fetch the canonical profile of one location — coordinate, address components,
  formatted address.
---

# Entity

The Location Entity endpoint returns the canonical, structured profile of a single location — coordinate, parsed address components, formatted address, alternative names — keyed by its Fusionbase entity ID (`fb_entity_id`). This is the "base record" you fetch after locating a place with Location Search; the deeper, typed data slices (risks, crime statistics, regional indicators, nearby organizations) are exposed separately via the Location Relations endpoint.

```
GET https://api.fusionbase.com/api/v2/entities/location/get/<FB_ENTITY_ID>
```

***

### Authentication

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Path parameters

| Parameter        | Required | Description                                                                                                                                                                               |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<FB_ENTITY_ID>` | yes      | The Fusionbase entity ID of the location. Get it from the Location Search endpoint — it's the `fb_entity_id` field on every search result. Stable across data refreshes; safe to persist. |

***

### Example request

```bash
curl 'https://api.fusionbase.com/api/v2/entities/location/get/aeb56e1cd890051a9225a98e4149d331' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### Response

The endpoint returns a single JSON object representing the location. Optional fields may be `null` or absent depending on the resolution of the match (a street-level building entity carries more detail than a country-level one).

#### Top-level fields

| Field                | Type              | Description                                                                                    |
| -------------------- | ----------------- | ---------------------------------------------------------------------------------------------- |
| `fb_entity_id`       | string            | Stable Fusionbase identifier — matches the request path.                                       |
| `fb_datetime`        | ISO 8601 datetime | Server-side timestamp for this snapshot.                                                       |
| `fb_entity_version`  | string            | Content-hash of the entity record. Changes whenever any field changes — useful as a cache key. |
| `entity_type`        | string            | Always `"LOCATION"` on this endpoint.                                                          |
| `entity_subtype`     | string            | Resolution of the place. See Subtypes.                                                         |
| `created_at`         | ISO 8601 datetime | When Fusionbase first ingested this entity.                                                    |
| `updated_at`         | ISO 8601 datetime | When the entity record was last updated.                                                       |
| `coordinate`         | object            | `{ "latitude": <number>, "longitude": <number> }` — WGS-84 decimal degrees.                    |
| `address_components` | object\[]         | Flat list of parsed pieces — see Address components.                                           |
| `formatted_address`  | string            | Single-line rendering ready for UI.                                                            |
| `alternative_names`  | string\[]         | Other names by which the place is known (often empty).                                         |
| `location_level`     | string \| null    | Administrative level when applicable (e.g., `KRE` for Kreis).                                  |
| `elevation`          | number \| null    | Elevation above sea level in metres, when known.                                               |

#### Entity subtypes

`entity_subtype` describes the resolution of the place. The most common values:

| Value                 | Meaning                                                            |
| --------------------- | ------------------------------------------------------------------ |
| `LOCALITY`            | A specific address — street + house number resolved to a building. |
| `CITY_POSTAL_CODE`    | A postal-code area inside a city.                                  |
| `CITY_NO_POSTAL_CODE` | A whole city / town without postal-code disambiguation.            |
| `MUNICIPALITY`        | A municipality (Gemeinde).                                         |
| `COUNTY`              | A Kreis / Landkreis.                                               |
| `STATE`               | A federal state (Bundesland).                                      |
| `COUNTRY`             | A country.                                                         |

Building-level relations (flood risk at point, fire risks) only attach to LOCATION entities with `entity_subtype: "LOCALITY"`. Statistical relations (population, employment, …) typically resolve at `MUNICIPALITY` / `COUNTY` / postal-code granularity.

#### Address components

`address_components` is a flat array of `{ component_type, component_value }` pairs:

```json
"address_components": [
  { "component_type": "house_number", "component_value": "118" },
  { "component_type": "street",       "component_value": "Dennhäuser Straße" },
  { "component_type": "postal_code",  "component_value": "34134" },
  { "component_type": "city",         "component_value": "Kassel" },
  { "component_type": "county",       "component_value": "Kassel (Stadt)" },
  { "component_type": "state",        "component_value": "Hessen" },
  { "component_type": "country",      "component_value": "DEU" }
]
```

| `component_type` | Description                                                                             |
| ---------------- | --------------------------------------------------------------------------------------- |
| `house_number`   | Building number (may be a range like `"23-25"`).                                        |
| `street`         | Street name.                                                                            |
| `postal_code`    | Postcode.                                                                               |
| `city`           | City / municipality name.                                                               |
| `county`         | Kreis / county (local-language).                                                        |
| `state`          | Federal state, in the local language (e.g., `Hessen`, `Bayern`, `Nordrhein-Westfalen`). |
| `country`        | ISO 3166-1 alpha-3 country code (`DEU`, `AUT`, `CHE`, …).                               |

Not every component is present for every entity — a `COUNTY` entity won't carry `house_number` / `street`, for example. Note that the entity endpoint returns local-language state names and ISO country codes; the Location Search endpoint returns English names for the same fields.

#### Coordinate

```json
"coordinate": { "latitude": 51.28119, "longitude": 9.48183 }
```

WGS-84 decimal degrees. For non-point entities (cities, counties, …) the coordinate is the centroid of the matched geography.

***

### Full response example

```json
{
  "fb_entity_id": "aeb56e1cd890051a9225a98e4149d331",
  "fb_datetime": "2026-05-11T15:42:32.027000+00:00",
  "fb_entity_version": "9b4b1301a7f220a24927d9ee8cd84225",
  "entity_type": "LOCATION",
  "entity_subtype": "LOCALITY",
  "updated_at": "2026-05-11T15:42:32.027000+00:00",
  "created_at": "2025-01-10T17:16:07.844000+00:00",
  "coordinate": {
    "latitude": 51.28119,
    "longitude": 9.48183
  },
  "location_level": null,
  "address_components": [
    { "component_type": "house_number", "component_value": "118" },
    { "component_type": "street",       "component_value": "Dennhäuser Straße" },
    { "component_type": "postal_code",  "component_value": "34134" },
    { "component_type": "city",         "component_value": "Kassel" },
    { "component_type": "county",       "component_value": "Kassel (Stadt)" },
    { "component_type": "state",        "component_value": "Hessen" },
    { "component_type": "country",      "component_value": "DEU" }
  ],
  "elevation": null,
  "alternative_names": [],
  "formatted_address": "Dennhäuser Straße 118, 34134 Kassel, DEU"
}
```

***

### Tips

* **Use `fb_entity_version` as a cache key.** It changes whenever any field on the entity changes, so a simple `ETag`-style check ("did I already fetch this version?") is enough to skip redundant work.
* **Three-call flow.** Search → Entity → Relation. Resolve `fb_entity_id` with Location Search, fetch the base record here, then fan out to any Location Relations you need.
* **`fb_entity_id` is the primary key.** It's stable across data refreshes — persist it instead of raw address strings, which can change with administrative renamings.
* **Locations are reused.** Organization addresses and person home locations both reference Location entities by `fb_entity_id`. Persist the location once and join from multiple places.
