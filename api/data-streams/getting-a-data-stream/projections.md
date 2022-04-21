---
description: >-
  If you are only interested in a subset of columns in a Data Stream you can
  project them. Therefore, the result set does not contain any additional
  columns.
---

# Projections

In cases when the Data Stream has a large number of columns, this can significantly speed up the retrieval performance.

{% swagger method="get" path="/get/[STREAM_ID]?fields=fb_id&fields=region_name" baseUrl="https://api.fusionbase.com/api/v1/data-stream" summary="Projections" %}
{% swagger-description %}
If you are only interested in a subset of columns in a Data Stream you can project them. Therefore, the result set does not contain any additional columns. Specifically, in cases when the Data Stream has a large number of columns, this can significantly speed up the retrieval performance.
{% endswagger-description %}

{% swagger-parameter in="query" name="fields" type="List" %}
The name of the projected column, i.e., the fields that should exist in the result set
{% endswagger-parameter %}

{% swagger-parameter in="path" name="SREAM_ID" %}
The ID of the Data Stream
{% endswagger-parameter %}
{% endswagger %}

