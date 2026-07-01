---
description: Best-match a single company, even from messy input or outside the register.
---

# AI Search

AI Search resolves a single, best-matching organization from a **name**, a **register identifier**, or **free text** — and enriches it. Where the standard Search endpoint ranks candidates for an exact query, AI Search hands you back one canonical organization, even when your input isn't clean.

```
GET https://api.fusionbase.com/api/v2/entities/organization/ai/get/<QUERY>
```

***

#### Authentication

```http
X-API-KEY: YOUR_API_KEY
```

***

#### Path parameter

| Parameter | Required | Description                                                                                                                                                                                       |
| --------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<QUERY>` | yes      | The organization to resolve — a company name, a register identifier (e.g. `München HRB 210918`), or free text. URL-encode it. A `fb_entity_id` is also accepted and returns that entity directly. |

#### Why use it

* **Resilient matching.** Resolves the right company even when the input is incomplete or messy — typos, wrong casing, missing umlauts, a bare register identifier. For example, `KONUX GmbH`, `München HRB 210918`, and `konux gmbh munich` all resolve to the same organization, where an exact search would miss.
* **Beyond the Handelsregister.** Returns base data — address, contact, website, description, industry classification — for organizations that aren't in the commercial register: small businesses, freelancers, and foreign companies. A query like `di pinto di blu, münchen` comes back as a fully-populated organization even though there's no register entry.

#### Good to know

* **Responses take a little longer.** AI Search enriches on the fly, so expect a few seconds per call, versus sub-second for the standard Entity and Search endpoints.
* **It costs slightly more credits** than a plain lookup — the tradeoff for the extra matching power and coverage.

Reach for AI Search when your input isn't clean or the company may sit outside the commercial register; use the standard Search and Entity endpoints for exact queries and known `fb_entity_id`s.

***

#### Examples

The query is part of the URL **path**, so it must be URL-encoded — spaces become `%20`, `ü` becomes `%C3%BC`, `,` becomes `%2C`. Most HTTP clients do this for you; the examples below show the encoded form.

**Resolve by name** (`KONUX GmbH`):

```bash
curl 'https://api.fusionbase.com/api/v2/entities/organization/ai/get/KONUX%20GmbH' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

**Resolve by register identifier** (`München HRB 210918`):

```bash
curl 'https://api.fusionbase.com/api/v2/entities/organization/ai/get/M%C3%BCnchen%20HRB%20210918' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

**Resolve a company that isn't in the Handelsregister** (`di pinto di blu, münchen`):

```bash
curl 'https://api.fusionbase.com/api/v2/entities/organization/ai/get/di%20pinto%20di%20blu%2C%20m%C3%BCnchen' \
  -H 'X-API-KEY: YOUR_API_KEY'
```

***

#### Response

Returns a single organization object in the same structure as the Entity endpoint. If no organization can be resolved, the endpoint returns `404`.
