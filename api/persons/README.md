---
description: >-
  Find and profile individuals appearing in German company filings — directors,
  board members, prokurists, shareholders.
---

# Persons

The Persons API gives you structured access to the individuals named in official German business registers — managing directors, board members, prokurists, shareholders, beneficial owners. Every person is identified by a stable `fb_entity_id` that links their base profile to all their appearances across organizations.

This section is organised as three endpoints, each with its own subpage:

| Endpoint         | What it returns                                                                                                                                         | When to use it                                          |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Person Search    | Ranked list of people matching a free-text query.                                                                                                       | Find a person by name.                                  |
| Person Entity    | The canonical profile of one person — name, home location, birth date, jurisdiction.                                                                    | Fetch the base record once you know the `fb_entity_id`. |
| Person Relations | Typed data slices about one person: career history (every management role they've held), and the network of organizations and people connected to them. | Drill into specific data for a known person.            |

***

### The three-step flow

Almost every integration follows the same pattern:

```
1. Search        →  resolve fb_entity_id
2. Entity        →  load the base profile
3. Relation(s)   →  fan out to the data slices you need
```

`fb_entity_id` is the linking key. It's stable across data refreshes — persist it instead of name + birthdate combinations.

#### Minimal end-to-end example

```bash
# 1. Find the person
curl -G 'https://api.fusionbase.com/api/v2/search/entities/person' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  --data-urlencode 'q=Timotheus Höttges'
# → results[0].entity.fb_entity_id = "29db47808ed652004d14aaf9bdaea6d3"

# 2. Load the base record
curl 'https://api.fusionbase.com/api/v2/entities/person/get/29db47808ed652004d14aaf9bdaea6d3' \
  -H 'X-API-KEY: <YOUR_API_KEY>'

# 3. Fetch the History relation (5990434853)
curl -X POST 'https://api.fusionbase.com/api/v2/relation/resolve/5990434853/29db47808ed652004d14aaf9bdaea6d3' \
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

Persons are extracted from the official German registers wherever a natural person appears in connection with a registered entity:

* **Handelsregister** — managing directors, board members, prokurists, liquidators (HRA / HRB / GnR / GsR / PR / VR).



* **Shareholder lists (Gesellschafterlisten)** — natural-person shareholders disclosed in commercial-register filings.

A person record is only created when the individual is explicitly named in an official filing — Fusionbase does not enrich persons from non-registry sources.

***

### Privacy

Person entities are part of public registers. Some fields (e.g., date of birth) are hidden to API tiers that grant access to identifying details; others (name, home town, role history) are public-by-default.
