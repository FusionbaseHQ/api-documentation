# Getting a Data Stream

To get a Data Stream we need to compose a request URI and send it via an authenticated `GET`request to the Fusionbase API. Each successful request returns the data, as well as, the metadata of a Data Stream.

{% hint style="warning" %}
If you don't have access to the Data Stream, you'll only get 10 records of the data
{% endhint %}

If we just want to get the data without any special queries, we can just copy the Endpoint URI provided in the Fusionbase App as shown in the screenshot below:

![Data Stream URI](../../../.gitbook/assets/screenshot\_api\_data\_stream.png)

## Basic Request

By default, the Data Stream API endpoint URL looks as follows:

```url
https://api.fusionbase.com/api/v1/data-stream/get/403398?skip=0&limit=10&sort_keys=fb_datetime&sort_order=desc
```

### URL Components <a href="#uri-components" id="uri-components"></a>

The URL consists of four parts:

* The base URL `https://api.fusionbase.com/api/`
* The API version `/v1/` (current version is 1)
* The data stream resource `/data-stream/get/[DATA_STREAM_ID]`
* The query parameters `?[QUERY_PARAMETERS]`

A sample request to the stated URL will return the Data Stream with the ID `403398`, skipping `0`records, limiting the returned records to `10` and sort the result by `fb_datetime` in descending order.

## Parameters

The API implements different filter and query parameters to tailor the result set to your needs. All basic query operations are supported.

{% hint style="info" %}
All query parameters can be arbitrarily combined in a single request!
{% endhint %}

The following retrieval parameters are currently available:

{% content-ref url="pagination.md" %}
[pagination.md](pagination.md)
{% endcontent-ref %}

{% content-ref url="sorting.md" %}
[sorting.md](sorting.md)
{% endcontent-ref %}

{% content-ref url="filter-queries.md" %}
[filter-queries.md](filter-queries.md)
{% endcontent-ref %}

{% content-ref url="projections.md" %}
[projections.md](projections.md)
{% endcontent-ref %}

{% content-ref url="downloading.md" %}
[downloading.md](downloading.md)
{% endcontent-ref %}
