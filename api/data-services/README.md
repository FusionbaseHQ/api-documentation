---
description: >-
  Discover and invoke parameterised data services with structured inputs and
  outputs.
---

# Services

Services are Fusionbase's dynamic data endpoints — parameterised, input-driven APIs that compute or look up results on demand. Where a Dataset is a fixed table of rows you page through, a Service takes structured inputs (e.g., an address, a coordinate, a company identifier) and returns a structured response specific to those inputs. Geocoding lookups, risk-scoring calls, and enrichment endpoints are all Services.

This section is organised as two endpoints, each with its own subpage:

| Endpoint       | What it returns                                                                         | When to use it                                                    |
| -------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Service Search | Ranked list of services (and datasets) matching a free-text query.                      | Find a service by topic, name fragment, or coverage.              |
| Service Fetch  | Service metadata and the invocation endpoint that runs the service against your inputs. | Inspect a service's input schema, then invoke it to get a result. |

***

### The two-step flow

Most integrations follow the same pattern:

```
1. Search   →  resolve service ID
2. Invoke   →  POST your inputs, read the result
```

The service ID returned by Search is the key you use when invoking the service.

#### Minimal end-to-end example

```bash
# 1. Find the service
curl 'https://api.fusionbase.com/api/v2/search/data?q=Geocoding' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  -H 'Content-Type: application/json; charset=utf-8'
# → results[0].entity.key = "<SERVICE_ID>" (entity_type: "SERVICE")

# 2. Read the service metadata to learn the input schema
curl 'https://api.fusionbase.com/api/v2/service/get/<SERVICE_ID>' \
  -H 'X-API-KEY: <YOUR_API_KEY>'

# 3. Invoke the service
curl -X POST 'https://api.fusionbase.com/api/v2/service/invoke' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -d '{
    "service_key": "<SERVICE_ID>",
    "inputs": { /* per the service'\''s input schema */ }
  }'
```

The exact `inputs` shape varies per service — read the metadata returned by `/service/get/{SERVICE_ID}` (or browse the service's page in the Data Hub) to see the expected fields.

***

### Datasets vs. Services

The two surfaces are deliberately different:

|             | Datasets                                               | Services                                                 |
| ----------- | ------------------------------------------------------ | -------------------------------------------------------- |
| Fetch model | Pull rows by ID + pagination                           | POST inputs, get a result                                |
| HTTP method | `GET`                                                  | `POST`                                                   |
| Cacheable?  | Highly — same query returns same rows                  | Per-input — cache by `(service_id, inputs)`              |
| Examples    | Geodata Lookups for Germany, IPv4 Geolocation Mappings | Address geocoding, risk-score lookup, company enrichment |

Both surfaces are discoverable through the same Search endpoint — filter by `entity_type` (`"SERVICE"` vs `"STREAM"`) in the response to separate them.

***

### Authentication

Both endpoints share the same authentication scheme — your Fusionbase API key in the `X-API-KEY` header on every request:

```http
X-API-KEY: <YOUR_API_KEY>
Content-Type: application/json; charset=utf-8
```

See each subpage for the full request format and examples.

***

### Browse before you build

The fastest way to see what's available is the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data). Every service has a detail page that previews its input schema, sample output, pricing, and an interactive playground — useful for picking the right service ID and trying it out before wiring up the API.
