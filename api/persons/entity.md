---
description: Fetch the canonical profile of one person by their Fusionbase entity ID.
---

# Entity

The Person Entity endpoint returns the canonical, structured profile of a single person — name, home location, birth date — keyed by their Fusionbase entity ID (`fb_entity_id`). This is the "base record" you fetch after locating a person with Person Search; the deeper, typed data slices (career history, network) are exposed separately via the Person Relations endpoint.

```
GET https://api.fusionbase.com/api/v2/entities/person/get/<FB_ENTITY_ID>
```

***

### Authentication

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Path parameters

| Parameter        | Required | Description                                                                                                                                                                           |
| ---------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<FB_ENTITY_ID>` | yes      | The Fusionbase entity ID of the person. Get it from the Person Search endpoint — it's the `fb_entity_id` field on every search result. Stable across data refreshes; safe to persist. |

***

### Example request

```bash
curl 'https://api.fusionbase.com/api/v2/entities/person/get/29db47808ed652004d14aaf9bdaea6d3' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### Response

The endpoint returns a single JSON object representing the person. Optional fields may be `null` or absent depending on data availability and on the access tier of your API key (date of birth is paywalled on some plans).

#### Top-level fields

| Field               | Type              | Description                                                                                    |
| ------------------- | ----------------- | ---------------------------------------------------------------------------------------------- |
| `fb_entity_id`      | string            | Stable Fusionbase identifier — matches the request path.                                       |
| `fb_datetime`       | ISO 8601 datetime | Server-side timestamp for this snapshot.                                                       |
| `fb_entity_version` | string            | Content-hash of the entity record. Changes whenever any field changes — useful as a cache key. |
| `entity_type`       | string            | Always `"PERSON"` on this endpoint.                                                            |
| `entity_subtype`    | string            | `"INDIVIDUAL"`.                                                                                |
| `created_at`        | ISO 8601 datetime | When Fusionbase first ingested this entity.                                                    |
| `updated_at`        | ISO 8601 datetime | When the entity record was last updated.                                                       |
| `name`              | object            | See Name.                                                                                      |
| `birth_date`        | object \| null    | See Birth date. Access may be restricted depending on your API tier.                           |
| `locations`         | object            | See Locations.                                                                                 |
| `source`            | object            | `{ "id": "data_sources/<source_key>" }` — the data source from which this record originates.   |

#### Name

```json
"name": {
  "given": "Timotheus",
  "family": "Höttges",
  "maiden": null,
  "aliases": ["Höttges, Timotheus"]
}
```

| Field     | Type           | Description                                                  |
| --------- | -------------- | ------------------------------------------------------------ |
| `given`   | string         | First / given name as registered.                            |
| `family`  | string         | Last / family name as registered.                            |
| `maiden`  | string \| null | Maiden name where disclosed.                                 |
| `aliases` | string\[]      | Other spellings or order variants (e.g., `"Family, Given"`). |

#### Birth date

```json
"birth_date": {
  "value": "1962-09-18T00:00:00+00:00",
  "is_month": false
}
```

| Field      | Type              | Description                                                                                                                               |
| ---------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `value`    | ISO 8601 datetime | The day of birth. May be paywalled depending on your API tier.                                                                            |
| `is_month` | boolean           | `true` when only month-and-year are known (the `value` then represents the first day of that month); `false` when the exact day is known. |

#### Locations

```json
"locations": {
  "home": {
    "fb_entity_id": "c4b48be985c887c63ad0150c1d85be23",
    "fb_datetime": "2026-04-01T16:54:38.709000+00:00",
    "fb_entity_version": "177f17c55eec963833d99698d16ceb69",
    "entity_type": "LOCATION",
    "entity_subtype": "CITY_POSTAL_CODE",
    "updated_at": "2026-04-01T16:54:38.709000+00:00",
    "created_at": "2024-11-23T06:53:59.124000+00:00",
    "extra": null,
    "coordinate": { "latitude": 50.73181, "longitude": 7.09888 },
    "location_level": null,
    "address_components": [
      { "component_type": "postal_code", "component_value": "53113" },
      { "component_type": "city",        "component_value": "Bonn" },
      { "component_type": "county",      "component_value": "Bonn" },
      { "component_type": "state",       "component_value": "Nordrhein-Westfalen" },
      { "component_type": "country",     "component_value": "DEU" }
    ],
    "elevation": null,
    "alternative_names": [],
    "formatted_address": "53113 Bonn, DEU"
  }
}
```

`locations.home` is a typed LOCATION entity (note the nested `fb_entity_id`, `fb_entity_version`, etc. — the location can be persisted and refreshed on its own). Full street addresses for natural persons are never published; coverage is limited to postal-code-level resolution (`entity_subtype: "CITY_POSTAL_CODE"`). The `country` component is the ISO 3166-1 alpha-3 code (`DEU`, `AUT`, …); the `state` value is the local-language name.

| Field                  | Description                                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `fb_entity_id`         | Stable identifier of the location. Persist this if you want to deduplicate persons living at the same postal code. |
| `entity_subtype`       | `CITY_POSTAL_CODE`, `CITY_NO_POSTAL_CODE`, etc.                                                                    |
| `coordinate`           | Latitude / longitude (approximate centroid of the postal area).                                                    |
| `address_components[]` | Flat list of `component_type` / `component_value` pairs — `postal_code`, `city`, `county`, `state`, `country`.     |
| `formatted_address`    | Single-line rendering ready for UI.                                                                                |
| `alternative_names`    | Other names by which the place is known. Often empty.                                                              |

***

### Tips

* **Use `fb_entity_version` as a cache key.** It changes whenever any field on the entity changes, so a simple `ETag`-style check ("did I already fetch this version?") is enough to skip redundant work.
* **Three-call flow.** Search → Entity → Relation. Resolve `fb_entity_id` with Person Search, fetch the base record here, then fan out to any Person Relations you need.
* **`fb_entity_id` is the primary key.** It's stable across data refreshes — persist it instead of name + birthdate combinations.
* **Locations.** The home location is itself an entity with its own `fb_entity_id` — persist that if you need a stable reference to the place of residence (e.g., for deduplication).
