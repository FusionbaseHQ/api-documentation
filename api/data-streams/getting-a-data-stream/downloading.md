---
description: >-
  Instead of getting a direct JSON response, the API supports the generation of
  a gzipped file of the Data Stream as well. Specifically for larger result
  sets, this can be the way to go.
---

# Downloading

{% swagger method="get" path="/get/[STREAM_ID]/?compressed_file=true" baseUrl="https://api.fusionbase.com/api/v1/data-stream" summary="Compressed File" %}
{% swagger-description %}
Instead of getting a direct JSON response, the API supports the generation of a gzipped file of the Data Stream as well. Specifically for larger result sets, this can be the way to go.
{% endswagger-description %}

{% swagger-parameter in="query" name="compressed_file" type="Boolean" %}
Indicates whether to generate a gzipped compressed file or not. Default is 

`false`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="STREAM_ID" %}
The ID of the Data Stream
{% endswagger-parameter %}
{% endswagger %}

The query would return a gzipped compressed file version of the Data Stream.
