---
description: >-
  Drill into typed data slices — management, financials, news, network — for one
  organization.
---

# Relations

Relations expose **typed data features** about a single organization — the people running it, its shareholders, balance sheets, news mentions, neighbouring companies, and more. Each slice is a separate relation identified by a stable `<RELATION_ID>`. You combine that ID with the `<ENTITY_ID>` (the `fb_entity_id` returned by Organization Search or the Base Entity endpoint) to fetch the slice for a specific company.

Browse the full set of organization-related relations under the **Unternehmen / Company** category of the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data).

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

| Parameter       | Required | Description                                                                                                                                        |
| --------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<RELATION_ID>` | yes      | Stable identifier of the relation type. See Available organization relations below for the full catalog.                                           |
| `<ENTITY_ID>`   | yes      | The Fusionbase entity ID of the organization. Get it from the Organization Search endpoint — it's the `fb_entity_id` field on every search result. |

**Optional relation parameters**

Some relations accept additional parameters (for example, **Financial Reports** can be narrowed to a specific fiscal year and document type). Send them **nested inside a top-level `params` object** in the **request body**, with `Content-Type: application/json`:

```http
POST /api/v2/relation/resolve/6322183029/110fe0da2f84c8d3174ec7bfd1f0f15a
X-API-KEY: YOUR_API_KEY
Content-Type: application/json

{
  "params": {
    "reference_year": "2023",
    "document_type": "Jahresabschluss"
  }
}
```

When a relation takes no parameters, the body can be omitted (or sent as an empty `{}` / `{"params": {}}`). Each relation's API Playground in the Data Hub lists the parameters it accepts.

***

#### Available organization relations

Each row links to the same slice you see on an organization's detail page. Relation IDs are stable; the slugs in the example URLs are display-only and may be localized.

**People & network**

| Relation                            | ID           | Description                                                                                                 |
| ----------------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------- |
| **Management**                      | `5906082026` | Current and former managing directors, board members, prokurists. Includes start/end dates and role labels. |
| **Network**                         | `3138484719` | Graph of related entities (people, organizations) up to N hops out, with `depth` per link.                  |
| **Shareholders**                    | `7326258463` | Shareholders per the commercial register, with stake percentages where disclosed.                           |
| **Shareholdings**                   | `9136475649` | The organizations this company holds stakes in.                                                             |
| **Ultimate Beneficial Owner (UBO)** | `9136468925` | UBOs disclosed under the German Transparenzregister regime.                                                 |

**Financials**

| Relation                       | ID           | Description                                                                                                                                                                                               |
| ------------------------------ | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Financial KPIs**             | `5601746763` | Headline financial metrics (revenue, net income, total assets, employees) per fiscal year.                                                                                                                |
| **Balance Sheets**             | `5601746108` | Itemised balance sheet accounts per fiscal year.                                                                                                                                                          |
| **Profit and Loss Statements** | `5601734199` | Itemised P\&L per fiscal year, hierarchical (`children` carry nested line items).                                                                                                                         |
| **Financial Reports**          | `6322183029` | Full Bundesanzeiger filings — Jahresabschluss / Konzernabschluss. Sanitised HTML body plus metadata. Accepts a body of `{"params":{"reference_year":"…","document_type":"…"}}` to pick a specific filing. |

**Public records**

| Relation                          | ID           | Description                                                                  |
| --------------------------------- | ------------ | ---------------------------------------------------------------------------- |
| **Insolvency Announcements**      | `4655345278` | Insolvency proceedings from the official insolvency notices.                 |
| **History**                       | `5990427555` | Commercial-register publication timeline — every official change ever filed. |
| **Commercial Register Documents** | `6391320624` | Downloadable register documents (Handelsregister-Auszüge etc.).              |
| **News & Media**                  | `5162071547` | News mentions and media coverage tied to the organization.                   |

**Discovery**

| Relation             | ID           | Description                                                                                     |
| -------------------- | ------------ | ----------------------------------------------------------------------------------------------- |
| **Companies nearby** | `8246474913` | Other registered organizations within a geographic radius of this company's registered address. |

***

#### Example requests

Fetch the **Management** relation for ADOS GmbH (`fb_entity_id = 75e8887e25587ed64cc8e1733a6c7160`):

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/5906082026/75e8887e25587ed64cc8e1733a6c7160' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

Fetch the **Financial Reports** relation for a specific fiscal year + document type (note the `params` wrapper):

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/6322183029/110fe0da2f84c8d3174ec7bfd1f0f15a' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"params":{"reference_year":"2023","document_type":"Jahresabschluss"}}'
```

Walk the **Network** graph for the same company:

```bash
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/3138484719/75e8887e25587ed64cc8e1733a6c7160' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

***

#### Response

All relation endpoints return the same envelope. The shape of `entity.value` varies per relation — see each relation's API Playground in the Data Hub for its exact schema.

```json
[
  {
    "label": "ENTITY_NETWORK",
    "entity_from": {
      "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
      "entity_type": "ORGANIZATION",
      "display_name": "ADOS GmbH"
    },
    "entity": {
      "entity_type": "FEATURE",
      "entity_subtype": "NETWORK",
      "fb_semantic_id": "feature|network|organization:75e8887e25587ed64cc8e1733a6c7160",
      "value": {
        "root": {
          "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
          "entity_type": "ORGANIZATION",
          "display_name": "ADOS GmbH"
        },
        "links": [
          {
            "depth": 1,
            "relation_id": "135ff9ab592bb1b41ab948b3ad95f91f",
            "label": "MANAGING_DIRECTOR",
            "entity_from": {
              "fb_entity_id": "8a00ab6520e0b64eb83345586ba1613e",
              "entity_type": "PERSON",
              "display_name": "Herbert Rütgers"
            },
            "entity_to": {
              "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
              "entity_type": "ORGANIZATION",
              "display_name": "ADOS GmbH"
            },
            "meta": { "start_date": "2002-06-03", "end_date": "2020-07-06" }
          }
        ]
      }
    }
  }
]
```

**Envelope fields**

| Field                   | Description                                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `label`                 | Type of the top-level relation (e.g., `ENTITY_NETWORK`, `MANAGEMENT`, `FINANCIALS_KPI`). Matches the relation's catalog entry. |
| `entity_from`           | The organization the relation was queried for — echoes your `fb_entity_id`.                                                    |
| `entity.entity_type`    | Always `FEATURE` for typed organization relations.                                                                             |
| `entity.entity_subtype` | Specific feature subtype (e.g., `NETWORK`, `BALANCE_SHEET`, `PROFIT_AND_LOSS`).                                                |
| `entity.fb_semantic_id` | Stable identifier for the feature instance (typically `feature\|<subtype>\|organization:<id>`).                                |
| `entity.value`          | The actual payload. Shape depends on the relation; see the per-relation schema in the API Playground.                          |

**Common payload shapes**

* **Network** (`3138484719`) — `value.root` + `value.links[]` with `depth`, `relation_id`, `label`, `entity_from`, `entity_to`, `meta`. Walks outward from the queried entity.
* **Management / Shareholders / UBO** — `value` is an array (or `{current,past}` split) of person/organization references with role + date metadata.
* **Financial KPIs / Balance Sheets / Profit and Loss** — `value` is a per-year array. Balance sheets and P\&L each carry hierarchical `profit_and_loss_accounts` / `balance_sheet_accounts` arrays with nested `children`.
* **Financial Reports** — `value.report_html` (cleaned HTML body), `value.metadata` (document type, fiscal year, publication date), and a `signup_hint` when the caller's plan doesn't grant access to the latest filing.
* **News & Media** — `value` is a list of items with `headline`, `url`, `published_at`, `source`.
* **Insolvency Announcements / History / Commercial Register Documents** — list of dated, source-anchored records.
* **Companies nearby** — list of organization references with `distance_meters`.

***

#### Tips

* **Cache by relation × entity.** Each `(relation_id, fb_entity_id)` pair is cacheable on the client. Use the `fb_semantic_id` from the response as a stable cache key.
* **Pair with Organization Search.** Search first to resolve `fb_entity_id`, then fan out to the relations you need.
* **Browse the catalog first.** The [Data Hub catalog](https://app.fusionbase.com/browse-data) lets you preview every organization relation interactively before wiring it into your code. Each relation also has an API Playground page that shows the exact response schema for a real example entity.
