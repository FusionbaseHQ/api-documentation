---
description: Fetch the canonical profile of one organization by its Fusionbase entity ID.
---

# Entity

The Organization Entity endpoint returns the canonical, structured profile of a single organization — name, address, legal registration, classifications, contact details and status — keyed by its Fusionbase entity ID (`fb_entity_id`). This is the "base record" you fetch after locating an organization with Organization Search; the deeper, typed data slices (financials, management, network, …) are exposed separately via the Organization Relations endpoint.

```
GET https://api.fusionbase.com/api/v2/entities/organization/get/<FB_ENTITY_ID>
```

***

### Authentication

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Path parameters

| Parameter        | Required | Description                                                                                                                                                                                       |
| ---------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<FB_ENTITY_ID>` | yes      | The Fusionbase entity ID of the organization. Get it from the Organization Search endpoint — it's the `fb_entity_id` field on every search result. Stable across data refreshes; safe to persist. |

***

### Example request

```bash
curl 'https://api.fusionbase.com/api/v2/entities/organization/get/75e8887e25587ed64cc8e1733a6c7160' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### Response

The endpoint returns a single JSON object representing the organization. The full response is rich — every meaningful field is included below. Optional fields may be `null` or absent depending on data availability for the specific company.

#### Top-level fields

| Field               | Type                      | Description                                                                                                                                            |
| ------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `fb_entity_id`      | string                    | Stable Fusionbase identifier — matches the request path.                                                                                               |
| `fb_datetime`       | ISO 8601 datetime         | Server-side timestamp for this snapshot.                                                                                                               |
| `fb_entity_version` | string                    | Content-hash of the entity record. Changes whenever any field changes — useful as a cache key.                                                         |
| `fb_semantic_id`    | string                    | Human-meaningful identifier derived from registry + type + number, e.g., `organization\|corporation\|...\|registration_number:5\|...`.                 |
| `entity_type`       | string                    | Always `"ORGANIZATION"` on this endpoint.                                                                                                              |
| `entity_subtype`    | string                    | `CORPORATION`, `PARTNERSHIP`, `COOPERATIVE`, `ASSOCIATION`, `SOLE_TRADER`, `PUBLIC_LAW`, …                                                             |
| `created_at`        | ISO 8601 datetime         | When Fusionbase first ingested this entity.                                                                                                            |
| `updated_at`        | ISO 8601 datetime         | When the entity record was last updated.                                                                                                               |
| `external_ids`      | object                    | Map of source-system identifiers, e.g., `fb_german_business_registry_entity_id`.                                                                       |
| `status`            | object                    | See Status.                                                                                                                                            |
| `name`              | string                    | Official legal name.                                                                                                                                   |
| `other_names`       | string\[]                 | Historic / alternate names.                                                                                                                            |
| `description`       | object                    | Source-grouped descriptions, keyed by locale (`description.registry.de`, `description.registry.en`, …). Usually carries the registered business scope. |
| `address`           | object                    | Primary registered address. See Address.                                                                                                               |
| `other_addresses`   | object\[] \| null         | Additional known addresses (branches, mail addresses).                                                                                                 |
| `jurisdiction`      | object                    | `{ "iso_alpha3": "DEU" }` etc.                                                                                                                         |
| `contact`           | object                    | Websites and phone numbers, grouped by purpose (`primary`, `secondary`).                                                                               |
| `founding_date`     | ISO 8601 datetime \| null | Date the company was founded / first registered.                                                                                                       |
| `cessation_date`    | ISO 8601 datetime \| null | Date the company was dissolved / deregistered, if applicable.                                                                                          |
| `classifications`   | object                    | See Classifications.                                                                                                                                   |
| `legal`             | object                    | See Legal.                                                                                                                                             |
| `source`            | object                    | `{ "id": "data_sources/<source_key>" }` — the data source from which this record originates.                                                           |

#### Status

```json
"status": {
  "active": true,
  "status": "ACTIVE",
  "status_detail": null
}
```

| Field           | Description                                                                         |
| --------------- | ----------------------------------------------------------------------------------- |
| `active`        | Boolean shorthand — convenient for filtering.                                       |
| `status`        | Free-form status code: `ACTIVE`, `INACTIVE`, `DISSOLVED`, `MERGED`, `LIQUIDATED`, … |
| `status_detail` | Optional reason/explanation string.                                                 |

#### Address

```json
"address": {
  "fb_entity_id": "b69ea5ed95c054a3ad9b3d75335386a7",
  "coordinate": { "latitude": 50.7697654, "longitude": 6.1196758544025 },
  "address_components": [
    { "component_type": "house_number", "component_value": "23-25" },
    { "component_type": "street",       "component_value": "Trierer Straße" },
    { "component_type": "postal_code",  "component_value": "52078" },
    { "component_type": "city",         "component_value": "Aachen" },
    { "component_type": "county",       "component_value": "Städteregion Aachen" },
    { "component_type": "state",        "component_value": "North Rhine-Westphalia" },
    { "component_type": "country",      "component_value": "Germany" }
  ],
  "fb_semantic_id": "location|locality:germany:north_rhine-westphalia:52078:aachen:trierer_strasse:23-25",
  "formatted_address": "Trierer Straße 23-25, 52078 Aachen, Germany"
}
```

Address is itself a typed Fusionbase entity (note the `fb_entity_id` and `fb_semantic_id`). The flat `address_components[]` array gives you each piece of the postal address; `formatted_address` is the single-line rendering you typically display.

`component_type` values include `house_number`, `street`, `postal_code`, `city`, `county`, `state`, `country`. Not all components are present for every entity.

#### Contact

```json
"contact": {
  "websites":     { "primary": "https://www.ados.de/" },
  "phone_numbers": { "primary": "0241 97690" }
}
```

#### Classifications

```json
"classifications": {
  "web": [
    { "source": "Google", "value": { "de": "Hersteller elektronischer Geräte" } }
  ],
  "industry_codes": null,
  "legal_form": null
}
```

| Field            | Description                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| `web`            | Free-text classifications derived from web sources, each annotated with its `source`.                        |
| `industry_codes` | Structured codes from official taxonomies (e.g., WZ2025). Use these as filter values in Organization Search. |
| `legal_form`     | Structured legal-form code. See the Legal Forms reference.                                                   |

#### Legal

```json
"legal": {
  "registration_authority": {
    "local": {
      "registration_authority_id": "R3101",
      "registration_authority_name": "Aachen",
      "registration_authority_entity_id": "R3101 HRB 5",
      "registration_authority_entity_name": "Aachen HRB 5",
      "registration_type": "HRB",
      "registration_number": "5",
      "registration_id_extra": null,
      "registration_authority_location": { /* full LOCATION entity, abridged */ }
    }
  }
}
```

`registration_authority.local` is the entry you'll touch most often. Combine `registration_type`, `registration_authority_name` and `registration_number` to reproduce the canonical register reference (e.g., `Aachen HRB 5`).

| Field                                | Description                                                             |
| ------------------------------------ | ----------------------------------------------------------------------- |
| `registration_authority_id`          | Stable Fusionbase code for the registry court / authority.              |
| `registration_authority_name`        | Human-readable authority name (e.g., `Aachen`).                         |
| `registration_authority_entity_name` | Combined label, ready to display: `<Authority> <Type> <Number>`.        |
| `registration_type`                  | One of `HRA`, `HRB`, `GnR`, `GsR`, `PR`, `VR` — see Registration types. |
| `registration_number`                | The number under which the company is registered.                       |
| `registration_authority_location`    | Full nested LOCATION entity for the authority itself.                   |

***

### Full response example

<details>

<summary>Click to expand the complete payload for <code>ADOS GmbH</code></summary>

```json
{
  "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
  "fb_datetime": "2024-02-01T16:12:18.849000",
  "fb_entity_version": "55afcba0d74181bd508e0207e3fbddfc",
  "entity_type": "ORGANIZATION",
  "entity_subtype": "CORPORATION",
  "updated_at": "2024-02-01T16:12:18.849000",
  "created_at": "2023-11-24T10:30:02.502000",
  "external_ids": {
    "fb_german_business_registry_entity_id": "75e8887e25587ed64cc8e1733a6c7160"
  },
  "status": {
    "active": true,
    "status": "ACTIVE",
    "status_detail": null
  },
  "name": "ADOS GmbH",
  "other_names": [
    "Ados Gesellschaft mit beschränkter Haftung"
  ],
  "description": {
    "registry": {
      "de": "Gegenstand des Unternehmens ist die Herstellung und der Verkauf der unter dem Namen \"ADOS\" geschützten Artikel sowie der Erwerb, die Veräußerung, die Verwertung von Patenten und Erfinderrechten und die Herstellung, Anschaffung und der Vertrieb von anderen Gegenständen des Gewerbefleisses."
    }
  },
  "address": {
    "fb_entity_id": "b69ea5ed95c054a3ad9b3d75335386a7",
    "coordinate": { "latitude": 50.7697654, "longitude": 6.1196758544025 },
    "address_components": [
      { "component_type": "house_number", "component_value": "23-25" },
      { "component_type": "street", "component_value": "Trierer Straße" },
      { "component_type": "postal_code", "component_value": "52078" },
      { "component_type": "city", "component_value": "Aachen" },
      { "component_type": "county", "component_value": "Städteregion Aachen" },
      { "component_type": "state", "component_value": "North Rhine-Westphalia" },
      { "component_type": "country", "component_value": "Germany" }
    ],
    "fb_semantic_id": "location|locality:germany:north_rhine-westphalia:52078:aachen:trierer_strasse:23-25",
    "formatted_address": "Trierer Straße 23-25, 52078 Aachen, Germany"
  },
  "other_addresses": null,
  "jurisdiction": { "iso_alpha3": "DEU" },
  "contact": {
    "websites":     { "primary": "https://www.ados.de/" },
    "phone_numbers": { "primary": "0241 97690" }
  },
  "founding_date": "1961-02-20T00:00:00",
  "cessation_date": null,
  "classifications": {
    "web": [
      { "source": "Google", "value": { "de": "Hersteller elektronischer Geräte" } }
    ],
    "industry_codes": null,
    "legal_form": null
  },
  "source": { "id": "data_sources/1051122944" },
  "fb_semantic_id": "organization|corporation|registration_authority_id:r3101|registration_authority_name:r3101|registration_type:hrb|registration_number:5|registration_authority_location_id:2077c7f46e95a3740492448c2beb2011|source:data_sources/1051122944",
  "legal": {
    "registration_authority": {
      "local": {
        "registration_authority_id": "R3101",
        "registration_authority_name": "Aachen",
        "registration_authority_entity_id": "R3101 HRB 5",
        "registration_authority_entity_name": "Aachen HRB 5",
        "registration_type": "HRB",
        "registration_number": "5",
        "registration_id_extra": null,
        "registration_authority_location": {
          "fb_entity_id": "2077c7f46e95a3740492448c2beb2011",
          "entity_type": "LOCATION",
          "entity_subtype": "CITY_NO_POSTAL_CODE",
          "coordinate": { "latitude": 50.776351, "longitude": 6.083862 },
          "address_components": [
            { "component_type": "city", "component_value": "Aachen" },
            { "component_type": "county", "component_value": "Städteregion Aachen" },
            { "component_type": "state", "component_value": "North Rhine-Westphalia" },
            { "component_type": "country", "component_value": "Germany" }
          ],
          "fb_semantic_id": "location|city_no_postal_code:germany:north_rhine-westphalia:aachen",
          "formatted_address": "Aachen, Germany"
        }
      }
    }
  }
}
```

</details>

***

### Tips

* **Use `fb_entity_version` as a cache key.** It changes whenever any field on the entity changes, so a simple `ETag`-style check ("did I already fetch this version?") is enough to skip redundant work.
* **Three-call flow.** Search → Entity → Relation. Resolve `fb_entity_id` with Organization Search, fetch the base record here, then fan out to any Organization Relations you need.
* **`fb_entity_id` is the primary key.** It's stable across data refreshes — persist it instead of name + address combinations, which can change when a company moves or rebrands.
* **Address is itself an entity.** Persist `address.fb_entity_id` if you need a stable reference to the location (e.g., to deduplicate companies at the same building).
