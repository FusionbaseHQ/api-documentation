# Data Streams

The Data Stream API lets you conveniently access data and metadata of all Data Streams. Each stream can be accessed via its unique stream id.&#x20;

{% hint style="info" %}
You can access almost any available Data Stream even if you are not subscribed to it!

The API provides most functions but is limited to a maximum of 10 records per stream.
{% endhint %}

## TL;DR <a href="#tldr" id="tldr"></a>

If you don't feel like reading the whole description and just want to try out our API feel free to use the following code snippet.

{% tabs %}
{% tab title="Python" %}
```python
import requests

headers = {
    'x-api-key': 'YOUR_API_KEY',
}

# COLUMN_NAME and COLUMN_VALUE are placeholders for
# the actual column names and values in the Data Stream
# STREAM_ID is the ID of the Data Stream
params = (
    ('skip', '0'),
    ('limit', '500'),
    ('sort_keys', ['fb_datetime', 'COLUMN_NAME']),
    ('sort_order', ['desc', 'asc']),
    ('query', '{"COLUMN_NAME": "COLUMN_VALUE", "COLUMN_NAME": "COLUMN_NAME"}'),
    ('fields', ['fb_id', 'COLUMN_NAME', 'COLUMN_NAME']),
    ('compressed_file', 'false')
)

response = requests.get('https://api.fusionbase.com/api/v1/data-stream/get/STREAM_ID', headers=headers, params=params)
assert response.code == 200, "CONNECTION_ERROR"

data_stream = response.json()
data = data_stream["data"]
```
{% endtab %}
{% endtabs %}

## Data Structure

All Data Streams in Fusionbase, independent of the source or content follow the same data structure.&#x20;

The table below gives you a full overview of all attributes, their description and data types.

| Name                    | Description                                                                                                                                                                                                                                 | Data Type |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| \_key                   | Unique identifier of the stream                                                                                                                                                                                                             | String    |
| name                    | Name of the stream by language                                                                                                                                                                                                              | Object    |
| description             | Description of the stream by language                                                                                                                                                                                                       | Object    |
| meta                    | <p>Various meta information:<br><code>is_active</code>: Stream still actively updated<br><br><code>entry_count</code>: Number of records<br><em></em><br><em></em><code>main_property_count</code>: Number of properties, i.e., columns</p> | Object    |
| source                  | Information about the data source                                                                                                                                                                                                           | Object    |
| data\_item\_collections | Data property and schema information. This contains attribute names and the corresponding data types                                                                                                                                        | List      |
| data\_version           | The current version of the data                                                                                                                                                                                                             | String    |
| data\_updated\_at       | The date when the data of the stream has been last updated                                                                                                                                                                                  | Datetime  |
| data                    | The data of the stream based on the query input                                                                                                                                                                                             | List      |
| created\_at             | Date and time when the stream was created on Fusionbase                                                                                                                                                                                     | Datetime  |
| updated\_at             | Date and time when the stream was last updated either by it's data or metadata                                                                                                                                                              | Datetime  |



#### Example

Let's assume we would like to retrieve the latest automotive recalls in Germany. Luckily, we found a real-time [automotive recalls Data Stream](https://app.fusionbase.com/share/25123083) in the Fusionbase Data Hub.

The Data Stream API will return the stream in the above mentioned structure which looks like the following:

```json
{
    "_key": "430410",
    "name": {
        "en": "Automotive Recalls"
    },
    "description": {
        "en": "List of all recalls for cars and car accessories starting from 1995 - present"
    },
    "meta": {
        "is_active": true,
        "entry_count": 5685,
        "main_property_count": 18
    },
    "source": {
        "_key": "47480",
        "created_at": "2021-06-10T08:02:32.888477",
        "description": null,
        "label": "Kraftfahrt-Bundesamt",
        "name": "Kraftfahrt-Bundesamt",
        "primary_uri": "https://www.kba-online.de/",
        "updated_at": "2021-06-10T08:02:32.888477",
        "stream_specific": {
            "uri": "https://www.kba.de/DE/Home/home_node.html"
        }
    },
    "data_item_collections": [
        {
            "_key": "430413",
            "name": "fb_id",
            "description": {
                "en": "The ID is a unique fingerprint that is calculated based on the values of all non-Fusionbase specific columns."
            },
            "definition": {
                "en": "Unique record identifier and primary key"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "Fusionbase ID",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.525145",
            "updated_at": "2021-06-18T10:46:26.888675"
        },
        {
            "_key": "430416",
            "name": "kba_reference_number",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Unique reference number provided by Kraftfahrtbundesamt"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.528423",
            "updated_at": "2021-06-18T10:46:26.894534"
        },
        {
            "_key": "430419",
            "name": "manufacturer_recall_code",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Unique recall code provided by the manufacturer"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.531390",
            "updated_at": "2021-06-18T10:46:26.900453"
        },
        {
            "_key": "430422",
            "name": "manufacturer_brand",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Manufacturer brand of the car or accessory"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.533831",
            "updated_at": "2021-06-18T10:46:26.906302"
        },
        {
            "_key": "430425",
            "name": "trade_name",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Trade name of the car or accessory"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.536421",
            "updated_at": "2021-06-18T10:46:26.912149"
        },
        {
            "_key": "430428",
            "name": "construction_year_start",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Year when construction began"
            },
            "meta": null,
            "basic_data_type": "int",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.538828",
            "updated_at": "2021-06-18T10:46:26.918302"
        },
        {
            "_key": "430431",
            "name": "construction_year_end",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Year when construction ended"
            },
            "meta": null,
            "basic_data_type": "int",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.541334",
            "updated_at": "2021-06-18T10:46:26.924226"
        },
        {
            "_key": "430434",
            "name": "affected_vehicles_worldwide",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Number of vehicles affected worldwide"
            },
            "meta": null,
            "basic_data_type": "float",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.543767",
            "updated_at": "2021-06-18T10:46:26.930195"
        },
        {
            "_key": "430437",
            "name": "affected_vehicles_germany",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Number of vehicles affected in Germany"
            },
            "meta": null,
            "basic_data_type": "float",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.546184",
            "updated_at": "2021-06-18T10:46:26.936015"
        },
        {
            "_key": "430440",
            "name": "recall_supervised_by_kba",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Note whether the recall is supervised by Kraftfahrtbundesamt"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.548717",
            "updated_at": "2021-06-18T10:46:26.941896"
        },
        {
            "_key": "430443",
            "name": "recall_description",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Description of the recall"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.551148",
            "updated_at": "2021-06-18T10:46:26.947900"
        },
        {
            "_key": "430446",
            "name": "possible_restrictions_of_the_affected_vehicle_versions",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Possible restrictions that apply to the affected vehicle version"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.553559",
            "updated_at": "2021-06-18T10:46:26.954070"
        },
        {
            "_key": "430449",
            "name": "remedial_action_by_the_manufacturer",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Remedial action suggested by the manufacturer"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.556197",
            "updated_at": "2021-06-18T10:46:26.960045"
        },
        {
            "_key": "430452",
            "name": "known_incidents",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Known incidents in connection to the recall"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.558624",
            "updated_at": "2021-06-18T10:46:26.966000"
        },
        {
            "_key": "430455",
            "name": "release_date",
            "description": {
                "en": ""
            },
            "definition": {
                "en": "Release date of the recall"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.561138",
            "updated_at": "2021-06-18T10:46:26.971823"
        },
        {
            "_key": "430458",
            "name": "support_hotline",
            "description": {
                "en": "This can be from Kraftfahrtbundesamt, the manufacturer or other separate organizations. "
            },
            "definition": {
                "en": "Support hotline for the recall"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.563631",
            "updated_at": "2021-06-18T10:46:26.977575"
        },
        {
            "_key": "430461",
            "name": "fb_datetime",
            "description": {
                "en": "The ISO-8601 timestamp of when the record was added to the data stream."
            },
            "definition": {
                "en": "Timestamp of the record"
            },
            "meta": null,
            "basic_data_type": "Datetime",
            "semantic_type": "Datetime",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.566053",
            "updated_at": "2021-06-18T10:46:26.983433"
        },
        {
            "_key": "430464",
            "name": "fb_data_version",
            "description": {
                "en": "Each new set of records that is added to the data stream gets automatically versioned by Fusionbase."
            },
            "definition": {
                "en": "Version of the record"
            },
            "meta": null,
            "basic_data_type": "String",
            "semantic_type": "",
            "semantic_tags": null,
            "created_at": "2021-06-16T12:16:18.568492",
            "updated_at": "2021-06-18T10:46:26.994883"
        }
    ],
    "data_version": "50b55b54-8daa-4557-b7a8-85e6d33785cb",
    "data_updated_at": "2021-12-17T09:30:00.537631",
    "data": [
        {
            "fb_id": "ffff0d1ef73b3ba198504115b84a8f84",
            "kba_reference_number": "008266",
            "manufacturer_recall_code": "BREMS-LTG",
            "manufacturer_brand": "MERCEDES-BENZ LKW",
            "trade_name": "ECONIC",
            "construction_year_start": 2013,
            "construction_year_end": 2018,
            "affected_vehicles_worldwide": 77,
            "affected_vehicles_germany": 2,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Pneumatische Bremsleitungen können unter bestimmten Bedingungen durch Anschleifen an einer Gelenkwelle beschädigt werden.",
            "possible_restrictions_of_the_affected_vehicle_versions": "no_restrictions",
            "remedial_action_by_the_manufacturer": "Bei den betroffenen Fahrzeugen wird die Leitungsverlegung geprüft und ggf. korrigiert bzw. repariert.",
            "known_incidents": "unknown",
            "release_date": "2019-01-03",
            "support_hotline": "0080012777777",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "fff5b798f1ef9c7bad0f57b4d96af50b",
            "kba_reference_number": "009250",
            "manufacturer_recall_code": "8399002, 8399007",
            "manufacturer_brand": "MERCEDES-BENZ",
            "trade_name": "A-KLASSE, CLA",
            "construction_year_start": 2013,
            "construction_year_end": 2013,
            "affected_vehicles_worldwide": 30845,
            "affected_vehicles_germany": 14587,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Austausch des nicht vorschriftenkonformen Kältemittels",
            "possible_restrictions_of_the_affected_vehicle_versions": "no_restrictions",
            "remedial_action_by_the_manufacturer": "Umrüstung der Klimaanlage zur Verwendung des neuen Kältemittels",
            "known_incidents": "unknown",
            "release_date": "2019-09-23",
            "support_hotline": "0080012777777",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        }
    ],
    "created_at": "2021-06-16T12:16:18.367162",
    "updated_at": "2021-12-17T09:30:01.570181"
}
```



### Fusionbase Specific Columns

Fusionbase adds three additional metadata columns to each Data Stream. These columns may make your live easier in certain scenarios.



**Fusionbase ID**

The Fusionbase ID `fb_id` is a hash value based on all data values in the same row. You can see it as a primary and unique key for each record.



**Fusionbase Datetime**

The Fusionbase Datetime `fb_datetime` is an ISO formatted timestamp that represents the exact date and time when the record was added to the Data Stream by Fusionbase.



**Fusionbase Data Version**

The Fusionbase Data Version `fb_data_version` is automatically added to any new record or batch of records that are added to the Data Stream. The version corresponds to the `fb_datetime` but offers more query functionalities via the API.&#x20;
