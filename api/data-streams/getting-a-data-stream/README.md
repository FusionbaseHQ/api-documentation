# Getting a Dataset

The Fusionbase API provides streamlined access to data streams, offering specialized endpoints for retrieving metadata, querying data, and conducting string searches within the data. Access to these features is achieved through authenticated GET requests.

{% hint style="info" %}
The base URL is always\
[https://api.fusionbase.com](https://api.fusionbase.com)
{% endhint %}

**Endpoints**

1. **Metadata Retrieval**
   * Endpoint: `/api/v2/stream/base/{STREAM_ID}`
   * Purpose: Retrieves metadata for data streams.
2. **Data Retrieval and Querying**
   * Endpoint: `/api/v2/stream/data/{STREAM_ID}`
   * Purpose: Retrieves and queries data from a specified data stream.
3. **String Search within Data**
   * Endpoint: `/api/v2/stream/data/search/{STREAM_ID}`
   * Purpose: Performs string searches within a specified data stream.

**Basic Request Structure**

`skip` and `limit` are passed as plain top-level query parameters. **Sorting, filtering and projections are expressed through a single `query_parameters` object** (a URI-encoded JSON string) — see the Sorting, Filter Queries and Projections pages. To retrieve the first 10 rows sorted by `fb_datetime` descending:

```
/api/v2/stream/data/{STREAM_ID}?skip=0&limit=10&query_parameters={"sort_keys": ["fb_datetime"], "sort_order": ["desc"]}
```

**URL Components**

The URL is composed of the following elements:

* **Base URL**: `https://api.fusionbase.com/api/v2`
* **Resource Path**: This varies based on the operation, such as `/stream/base`, `/stream/data/{STREAM_ID}`, or `/stream/data/search/{STREAM_ID}`.
* **Top-level query parameters**: `skip`, `limit`, `format`, `version_boundary`.
* **`query_parameters`**: a URI-encoded JSON object carrying `sort_keys` / `sort_order` (sorting), `filters` (filtering) and `project_fields` (projections).

**Example**

A typical request to retrieve data from a stream looks like this:

```
/api/v2/stream/data/403398?skip=0&limit=10&query_parameters={"sort_keys": ["fb_datetime"], "sort_order": ["desc"]}
```

This request fetches data from the stream with ID 403398, skipping 0 records, limiting to 10 records, and sorting the results by `fb_datetime` in descending order.

{% hint style="warning" %}
Limited access to a Data Stream may result in receiving only a restricted number of records, such as 10 records.
{% endhint %}

**Query Parameters**

The API supports a variety of filter and query parameters to customize the result set according to user requirements. These parameters — sorting, filtering and projections — are all supplied inside the `query_parameters` object and can be combined in a single request for comprehensive data retrieval. See the dedicated Pagination, Sorting, Filter Queries and Projections pages for the exact syntax of each.
