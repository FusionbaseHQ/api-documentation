---
description: >-
  Find companies by name and rich structured filters — status, location,
  registration, industry, size, financials.
---

# Search

The Organization Search endpoint returns ranked organization results for a free-text query, optionally constrained by a rich filter set covering status, location, registration, industry, company size, and financials.

```
GET https://api.fusionbase.com/api/v2/search/entities/organization
```

***

### Authentication

Send your Fusionbase API key in the `X-API-KEY` header on every request.

```http
X-API-KEY: <YOUR_API_KEY>
```

***

### Query parameters

| Parameter    | Type          | Default      | Description                                                                                                                                                   |
| ------------ | ------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `q`          | string        | `""`         | Free-text search query (organization name, fragments, ID, etc.). URL-encode it. Empty string is valid and returns the full filtered set.                      |
| `limit`      | integer       | `10`         | Maximum number of results to return per page.                                                                                                                 |
| `skip`       | integer       | `0`          | Number of results to skip — pagination offset.                                                                                                                |
| `source_key` | string        | `1051122944` | Restricts results to a specific data source. `1051122944` is the German business registry (default). Pass another source key to broaden or change the corpus. |
| `filters`    | string (JSON) | —            | URL-encoded JSON object describing structured filters. See Filters. When omitted, no structural constraints are applied.                                      |

#### Minimal example

```
GET /api/v2/search/entities/organization?q=OroraTech%20GmbH
```

#### Paginated example

```
GET /api/v2/search/entities/organization?q=Siemens&limit=25&skip=50
```

***

### Filters

The `filters` parameter accepts a single JSON object. Pass it as a URL-encoded string:

```
filters=%7B%22active%22%3Atrue%2C%22city%22%3A%22Munich%22%7D
```

That decodes to:

```json
{ "active": true, "city": "Munich" }
```

All filter fields are optional. Unknown fields are ignored. String comparisons are case-insensitive unless otherwise noted. Multiple filter fields are combined with logical AND.

#### Top-level fields

| Field                         | Type                   | Example                 | Notes                                                                                                                               |
| ----------------------------- | ---------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `active`                      | boolean                | `true`                  | Whether the organization is currently active. Omit to include both active and inactive.                                             |
| `bs_company_size_category`    | enum                   | `"medium"`              | One of `micro`, `small`, `medium`, `large` (EU recommendation 2003/361/EC). See Company size categories.                            |
| `postal_code`                 | string                 | `"80331"`               | Exact postal code match.                                                                                                            |
| `city`                        | string                 | `"Munich"`              | City name match.                                                                                                                    |
| `state`                       | string                 | `"Bavaria"`             | Federal state / first-level administrative region.                                                                                  |
| `street`                      | string                 | `"Sankt-Martin-Straße"` | Street name match.                                                                                                                  |
| `house_number`                | string                 | `"112"`                 | House number match.                                                                                                                 |
| `legal_form_code`             | string                 | `"GmbH"`                | Legal-form code from the Legal forms enumeration.                                                                                   |
| `registration_type`           | enum                   | `"HRB"`                 | One of `HRA`, `HRB`, `GnR`, `GsR`, `PR`, `VR`. See Registration types.                                                              |
| `registration_authority_name` | string                 | `"München"`             | Registry court (or other authority) name.                                                                                           |
| `registration_number`         | string                 | `"243843"`              | Registration number at the authority. Combine with `registration_authority_name` and `registration_type` for a unique entity match. |
| `registration_date_from`      | string (ISO 8601 date) | `"2010-01-01"`          | Lower bound (inclusive) on the original registration date.                                                                          |
| `registration_date_to`        | string (ISO 8601 date) | `"2024-12-31"`          | Upper bound (inclusive) on the original registration date.                                                                          |
| `industry_code`               | string                 | `"62.01"`               | WZ2025 industry classification code (German industry taxonomy). Requires `industry_scheme`.                                         |
| `industry_scheme`             | string                 | `"WZ2025"`              | Currently only `WZ2025` is supported. Must be set whenever `industry_code` is set.                                                  |
| `financial_filters`           | object                 | see below               | Nested ranges for headcount / P\&L / balance-sheet metrics. See Financial filters.                                                  |

#### Financial filters

Nested under `financial_filters`. Each metric is an object with optional `gte` (≥) and/or `lte` (≤) numeric bounds. Either bound can be omitted to make the range open-ended on one side.

| Key                       | Unit        | Description                                           |
| ------------------------- | ----------- | ----------------------------------------------------- |
| `emp_count`               | integer     | Employee count (headcount).                           |
| `pl_revenue`              | EUR         | Revenue from the most recent profit & loss statement. |
| `pl_net_income`           | EUR         | Net income (Jahresergebnis).                          |
| `pl_ebit`                 | EUR         | Earnings before interest and taxes.                   |
| `bs_assets_total`         | EUR         | Total assets from the most recent balance sheet.      |
| `bs_equity_total`         | EUR         | Total equity.                                         |
| `bs_liabilities_total`    | EUR         | Total liabilities.                                    |
| `bs_cash_and_equivalents` | EUR         | Cash & equivalents.                                   |
| `bs_equity_ratio`         | ratio (0–1) | Equity / total assets.                                |
| `bs_debt_to_assets`       | ratio (0–1) | Total liabilities / total assets.                     |

Example — companies with revenue between €10 M and €50 M, at least 50 employees, equity ratio of at least 30 %:

```json
{
  "financial_filters": {
    "pl_revenue":     { "gte": 10000000, "lte": 50000000 },
    "emp_count":      { "gte": 50 },
    "bs_equity_ratio": { "gte": 0.3 }
  }
}
```

***

### Reference tables

#### Company size categories

EU recommendation 2003/361/EC thresholds.

| Value    | Headcount           | Annual revenue |
| -------- | ------------------- | -------------- |
| `micro`  | up to 9 employees   | up to €2 M     |
| `small`  | up to 49 employees  | up to €10 M    |
| `medium` | up to 249 employees | up to €50 M    |
| `large`  | over 249 employees  | or over €50 M  |

#### Registration types

| Value | Register                | Typical entities                     |
| ----- | ----------------------- | ------------------------------------ |
| `HRA` | Handelsregister A       | Sole traders, partnerships (OHG, KG) |
| `HRB` | Handelsregister B       | Corporations (GmbH, AG, SE, …)       |
| `GnR` | Genossenschaftsregister | Cooperatives (eG)                    |
| `GsR` | Gesellschaftsregister   | Civil-law companies (GbR)            |
| `PR`  | Partnerschaftsregister  | Professional partnerships (PartG)    |
| `VR`  | Vereinsregister         | Registered associations (e.V.)       |

#### Legal forms

Pass the legal form as `legal_form_code` (e.g., `GmbH`, `AG`, `GmbH & Co. KG`). Fusionbase recognises 122 codes grouped into eight families:

| Family            | Examples                                                              |
| ----------------- | --------------------------------------------------------------------- |
| **Common**        | `GmbH`, `e.V.`, `GmbH & Co. KG`, `UG (haftungsbeschränkt)`, `e.K.`    |
| **Corporations**  | `AG`, `SE`, `KGaA`, `gemeinnützige GmbH`                              |
| **Partnerships**  | `OHG`, `KG`, `GbR`, `PartG`, `PartG mbB`                              |
| **Mixed forms**   | `GmbH & Co. KG`, `AG & Co. KG`, `Stiftung & Co. KG`                   |
| **Cooperatives**  | `eG`, `e.G.`                                                          |
| **Public law**    | `Anstalt öffentlichen Rechts`, `Körperschaft des öffentlichen Rechts` |
| **International** | `Limited`, `Ltd`, `S.A.`, `S.p.A.`, `B.V.`, …                         |
| **Other**         | `Stiftung`, `Stiftung & Co. KG`, `Anstalt`, …                         |

→ **See the full enumeration on the Legal Forms reference page.** That subpage lists every accepted `legal_form_code`, grouped by family.

#### Industry codes

`industry_code` follows the German **WZ2025** classification (Klassifikation der Wirtschaftszweige, Ausgabe 2025), published by the Federal Statistical Office (Destatis). Provide the dotted code (e.g., `62.01`, `64.20.1`) along with `industry_scheme: "WZ2025"`. Hierarchical match: passing a parent code (e.g., `62`) returns all child codes underneath it.

WZ2025 contains roughly 1,000 codes across five hierarchical levels (section → division → group → class → subclass), so we don't reproduce the full list here. Two ways to look up a code:

* **Official source:** the Destatis "[Klassifikation der Wirtschaftszweige WZ 2025](https://www.klassifikationsserver.de/klassService/thyme/variant/wz2025)" publication contains the authoritative tree and translation tables.
* **Interactive picker:** the [Fusionbase Data Hub](https://app.fusionbase.com) industry filter on the organization search page lets you browse and search the tree in the same form used by this API.

***

### Response

```json
{
  "results": [
    {
      "entity_type": "ORGANIZATION",
      "entity": {
        "fb_entity_id": "82a68ab9f7151fa0af9bf189c1caa753",
        "status": "ACTIVE",
        "display_address": "Sankt-Martin-Straße 112, 81669 Munich, Germany",
        "registration_authority_entity_name": "München HRB 243843",
        "display_name": "OroraTech GmbH",
        "name": "OroraTech GmbH",
        "source_key": "1051122944"
      },
      "score": 40.53212
    }
  ],
  "total": 10
}
```

#### Fields

| Field                                                 | Description                                                                                                                                           |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `results[]`                                           | Array of matched organizations, ordered by `score` descending.                                                                                        |
| `results[].entity_type`                               | Always `"ORGANIZATION"` for this endpoint.                                                                                                            |
| `results[].entity.fb_entity_id`                       | **Stable Fusionbase entity identifier.** Use this to fetch the full entity, related relations, or to reference the organization elsewhere in the API. |
| `results[].entity.status`                             | `ACTIVE`, `INACTIVE`, `DISSOLVED`, `MERGED`, etc.                                                                                                     |
| `results[].entity.display_address`                    | Single-line postal address (street, postcode, city, country).                                                                                         |
| `results[].entity.registration_authority_entity_name` | Combined registry court and number, e.g., `"München HRB 243843"`.                                                                                     |
| `results[].entity.display_name`                       | Human-readable name (may include trade-name annotations).                                                                                             |
| `results[].entity.name`                               | Official legal name.                                                                                                                                  |
| `results[].entity.source_key`                         | The data source from which this record was loaded — matches the request's `source_key`.                                                               |
| `results[].score`                                     | Relevance score. Higher is more relevant. Comparable only within a single response (not across queries).                                              |
| `total`                                               | Total number of matching organizations across all pages — use it to compute the last page (`Math.ceil(total / limit)`).                               |

The `fb_entity_id` is the primary key you should persist if you reference the organization later — it's stable across data refreshes.

***

### Pagination

Iterate by combining `limit` and `skip`. `total` in the response tells you when you've reached the end.

```
# page 1
GET /api/v2/search/entities/organization?q=Software&limit=25&skip=0
# page 2
GET /api/v2/search/entities/organization?q=Software&limit=25&skip=25
# page 3
GET /api/v2/search/entities/organization?q=Software&limit=25&skip=50
```

There is no hard upper bound enforced by the API; pick a `limit` that suits your UI (10–50 is typical).

***

### Full examples

#### 1. Plain name search

```bash
curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=OroraTech GmbH'
```

#### 2. Active GmbH in Munich, registered HRB

```bash
FILTERS='{"active":true,"city":"Munich","legal_form_code":"GmbH","registration_type":"HRB"}'

curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=' \
  --data-urlencode "filters=$FILTERS" \
  --data-urlencode 'limit=25'
```

#### 3. Medium-sized software companies (WZ2025 62.01) with revenue ≥ €10 M and equity ratio ≥ 30 %

```bash
FILTERS='{
  "active": true,
  "bs_company_size_category": "medium",
  "industry_code": "62.01",
  "industry_scheme": "WZ2025",
  "financial_filters": {
    "pl_revenue": { "gte": 10000000 },
    "bs_equity_ratio": { "gte": 0.3 }
  }
}'

curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=' \
  --data-urlencode "filters=$FILTERS"
```

#### 4. Companies founded in 2020, headcount between 10 and 50

```bash
FILTERS='{
  "registration_date_from": "2020-01-01",
  "registration_date_to":   "2020-12-31",
  "financial_filters": { "emp_count": { "gte": 10, "lte": 50 } }
}'

curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=' \
  --data-urlencode "filters=$FILTERS"
```

#### 5. Lookup by exact registration

```bash
FILTERS='{
  "registration_type": "HRB",
  "registration_authority_name": "München",
  "registration_number": "243843"
}'

curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=' \
  --data-urlencode "filters=$FILTERS"
```

***

### Tips

* **Combining `q` with filters.** Filters narrow the corpus _before_ relevance ranking. `q=""` plus a tight filter set returns deterministic, filter-only results ordered by recency/relevance from the registry.
* **Stable IDs.** `fb_entity_id` is stable across data refreshes — persist it instead of name/address combinations.
* **Empty ranges.** A range field with neither `gte` nor `lte` is treated as absent. Drop it client-side to keep payloads small.
* **Industry hierarchy.** Pass a parent WZ2025 code to match all descendants (`62` includes `62.01`, `62.02`, …).
