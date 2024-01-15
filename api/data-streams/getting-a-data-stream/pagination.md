---
description: >-
  Some Data Streams are too large to get all the records via the API in a single
  call. In such cases, the API lets you paginate the results via the skip and
  limit parameters.
---

# Pagination

{% swagger method="get" path="/{STREAM_ID}?skip=0&limit=2&format=json" baseUrl="https://api.fusionbase.com/api/v2/stream/data" summary="Pagination" expanded="true" %}
{% swagger-description %}
Some Data Streams are too large to get all the records via the API in a single call. In such cases the API lets you paginate the results via the `skip` and `limit`parameters.
{% endswagger-description %}

{% swagger-parameter in="path" name="STREAM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Integer" %}
Indicates the number of records that are retrieved in the result set.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="skip" type="Integer" %}
Indicates the number of records that should be skipped from the beginning of the Data Stream.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="format" type="String" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
This is a sample data response, the response is always a list of dicts. If format is not explicitly specified, the default response format is msgpack.

```json
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

#### Understanding `skip` and `limit` Parameters

When querying a data stream, you can control the subset of records you retrieve by using `skip` and `limit` parameters. These parameters are crucial for pagination and managing large datasets.

* **`skip` Parameter**: This parameter defines the number of records to skip before starting to collect the result set. It is used to bypass a specific number of entries at the beginning of the data stream. For example, `skip=10` means the first 10 records in the data stream will be skipped.
* **`limit` Parameter**: This parameter specifies the maximum number of records to include in the result set. It effectively limits the size of your data retrieval. For instance, `limit=10` means only 10 records will be retrieved after applying the `skip` parameter.

#### Example

Consider a data stream containing a sequence of records represented by numbers:

`[`<mark style="color:blue;">`1`</mark>`,`<mark style="color:blue;">`2`</mark>`,`<mark style="color:blue;">`3`</mark>`,`<mark style="color:blue;">`4`</mark>`,`<mark style="color:blue;">`5`</mark>`,`<mark style="color:blue;">`6`</mark>`,`<mark style="color:blue;">`7`</mark>`,`<mark style="color:blue;">`8`</mark>`,`<mark style="color:blue;">`9`</mark>`,`<mark style="color:blue;">`10`</mark>`,`<mark style="color:red;">`11`</mark>`,`<mark style="color:red;">`12`</mark>`,`<mark style="color:red;">`13`</mark>`,`<mark style="color:red;">`14`</mark>`,`<mark style="color:red;">`15`</mark>`,`<mark style="color:red;">`16`</mark>`,`<mark style="color:red;">`17`</mark>`,`<mark style="color:red;">`18`</mark>`,`<mark style="color:red;">`19`</mark>`,`<mark style="color:red;">`20`</mark>`,21,22,23,24,25,...]`

Applying the query parameters `?skip=`<mark style="color:blue;">10</mark>`&limit=`<mark style="color:red;">10</mark>:

1. **Skip**: The first 10 records (`1` to `10`) are skipped.
2. **Limit**: The next 10 records after the skipped ones are included in the result set. These are records `11` to `20`.

So, the resulting dataset retrieved with these parameters will be:

`[11,12,13,14,15,16,17,18,19,20]`

These parameters are particularly useful in scenarios where you need to handle large volumes of data, allowing for efficient data access and reducing the load on both the server and client sides.\
