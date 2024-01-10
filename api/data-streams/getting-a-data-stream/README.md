# Getting a Stream

The Fusionbase API provides streamlined access to data streams, offering specialized endpoints for retrieving metadata, querying data, and conducting string searches within the data. Access to these features is achieved through authenticated GET requests.

{% hint style="info" %}
The base URL is always\
https://api.fusionbase.com
{% endhint %}

#### Endpoints

1. **Metadata Retrieval**
   * Endpoint: `/api/v2/stream/base/{STREAM_ID}`
   * Purpose: Retrieves metadata for data streams.
2. **Data Retrieval and Querying**
   * Endpoint: `/api/v2/stream/data/{STREAM_ID}`
   * Purpose: Retrieves and queries data from a specified data stream.
3. **String Search within Data**
   * Endpoint: `/api/v2/stream/data/search/{STREAM_ID}`
   * Purpose: Performs string searches within a specified data stream.

#### Basic Request Structure

To retrieve data from a stream without specific queries, the API endpoint URL is structured as follows:

```
/api/v2/stream/data/{STREAM_ID}?skip=0&limit=10&sort_keys=fb_datetime&sort_order=desc
```

#### URL Components

The URL is composed of the following elements:

* **Base URL**: `{{base_api_url}}/api/`
* **Resource Path**: This varies based on the operation, such as `/stream/base`, `/stream/data/{STREAM_ID}`, or `/stream/data/search/{STREAM_ID}`.
* **Query Parameters**: `?[QUERY_PARAMETERS]`, used for specifying details like pagination, sorting, and filtering.

#### Example

A typical request to retrieve data from a stream looks like this:

```
/api/v2/stream/data/403398?skip=0&limit=10&sort_keys=fb_datetime&sort_order=desc
```

This request fetches data from the stream with ID 403398, skipping 0 records, limiting to 10 records, and sorting the results by `fb_datetime` in descending order.

{% hint style="warning" %}
Limited access to a Data Stream may result in receiving only a restricted number of records, such as 10 records.
{% endhint %}

#### Query Parameters

The API supports a variety of filter and query parameters to customize the result set according to user requirements. These parameters include options for basic query operations and can be combined in a single request for comprehensive data retrieval.
