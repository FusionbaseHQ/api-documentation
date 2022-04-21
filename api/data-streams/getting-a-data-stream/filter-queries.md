---
description: >-
  The actual values of a Data Stream can be queried and filtered using the query
  parameter. This allows for the selection of a subset of the data based on the
  records values.
---

# Filter Queries

{% swagger method="get" path="/get/[STREAM_ID]?query={"fb_id": "06e47a2241e0ba7b9c7ba44f16223338"}" baseUrl="https://api.fusionbase.com/api/v1/data-stream" summary="Filter Query" %}
{% swagger-description %}
The actual values of a Data Stream can be queried and filtered using the 

`query`

 parameter. This allows for the selection of a subset of the data based on the records values.
{% endswagger-description %}

{% swagger-parameter in="query" name="query" type="Dictionary" %}
A dictionary where the keys are the column names of the Data Stream and corresponding values represent the record values to be selected.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="STREM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}
{% endswagger %}

This query would return the record with the `fb_id` _<mark style="color:blue;">`06e47a2241e0ba7b9c7ba44f16223338`</mark>_

{% hint style="info" %}
Make sure to properly URL encode the query parameters!
{% endhint %}
