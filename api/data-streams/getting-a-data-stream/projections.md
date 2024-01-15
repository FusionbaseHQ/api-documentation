---
description: >-
  This section of the API allows users to retrieve specific fields from a
  dataset. The project_fields parameter is used to specify which fields should
  be returned in the response. This can optimize the
---

# Projections

{% swagger expanded="true" method="get" path="/{STREAM_ID}?format=json&query_parameters={ "project_fields": ["FIELD_NAME"]}" baseUrl="https://api.fusionbase.com/api/v2/stream/data" summary="Projections" %}
{% swagger-description %}
If you are only interested in a subset of columns in a Data Stream you can project them. Therefore, the result set does not contain any additional columns. Specifically, in cases when the Data Stream has a large number of columns, this can significantly speed up the retrieval performance.
{% endswagger-description %}

{% swagger-parameter in="query" name="query_parameters" type="List" %}
The name of the projected column, i.e., the fields that should exist in the result set
{% endswagger-parameter %}

{% swagger-parameter in="path" name="SREAM_ID" %}
The ID of the Data Stream
{% endswagger-parameter %}
{% endswagger %}

#### Request Structure

To project fields in your data retrieval, use the `project_fields` parameter within the `query_parameters` in your GET request to `/api/v2/stream/data/{STREAM_ID}`. The `project_fields` parameter should be formatted as a JSON array of strings representing the field names.

#### Project Fields Properties

* `project_fields`: An array of string values where each string represents the name of a field that you want to include in the response.

#### Multiple Field Projection

You can project multiple fields in a single request. Include all desired field names as strings in the `project_fields` array.

#### Example

For instance, if you wish to retrieve only the `manufacturer_brand` field from a data stream, your request would be structured as follows:

```bash
/api/v2/stream/data/{STREAM_ID}?query_parameters={"project_fields": ["manufacturer_brand"]}&format=json
```

#### Notes

* Ensure that the field names used in `project_fields` correspond to valid column names in the data stream.
* The field names in `project_fields` are case-sensitive and should match the exact casing of the column names in the data stream.

#### Error Handling

Errors may occur if the `project_fields` criteria are not correctly structured, if invalid field names are used, or if there is a syntax error in the request. In such cases, the API might return an error response.

This approach to documenting the 'Project Fields' feature mirrors the structure and detail provided in your example for using filters, ensuring consistency in the API documentation style.

