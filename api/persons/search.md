---
description: Find people by name across the German business registry.
---

# Search

The Person Search endpoint is a specialised tool for retrieving detailed personal profiles from official business-registry sources. It returns ranked person results for a free-text query — typically a full name or a name fragment — along with each person's home location and the organizations they're affiliated with. Designed for business-intelligence, network-analysis and compliance use cases where role context matters as much as identity.

```
GET https://api.fusionbase.com/api/v2/search/entities/person
```

***

### Authentication

Send your Fusionbase API key in the `X-API-KEY` header on every request.

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Query parameters

| Parameter    | Type    | Default      | Description                                                                                          |
| ------------ | ------- | ------------ | ---------------------------------------------------------------------------------------------------- |
| `q`          | string  | `""`         | Free-text search query — usually the person's name or a fragment. URL-encode it.                     |
| `limit`      | integer | `10`         | Maximum number of results to return per page.                                                        |
| `skip`       | integer | `0`          | Number of results to skip — pagination offset.                                                       |
| `source_key` | string  | `1051122944` | Restricts results to a specific data source. `1051122944` is the German business registry (default). |

#### Minimal example

```
GET /api/v2/search/entities/person?q=Timotheus%20H%C3%B6ttges
```

#### Paginated example

```
GET /api/v2/search/entities/person?q=Schmidt&limit=25&skip=50
```

***

### Response

Each result carries the person's identity, their home location and the organizations they're affiliated with — all inline, no follow-up calls required for a quick read.

```json
{
  "results": [
    {
      "entity_type": "PERSON",
      "entity": {
        "fb_entity_id": "29db47808ed652004d14aaf9bdaea6d3",
        "entity_type": "PERSON",
        "entity_subtype": "INDIVIDUAL",
        "name": {
          "given": "Timotheus",
          "family": "Höttges"
        },
        "locations": {
          "home": {
            "formatted_address": "53113 Bonn, DEU"
          }
        },
        "relations": {
          "ORGANIZATION": [
            {
              "label": "MANAGING_DIRECTOR",
              "entity_to": {
                "fb_entity_id": "ff525267e6ff5f67a6dbe0af29b7e5cc",
                "display_name": "Deutsche Telekom AG"
              },
              "meta": {
                "start_date": "2014-01-01"
              }
            }
          ]
        },
        "source_id": "data_sources/1051122944"
      },
      "score": 19.639364
    }
  ],
  "total": 1
}
```

#### Fields

| Field                                               | Description                                                                                                                                |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `results[]`                                         | Array of matched people, ordered by `score` descending.                                                                                    |
| `results[].entity_type`                             | Always `"PERSON"` for this endpoint (duplicated inside the `entity` block).                                                                |
| `results[].entity.fb_entity_id`                     | **Stable Fusionbase entity identifier.** Use this to fetch the full entity, attach Person Relations, or to reference the person elsewhere. |
| `results[].entity.entity_subtype`                   | `"INDIVIDUAL"`.                                                                                                                            |
| `results[].entity.name.given`                       | First / given name.                                                                                                                        |
| `results[].entity.name.family`                      | Last / family name.                                                                                                                        |
| `results[].entity.locations.home.formatted_address` | Home city or registered place of residence, single line. Use it to disambiguate common names without a follow-up Entity call.              |
| `results[].entity.relations.ORGANIZATION[]`         | Inline list of the organizations this person is affiliated with — see Inline organization affiliations.                                    |
| `results[].entity.source_id`                        | Data source the record was loaded from (e.g., `"data_sources/1051122944"` for the German business registry).                               |
| `results[].score`                                   | Relevance score. Higher is more relevant. Comparable only within a single response.                                                        |
| `total`                                             | Total number of matching people across all pages — use it to compute the last page (`Math.ceil(total / limit)`).                           |

The `fb_entity_id` is the primary key you should persist if you reference the person later — it's stable across data refreshes.

#### Inline organization affiliations

The `entity.relations.ORGANIZATION[]` array is what makes this endpoint a one-shot for due-diligence and people-search UIs — every match comes with the organizations the person is or was tied to:

| Field                    | Description                                                                                                                      |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `label`                  | Role the person held in that organization — e.g., `MANAGING_DIRECTOR`, `BOARD_MEMBER`, `PROKURIST`, `LIQUIDATOR`, `SHAREHOLDER`. |
| `entity_to.fb_entity_id` | The organization's Fusionbase entity ID. Pivot into Organization Entity or Organization Relations from here.                     |
| `entity_to.display_name` | The organization's name, ready to display.                                                                                       |
| `meta.start_date`        | When the role started (ISO 8601 date).                                                                                           |
| `meta.end_date`          | When the role ended, if it has ended (ISO 8601 date or absent).                                                                  |

***

### Pagination

Iterate by combining `limit` and `skip`. `total` in the response tells you when you've reached the end.

```
# page 1
GET /api/v2/search/entities/person?q=Schmidt&limit=25&skip=0
# page 2
GET /api/v2/search/entities/person?q=Schmidt&limit=25&skip=25
```

***

### Examples

#### 1. Plain name search

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/person' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=Timotheus Höttges'
```

#### 2. Partial-name lookup

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/person' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=Höttges' \
  --data-urlencode 'limit=25'
```

***

### Tips

* **Affiliations come inline.** `entity.locations.home.formatted_address` plus `entity.relations.ORGANIZATION[]` give you enough context to disambiguate common names and build a people-search UI without any follow-up call.
* **Pivot directly to companies.** Each affiliation carries the organization's `fb_entity_id` — drop straight into the Organization Entity or Organization Relations endpoints without a second lookup.
* **Stable IDs.** `fb_entity_id` is stable across data refreshes — persist it instead of name + birthdate combinations.
* **Filters.** Person search is a thin name-based lookup; for advanced structural filters (industry, registration, financials), search the company first via Organization Search and then walk to the people via the company's Management relation.
