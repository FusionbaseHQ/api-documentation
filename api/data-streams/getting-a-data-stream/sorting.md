---
description: >-
  The Data Stream can be sorted by any column in ascending or descending order.
  Sorting by multiple columns at once is also possible.
---

# Sorting

{% swagger method="get" path="/get/[STREAM_ID]?sort_keys=region_name&sort_order=desc&region_name=asc" baseUrl="https://api.fusionbase.com/api/v1/data-stream" summary="Sorting" %}
{% swagger-description %}
The Data Stream can be sorted by any column in ascending or descending order. Sorting by multiple columns at once is also possible.
{% endswagger-description %}

{% swagger-parameter in="query" name="sort_keys" type="List" %}
The column names for which the Data Stream should be sorted.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort_order" %}
The order of how the Data Stream should be sorted. This is either 

`desc`

 or 

`asc`

.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="STREAM_ID" %}
The ID of the Data Stream
{% endswagger-parameter %}
{% endswagger %}

This query would sort the Data Stream by (if present) `region_name` in ascending order.\
\
The order within the `sort_keys` and `sort_order` parameters do matter. The first appearance of `sort_key` will correspond to the first appearance of `sort_order` and so on. \
\
Additionally, both parameters must have the same number of occurrences, otherwise, the query will fail.
