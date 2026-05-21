---
description: >-
  Search, profile and drill into German company data through one ID-linked API
  surface.
---

# Organizations

The Organizations API gives you structured access to companies registered in Germany — corporations (GmbH, AG, SE, …), partnerships (OHG, KG, GbR, …), cooperatives (eG), associations (e.V.) and public-law entities. Every organization is identified by a stable `fb_entity_id` that you use to fetch its base record and any of its data slices.

This section is organised as three endpoints, each with its own subpage:

| Endpoint               | What it returns                                                                                                                                         | When to use it                                                                                       |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Organization Search    | Ranked list of organizations matching a free-text query and optional structured filters (status, location, registration, industry, size, financials).   | Find a company. Build name pickers, market-mapping tools, or filter-driven lists.                    |
| Organization Entity    | The canonical profile of one organization — name, address, legal registration, classifications, contact details, status.                                | Fetch the base record once you know the `fb_entity_id`.                                              |
| Organization Relations | Typed data slices about one organization: management, shareholders, balance sheets, P\&L, financial reports, news, network, nearby companies, and more. | Drill into specific data for a known organization. Each slice is a separate relation you call by ID. |

***

### The three-step flow

Almost every integration follows the same pattern:

```
1. Search        →  resolve fb_entity_id
2. Entity        →  load the base profile
3. Relation(s)   →  fan out to the data slices you need
```

`fb_entity_id` is the linking key. It's stable across data refreshes — persist it instead of name + address combinations.

#### Minimal end-to-end example

```bash
# 1. Find the company
curl -G 'https://api.fusionbase.com/api/v2/search/entities/organization' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=OroraTech GmbH'
# → results[0].entity.fb_entity_id = "82a68ab9f7151fa0af9bf189c1caa753"

# 2. Load the base record
curl 'https://api.fusionbase.com/api/v2/entities/organization/get/82a68ab9f7151fa0af9bf189c1caa753' \
  -H 'X-API-KEY: <YOUR_API_KEY>'

# 3. Fetch the Management relation (5906082026)
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/5906082026/82a68ab9f7151fa0af9bf189c1caa753' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### Authentication

All three endpoints share the same authentication scheme — your Fusionbase API key in the `X-API-KEY` header on every request:

```http
X-API-KEY: <YOUR_API_KEY>
```

See each subpage for the full request format and examples.

***

### Data coverage

The Organizations API draws from Fusionbase's curated mirror of official German registers:

* **Handelsregister** — companies, partnerships, cooperatives and associations registered across all German registry courts (HRA / HRB / GnR / GsR / PR / VR).
* **Bundesanzeiger** — published annual financial statements (Jahresabschluss / Konzernabschluss).
* **Insolvency notices** — official insolvency proceedings from the federal insolvency notices portal.

International coverage and additional sources are added over time. The `source` field on each response tells you which dataset the record came from.

***

### Browse before you build

The fastest way to understand what's available is the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data). The **Unternehmen / Company** category lists every relation with a live API Playground that runs against real example entities — you can see the exact response schema and parameter shape before writing a line of code.
