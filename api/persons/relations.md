---
description: Drill into typed data slices — career history, network — for one person.
---

# Relations

Relations expose **typed data features** about a single person — every management role they've held over time, and the network of organizations and other people connected to them. Each slice is a separate relation identified by a stable `<RELATION_ID>`. You combine that ID with the `<ENTITY_ID>` (the `fb_entity_id` returned by Person Search or Person Entity) to fetch the slice for a specific person.

Browse the full set of person-related relations under the **Unternehmen / Company** category of the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data) — person relations sit alongside organization relations there.

```
POST https://api.fusionbase.com/api/v2/relation/resolve/<RELATION_ID>/<ENTITY_ID>
```

***

### Authentication

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Path parameters

| Parameter       | Required | Description                                                                                                                            |
| --------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `<RELATION_ID>` | yes      | Stable identifier of the relation type. See Available person relations below for the full catalog.                                     |
| `<ENTITY_ID>`   | yes      | The Fusionbase entity ID of the person. Get it from the Person Search endpoint — it's the `fb_entity_id` field on every search result. |

#### Optional relation parameters

Some relations accept additional parameters. Send them as a JSON object in the **request body**, with `Content-Type: application/json`:

```http
POST /api/v2/relation/resolve/<RELATION_ID>/<ENTITY_ID>
X-API-KEY: <YOUR_API_KEY>
Content-Type: application/json

{ /* relation-specific params */ }
```

When a relation takes no parameters, the body can be omitted (or an empty `{}`). Each relation's API Playground in the Data Hub lists the parameters it accepts.

***

### Available person relations

Relation IDs are stable; the slugs in any example URLs are display-only and may be localized.

| Relation    | ID           | Description                                                                                                                                                                   |
| ----------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **History** | `5990434853` | Career timeline — every management role this person has held, with the linked organization, role label (managing director, board member, prokurist, …) and start / end dates. |
| **Network** | `2545866067` | Graph of connected entities — the organizations this person is or was involved with, plus other people sharing those organizations. Walks outward with `depth` per link.      |

***

### Example requests

Fetch the **History** relation for Timotheus Höttges (`fb_entity_id = 29db47808ed652004d14aaf9bdaea6d3`):

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/5990434853/29db47808ed652004d14aaf9bdaea6d3' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

Walk the **Network** graph for the same person:

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/2545866067/29db47808ed652004d14aaf9bdaea6d3' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### Response

All relation endpoints return the same envelope. The shape of `entity.value` varies per relation — see each relation's API Playground in the Data Hub for its exact schema.

```json
[
  {
    "label": "ENTITY_NETWORK",
    "entity_from": {
      "fb_entity_id": "29db47808ed652004d14aaf9bdaea6d3",
      "entity_type": "PERSON",
      "display_name": "Timotheus Höttges"
    },
    "entity": {
      "entity_type": "FEATURE",
      "entity_subtype": "NETWORK",
      "fb_semantic_id": "feature|network|person:29db47808ed652004d14aaf9bdaea6d3",
      "value": {
        "root": {
          "fb_entity_id": "29db47808ed652004d14aaf9bdaea6d3",
          "entity_type": "PERSON",
          "display_name": "Timotheus Höttges"
        },
        "links": [
          {
            "depth": 1,
            "relation_id": "135ff9ab592bb1b41ab948b3ad95f91f",
            "label": "MANAGING_DIRECTOR",
            "entity_from": {
              "fb_entity_id": "29db47808ed652004d14aaf9bdaea6d3",
              "entity_type": "PERSON",
              "display_name": "Timotheus Höttges"
            },
            "entity_to": {
              "fb_entity_id": "<organization_id>",
              "entity_type": "ORGANIZATION",
              "display_name": "Deutsche Telekom AG"
            },
            "meta": { "start_date": "2014-01-01", "end_date": null }
          }
        ]
      }
    }
  }
]
```

#### Envelope fields

| Field                   | Description                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| `label`                 | Type of the top-level relation (e.g., `ENTITY_NETWORK`, `PUBLICATIONS`).                              |
| `entity_from`           | The person the relation was queried for — echoes your `fb_entity_id`.                                 |
| `entity.entity_type`    | Always `FEATURE` for typed person relations.                                                          |
| `entity.entity_subtype` | Specific feature subtype (e.g., `NETWORK`, `PUBLICATIONS`).                                           |
| `entity.fb_semantic_id` | Stable identifier for the feature instance (typically `feature\|<subtype>\|person:<id>`).             |
| `entity.value`          | The actual payload. Shape depends on the relation; see the per-relation schema in the API Playground. |

#### Payload shapes

* **History** (`5990434853`) — list of publication / role records, each carrying the linked organization, role label, and date range. Use this to reconstruct a person's complete public career timeline.
* **Network** (`2545866067`) — `value.root` + `value.links[]` with `depth`, `relation_id`, `label`, `entity_from`, `entity_to`, `meta`. Walks outward from the queried person to organizations and onward to other people sharing those organizations.

***

### Tips

* **Cache by relation × entity.** Each `(relation_id, fb_entity_id)` pair is cacheable on the client. Use the `fb_semantic_id` from the response as a stable cache key.
* **Pair with Person Search.** Search first to resolve `fb_entity_id`, then fan out to the relations you need.
* **Pivot to organizations.** The History and Network relations both return `entity_to` references to organizations — follow those `fb_entity_id`s into the Organization Entity and Organization Relations endpoints to drill into the company side of the connection.
* **Browse the catalog first.** The [Data Hub catalog](https://app.fusionbase.com/browse-data) lets you preview every person relation interactively before wiring it into your code.
