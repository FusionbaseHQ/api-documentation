---
description: Search the data catalogue and pull rows from any dataset by ID.
---

# Datasets (Streams)

## Datasets

Datasets are Fusionbase's curated, regularly updated data sources — statistical series, geo lookups, public-registry exports, environmental feeds, and more. Every dataset has a stable numeric ID and exposes its metadata, full content, and a string-search facet over the same family of endpoints.

This section is organised as two endpoints, each with its own subpage:

| Endpoint       | What it returns                                                             | When to use it                                        |
| -------------- | --------------------------------------------------------------------------- | ----------------------------------------------------- |
| Dataset Search | Ranked list of datasets (and data services) matching a free-text query.     | Find a dataset by topic, name fragment, or coverage.  |
| Dataset Fetch  | Metadata, paginated rows, and in-dataset string search for a known dataset. | Retrieve content from a dataset once you know its ID. |

***

### The two-step flow

Most integrations follow the same pattern:

```
1. Search   →  resolve dataset ID
2. Fetch    →  pull metadata, then paginated rows (optionally filtered)
```

The dataset ID returned by Search is the key you use for every Fetch call.

#### Minimal end-to-end example

```bash
# 1. Find the dataset
curl 'https://api.fusionbase.com/api/v2/search/data?q=Geodata%20Lookups%20for%20Germany' \
  -H 'X-API-KEY: <YOUR_API_KEY>' \
  -H 'Content-Type: application/json; charset=utf-8'
# → results[0].entity.key = "4994292"

# 2. Read the dataset metadata
curl 'https://api.fusionbase.com/api/v2/stream/base/4994292' \
  -H 'X-API-KEY: <YOUR_API_KEY>'

# 3. Pull the first page of rows
curl 'https://api.fusionbase.com/api/v2/stream/data/4994292?skip=0&limit=10&sort_keys=fb_datetime&sort_order=desc' \
  -H 'X-API-KEY: <YOUR_API_KEY>'
```

***

### A note on naming

We refer to these resources as **Datasets** throughout the documentation. The underlying API paths and response fields still use the historical name **stream**, and that won't change in this version of the API:

| Documentation term              | API path / field                         |
| ------------------------------- | ---------------------------------------- |
| Dataset Search                  | `/api/v2/search/data`                    |
| Dataset metadata                | `/api/v2/stream/base/{STREAM_ID}`        |
| Dataset rows                    | `/api/v2/stream/data/{STREAM_ID}`        |
| In-dataset string search        | `/api/v2/stream/data/search/{STREAM_ID}` |
| `entity_type` on search results | `"STREAM"`                               |

Treat **dataset** and **stream** as synonymous — they're the same resource, just named differently in the URL space and the response payload.

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

The fastest way to see what's available is the [Fusionbase Data Hub catalog](https://app.fusionbase.com/browse-data). Every dataset has a detail page that previews schema, coverage and a sample of rows — useful for picking the right dataset ID before wiring up the API.
