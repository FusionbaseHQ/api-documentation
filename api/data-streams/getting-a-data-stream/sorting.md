---
description: >-
  The Data Stream can be sorted by any column in ascending or descending order.
  Sorting by multiple columns at once is also possible.
---

# Sorting

{% swagger method="get" path="/{STREAM_ID}?query_parameters={"sort_order": ["asc"], "sort_keys": ["fb_datetime"]}&format=json" baseUrl="https://api.fusionbase.comapi/v2/stream/data" summary="Sorting" expanded="true" %}
{% swagger-description %}
The Data Stream can be sorted by any column in ascending or descending order. Sorting by multiple columns at once is also possible.
{% endswagger-description %}

{% swagger-parameter in="path" name="STREAM_ID" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="format" type="String" %}
json or msgpack
{% endswagger-parameter %}

{% swagger-parameter in="query" type="Object" name="query_parameters" %}
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

### Overview

The Fusionbase API allows users to sort data within a stream using the `query_parameters` parameter in the request. This feature enables efficient organization and retrieval of data based on specific sorting criteria.

### Sorting Data in a Stream

#### Request Structure

To sort data in a stream, include the `query_parameters` parameter in your GET request to `/api/v2/stream/data/{STREAM_ID}`. This parameter accepts JSON-formatted sorting instructions.

#### Sorting Properties

The `query_parameters` JSON object can contain the following properties related to sorting:

* **`sort_keys`**: A list of column names by which the data should be sorted. These keys should correspond to columns in the data stream.
* **`sort_order`**: Specifies the sorting order for each key in `sort_keys`. The available options are `"asc"` for ascending order and `"desc"` for descending order.

#### Multiple Sorting Keys

You can specify multiple sort keys in the `sort_keys` list. The data will be sorted based on the order of keys provided. The corresponding `sort_order` list should match the length of `sort_keys` to specify the sorting order for each key.

#### Example

Consider a data stream with columns such as `fb_datetime`, `price`, and `product_name`. To sort the data first by `fb_datetime` in ascending order and then by `price` in descending order, structure your request as follows:

```http
/api/v2/stream/data/{STREAM_ID}?query_parameters={"sort_keys": ["fb_datetime", "price"], "sort_order": ["asc", "desc"]}&format=json
```

#### Note

* The sorting functionality is dependent on the structure of the data stream.
* Ensure that the column names used in `sort_keys` are valid and present in the data stream.
* The order of elements in `sort_keys` and `sort_order` is significant and should correspond to each other.

#### Error Handling

If invalid column names are used or if there's a mismatch in the length of `sort_keys` and `sort_order`, the API might return an error response.
