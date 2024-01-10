---
description: >-
  The actual values of a Data Stream can be queried and filtered using the query
  parameter. This allows for the selection of a subset of the data based on the
  records values.
---

# Filter Queries

{% swagger method="get" path="/{STREAM_ID]?query_parameters={"filters": [{"property": "construction_year_end", "operator": "GREATER_THAN", "value": 2013}]}&format=json" baseUrl="https://api.fusionbase.com/api/v2/stream/data" summary="Filter the data stream" %}
{% swagger-description %}
The actual values of a Data Stream can be queried and filtered using the `filters`parameter in `query_parameters`. This allows for the selection of a subset of the data based on the records values.
{% endswagger-description %}

{% swagger-parameter in="query" name="format" type="String" %}
json or msgpack
{% endswagger-parameter %}

{% swagger-parameter in="path" name="STREM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="query_parameters" type="Object" %}
URI encoded JSON string
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
This is a sample response for a specific stream:

```python
[
        {
            "fb_id": "31f72e92f33bcfb39ad100d8b981c9ac",
            "kba_reference_number": "011209",
            "manufacturer_recall_code": "NC3I6515R (0797186)",
            "manufacturer_brand": "MERCEDES-BENZ LKW",
            "trade_name": "SPRINTER",
            "construction_year_start": 2011,
            "construction_year_end": 2013,
            "affected_vehicles_worldwide": 5799.0,
            "affected_vehicles_germany": 3368.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Entfernung unzulässiger Abschalteinrichtungen bzw. der unzulässigen Reduzierung der Wirksamkeit des Emissionskontrollsystems.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Software Steuergeräte aktualisieren.",
            "known_incidents": "unknown",
            "release_date": "2021-10-25",
            "support_hotline": "06986798274",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "e8ffe1c04bc7f094520a13921e018579",
            "kba_reference_number": "011713",
            "manufacturer_recall_code": "30_2028161",
            "manufacturer_brand": "KTM",
            "trade_name": "125 SX, 125 XC, 150 SX",
            "construction_year_start": 2021,
            "construction_year_end": 2022,
            "affected_vehicles_worldwide": 5806.0,
            "affected_vehicles_germany": 104.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Defektes Zündsteuergerät führt zum Bruch des Pleuels. Es besteht erhöhte Sturzgefahr.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Austausch des Zündsteuergeräts.",
            "known_incidents": "Es sind derzeit 2 Schadenereignisse mit Unfallfolge oder Personenschäden bekannt.",
            "release_date": "2022-07-11",
            "support_hotline": "0962892110",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        }
]
```
{% endswagger-response %}
{% endswagger %}

The Fusionbase API offers a powerful filtering capability to refine and narrow down the data retrieved from a data stream. This is achieved using the `filters` parameter within the `query_parameters` in your request.

### Using Filters in Data Streams

#### Request Structure

To apply filters to your data retrieval, include the `filters` parameter within the `query_parameters` in your GET request to `/api/v2/stream/data/{STREAM_ID}`. The `filters` parameter should be formatted as a JSON object.

#### Filter Properties

The `filters` JSON object comprises an array of filter objects, each containing the following properties:

* **`property`**: The column name in the data stream that you want to filter on.
* **`operator`**: The operation to be used for filtering. Common operators include `EQUALS`, `GREATER_THAN`, `LESS_THAN`, etc.
* **`value`**: The value to compare against the column data.

#### Multiple Filters

You can apply multiple filters within the same request. Each filter in the array applies to the specified property independently.

#### Example

Suppose you want to filter a data stream to include records where the `construction_year_end` is later than 2013. The request would be structured as follows:

```http
/api/v2/stream/data/{STREAM_ID}?query_parameters={"filters": [{"property": "construction_year_end", "operator": "GREATER_THAN", "value": 2013}]}&format=json
```

#### Notes

* Ensure that the property names used in filters correspond to valid column names in the data stream.
* The filter's `value` should be appropriate for the property's data type (e.g., numeric, string, date).
* Filters are case-sensitive and should match the exact casing of column names and operators.

#### Error Handling

Errors may occur if the filter criteria are not properly structured, if invalid column names are used, or if there is a data type mismatch. In such cases, the API might return an error response.

{% hint style="info" %}
Make sure to properly URL encode the query parameters!
{% endhint %}
