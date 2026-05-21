---
description: Geocode an address or look up a place by name.
---

# Search

The Location Search endpoint is a comprehensive solution for place searches and address geocoding — akin to Google Maps Places Search. Use it to plot points of interest or transform free-text addresses into WGS-84 coordinates with map-ready results.

> **Not designed for typeahead.** The endpoint expects a reasonably complete query (a full address, postal code, or place name). Very short or partial fragments (e.g., a single letter, just a street prefix) return no results. If you need keystroke-level autocomplete, build a UI that triggers the search only after the user has typed a meaningful chunk (e.g., a complete street name or postal code).

```
GET https://api.fusionbase.com/api/v2/search/entities/location
```

***

### Authentication

Send your Fusionbase API key in the `X-API-KEY` header on every request.

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Query parameters

| Parameter | Type   | Description                                                                                                                                                       |
| --------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `q`       | string | Free-text search query — address, city, postal code, neighbourhood, building name. URL-encode it. Must be reasonably complete; short fragments return no results. |

#### Minimal example

```
GET /api/v2/search/entities/location?q=Agnes-Pockels-Bogen%201%2C%2080992%20M%C3%BCnchen
```

***

### Response

The endpoint returns a `results` array. Each result carries the **full Location entity** inline — the same shape you get back from the Location Entity endpoint — so you typically don't need a second call just to read coordinates or parsed address components after a search.

```json
{
  "results": [
    {
      "entity": {
        "fb_entity_id": "45d488830421ebc1d9130cc0856b3c5d",
        "fb_datetime": "2023-12-18T16:17:53.570000",
        "fb_entity_version": "9cefd42e59f49e3de4b0b33f983b740c",
        "entity_type": "LOCATION",
        "updated_at": "2023-12-18T16:17:53.570000",
        "created_at": "2023-11-09T16:17:51.381000",
        "external_ids": {
          "nominatim": {
            "osm_id": 1787145076,
            "osm_type": "node",
            "class": "amenity"
          }
        },
        "coordinate": {
          "latitude": 48.1735835,
          "longitude": 11.5323446
        },
        "location_level": null,
        "address_components": [
          { "component_type": "house_number", "component_value": "1" },
          { "component_type": "street",       "component_value": "Agnes-Pockels-Bogen" },
          { "component_type": "postal_code",  "component_value": "80992" },
          { "component_type": "city",         "component_value": "Munich" },
          { "component_type": "state",        "component_value": "Bavaria" },
          { "component_type": "country",      "component_value": "Germany" }
        ],
        "alternative_names": [],
        "fb_semantic_id": "location|locality:germany:bavaria:80992:munich:agnes-pockels-bogen:1",
        "formatted_address": "Agnes-Pockels-Bogen 1, 80992 Munich, Germany"
      },
      "entity_type": "LOCATION"
    }
  ]
}
```

#### Fields

| Field                                        | Description                                                                                                                                                 |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `results[]`                                  | Array of matched locations, ordered by match strength.                                                                                                      |
| `results[].entity_type`                      | Always `"LOCATION"` for this endpoint.                                                                                                                      |
| `results[].entity.fb_entity_id`              | **Stable Fusionbase entity identifier.** Use this to fetch the entity by ID, to attach Location Relations, or to reference the place elsewhere.             |
| `results[].entity.fb_datetime`               | Server-side timestamp for this snapshot.                                                                                                                    |
| `results[].entity.fb_entity_version`         | Content-hash of the entity record. Useful as a cache key.                                                                                                   |
| `results[].entity.fb_semantic_id`            | Human-meaningful identifier built from administrative path + street + house number.                                                                         |
| `results[].entity.created_at` / `updated_at` | When Fusionbase first ingested / last refreshed this entity.                                                                                                |
| `results[].entity.external_ids`              | Source-system identifiers — typically `nominatim` `{ osm_id, osm_type, class }`.                                                                            |
| `results[].entity.coordinate`                | WGS-84 decimal degrees: `{ latitude, longitude }`. For non-point matches this is the centroid.                                                              |
| `results[].entity.address_components[]`      | Flat list of parsed pieces — `house_number`, `street`, `postal_code`, `city`, `county`, `state`, `country`. Not every component is present for every match. |
| `results[].entity.formatted_address`         | Single-line rendering ready for UI.                                                                                                                         |
| `results[].entity.alternative_names`         | Other names by which the place is known. Often empty.                                                                                                       |
| `results[].entity.location_level`            | Administrative level when applicable (e.g., `KRE` for Kreis). Often `null` for building-level matches.                                                      |

The `fb_entity_id` is the primary key you should persist if you reference the location later — it's stable across data refreshes.

State and country values are returned as their **English names** (`"Bavaria"`, `"Germany"`), not ISO codes.

***

### Examples

#### 1. Full street address → geocode

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/location' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=Agnes-Pockels-Bogen 1, 80992 München'
```

#### 2. Postal code lookup

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/location' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=80992'
```

#### 3. City-level lookup

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/location' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=München'
```

***

### Tips

* **The search returns the full entity inline.** You usually don't need a follow-up call to Location Entity — read `coordinate`, `address_components`, `formatted_address` straight off the search result.
* **Stable IDs.** `fb_entity_id` is stable across data refreshes — persist it instead of raw address strings.
* **Query specificity.** Short fragments and very incomplete strings return empty results. Submit reasonably complete inputs — a full address, postal code, or place name — rather than wiring this to every keystroke.
* **Coordinates.** Coordinates are WGS-84 decimal degrees (`(latitude, longitude)` — note the order). For non-point matches the coordinate is the centroid of the matched geography.
