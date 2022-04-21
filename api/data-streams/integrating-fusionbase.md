# Integrating Fusionbase

The Fusionbase API is designed to make the integration of our Data Streams as convenient as possible. For scenarios where the data is regularly pulled and integrated into a local environment, the API offers a delta endpoint that only returns records that are more recent than the given `fb_data_version`.

## Delta Endpoint

Let's assume a Data Stream is updated five times a day and you decide to pull the data just once a day. To get all of the newly added records from your local version up to the most recent version of the Data Stream, simply use the delta endpoint.

{% swagger baseUrl="https://api.fusionbase.com/api/v1/data-stream" method="get" path="/get/delta/[STREAM_ID]/[FB_DATA_VERSION]" summary="Get records from a certain version" %}
{% swagger-description %}
Get the data of a given Data stream since a specified version
{% endswagger-description %}

{% swagger-parameter in="path" name="STREAM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="path" name="FB_DATA_VERSION" required="true" %}
The version of the Data stream since when you want to query the data.
{% endswagger-parameter %}

{% swagger-response status="200" description="Pet successfully created" %}
```javascript
{
    "name"="Wilson",
    "owner": {
        "id": "sha7891bikojbkreuy",
        "name": "Samuel Passet",
    "species": "Dog",}
    "breed": "Golden Retriever",
}
```
{% endswagger-response %}
{% endswagger %}

Example: `https://api.fusionbase.com/api/v1/data-stream/get/delta/1008886/b080b7a1-f928-46d4-8fb4-01af9e44e01c`&#x20;

\
An API request to the given URL would return all records that have been added to the Data Stream with the ID `1008886` since version `b080b7a1-f928-46d4-8fb4-01af9e44e01c`
