# Streams

The Data Stream API lets you conveniently access data and metadata of all Data Streams. Each stream can be accessed via its unique stream id.&#x20;

{% hint style="info" %}
You can access almost any available Data Stream even if you are not subscribed to it!

The API provides most functions but is limited to a maximum of 10 records per stream.
{% endhint %}

## TL;DR <a href="#tldr" id="tldr"></a>

The Fusionbase API provides a streamlined way to interact with data streams, offering three primary endpoints: `/base`, `/data`, and `/search`. Each endpoint serves a distinct purpose, enabling users to access metadata, retrieve and query data, and perform searches within the data streams.

For the `/data` and `/search`  endpoints the default response type is msgpack, JSON is available using the GET parameter `format=json`

{% tabs %}
{% tab title="Python (Base)" %}
```python
import requests

headers = {
    'x-api-key': 'YOUR_API_KEY',
}

response = requests.get('https://api.fusionbase.com/api/v2/stream/base/STREAM_ID', headers=headers, params=params)
assert response.code == 200, "CONNECTION_ERROR"

data_stream = response.json()
print(data_stream)
```
{% endtab %}

{% tab title="Python (Data)" %}
```python
import requests
import json

# Replace 'YOUR_API_KEY' with your actual API key
headers = {
    'x-api-key': 'YOUR_API_KEY',
}

# Define the query parameters
params = {
    'skip': '1000',
    'limit': '5',
    'version_boundary': 'bd764e07-50a1-4c0e-a102-ae1a259557av',
    'format': 'json',
    'query_parameters': json.dumps({
        "sort_order": ["asc"],
        "sort_keys": ["fb_datetime"],
        "skip": 0,
        "limit": 2,
        "project_fields": ["manufacturer_brand", "recall_supervised_by_kba"],
        "filters": [{
            "property": "construction_year_end",
            "operator": "GREATER_THAN",
            "value": 2013
        }]
    })
}

# The URL for the GET request
url = 'https://api.fusionbase.com/api/v2/stream/data/430410'

# Make the GET request
response = requests.get(url, headers=headers, params=params)

# Check if the request was successful
assert response.status_code == 200, "CONNECTION_ERROR"

# Parse the response to JSON
data = response.json()

# Example of how to use the data
print(data)
```
{% endtab %}

{% tab title="Python (Search)" %}
```python
import requests

# Replace 'YOUR_API_KEY' with your actual API key
headers = {
    'x-api-key': 'YOUR_API_KEY',
}

# Define the query parameters
params = {
    'q': 'LAMBORGHINI',  # Your search query
    'format': 'json'      # Specify the response format
}

# Construct the full URL for the GET request
url = f'https://api.fusionbase.com/api/v2/stream/data/search/430410'

# Make the GET request
response = requests.get(url, headers=headers, params=params)

# Check if the request was successful
assert response.status_code == 200, "CONNECTION_ERROR"

# Parse the response to JSON
search_results = response.json()

# Example of how to use the data
print(search_results)

```
{% endtab %}
{% endtabs %}

## Data Structure

All Data Streams in Fusionbase, independent of the source or content follow the same data structure.&#x20;

The table below gives you a full overview of all attributes, their description and data types.

<table><thead><tr><th width="247.33333333333331">Name</th><th width="290.52023692003957">Description</th><th>Data Type</th></tr></thead><tbody><tr><td>_key</td><td>Unique identifier of the stream</td><td>String</td></tr><tr><td>name</td><td>Name of the stream by language</td><td>Object</td></tr><tr><td>description</td><td>Description of the stream by language</td><td>Object</td></tr><tr><td>meta</td><td>Various meta information:<br><code>is_active</code>: Stream still actively updated<br><br><code>entry_count</code>: Number of records<br><br><code>main_property_count</code>: Number of properties, i.e., columns</td><td>Object</td></tr><tr><td>source</td><td>Information about the data source</td><td>Object</td></tr><tr><td>data_item_collections</td><td>Data property and schema information. This contains attribute names and the corresponding data types</td><td>List</td></tr><tr><td>data_version</td><td>The current version of the data</td><td>String</td></tr><tr><td>data_updated_at</td><td>The date when the data of the stream has been last updated</td><td>Datetime</td></tr><tr><td>data</td><td>The data of the stream based on the query input</td><td>List</td></tr><tr><td>created_at</td><td>Date and time when the stream was created on Fusionbase</td><td>Datetime</td></tr><tr><td>updated_at</td><td>Date and time when the stream was last updated either by it's data or metadata</td><td>Datetime</td></tr></tbody></table>



#### Example

Let's assume we would like to retrieve the latest automotive recalls in Germany. Luckily, we found a real-time[ automotive recalls Data Stream](https://fusionbase.com/en/data/stream/430410/Germany:%20Automotive%20Recalls) in the Fusionbase Data Hub.

The Data Stream API will return the stream in the above mentioned structure which looks like the following:

```json
{
    "id": "data_streams/430410",
    "key": "430410",
    "name": {
        "en": "Germany: Automotive Recalls"
    },
    "description": {
        "en": "This extensive database presents a systematic compilation of all automotive recalls issued in Germany from 1995 to the current date. Encompassing a wide range of vehicles and car accessories, the catalog serves as a vital tool for researchers, automotive engineers, policy makers, and safety analysts. Each entry in the database is meticulously documented, providing detailed information on the nature of the defect, the models and components affected, and the remedial actions recommended. By offering a chronological and categorical organization of the data, the catalog facilitates trend analysis and retrospective studies, contributing to the broader understanding of automotive safety and reliability over time. The aim of this resource is to support scientific inquiry and data-driven decision-making in the field of automotive safety, ensuring a safer and more informed vehicular environment in Germany."
    },
    "meta": {
        "entry_count": 8119,
        "main_property_count": 18
    },
    "created_at": "2021-06-16T12:16:18.367162",
    "updated_at": "2023-10-26T22:35:11.296891",
    "data_item_collections": [
        {
            "id": "data_item_collections/430413",
            "key": "430413",
            "name": "fb_id",
            "description": {
                "en": "The ID is a unique fingerprint that is calculated based on the values of all non-Fusionbase specific columns."
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.525145",
            "updated_at": "2021-06-18T10:46:26.888675",
            "definition": {
                "en": "Unique record identifier and primary key"
            },
            "basic_data_type": "String",
            "semantic_type": "Fusionbase ID",
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430416",
            "key": "430416",
            "name": "kba_reference_number",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.528423",
            "updated_at": "2021-06-18T10:46:26.894534",
            "definition": {
                "en": "Unique reference number provided by Kraftfahrtbundesamt"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430419",
            "key": "430419",
            "name": "manufacturer_recall_code",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.531390",
            "updated_at": "2021-06-18T10:46:26.900453",
            "definition": {
                "en": "Unique recall code provided by the manufacturer"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430422",
            "key": "430422",
            "name": "manufacturer_brand",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.533831",
            "updated_at": "2021-06-18T10:46:26.906302",
            "definition": {
                "en": "Manufacturer brand of the car or accessory"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430425",
            "key": "430425",
            "name": "trade_name",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.536421",
            "updated_at": "2021-06-18T10:46:26.912149",
            "definition": {
                "en": "Trade name of the car or accessory"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430428",
            "key": "430428",
            "name": "construction_year_start",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.538828",
            "updated_at": "2021-06-18T10:46:26.918302",
            "definition": {
                "en": "Year when construction began"
            },
            "basic_data_type": "int",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430431",
            "key": "430431",
            "name": "construction_year_end",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.541334",
            "updated_at": "2021-06-18T10:46:26.924226",
            "definition": {
                "en": "Year when construction ended"
            },
            "basic_data_type": "int",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430434",
            "key": "430434",
            "name": "affected_vehicles_worldwide",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.543767",
            "updated_at": "2021-06-18T10:46:26.930195",
            "definition": {
                "en": "Number of vehicles affected worldwide"
            },
            "basic_data_type": "float",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430437",
            "key": "430437",
            "name": "affected_vehicles_germany",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.546184",
            "updated_at": "2021-06-18T10:46:26.936015",
            "definition": {
                "en": "Number of vehicles affected in Germany"
            },
            "basic_data_type": "float",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430440",
            "key": "430440",
            "name": "recall_supervised_by_kba",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.548717",
            "updated_at": "2021-06-18T10:46:26.941896",
            "definition": {
                "en": "Note whether the recall is supervised by Kraftfahrtbundesamt"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430443",
            "key": "430443",
            "name": "recall_description",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.551148",
            "updated_at": "2021-06-18T10:46:26.947900",
            "definition": {
                "en": "Description of the recall"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430446",
            "key": "430446",
            "name": "possible_restrictions_of_the_affected_vehicle_versions",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.553559",
            "updated_at": "2021-06-18T10:46:26.954070",
            "definition": {
                "en": "Possible restrictions that apply to the affected vehicle version"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430449",
            "key": "430449",
            "name": "remedial_action_by_the_manufacturer",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.556197",
            "updated_at": "2021-06-18T10:46:26.960045",
            "definition": {
                "en": "Remedial action suggested by the manufacturer"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430452",
            "key": "430452",
            "name": "known_incidents",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.558624",
            "updated_at": "2021-06-18T10:46:26.966000",
            "definition": {
                "en": "Known incidents in connection to the recall"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430455",
            "key": "430455",
            "name": "release_date",
            "description": {
                "en": ""
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.561138",
            "updated_at": "2021-06-18T10:46:26.971823",
            "definition": {
                "en": "Release date of the recall"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430458",
            "key": "430458",
            "name": "support_hotline",
            "description": {
                "en": "This can be from Kraftfahrtbundesamt, the manufacturer or other separate organizations. "
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.563631",
            "updated_at": "2021-06-18T10:46:26.977575",
            "definition": {
                "en": "Support hotline for the recall"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430461",
            "key": "430461",
            "name": "fb_datetime",
            "description": {
                "en": "The ISO-8601 timestamp of when the record was added to the data stream."
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.566053",
            "updated_at": "2021-06-18T10:46:26.983433",
            "definition": {
                "en": "Timestamp of the record"
            },
            "basic_data_type": "Datetime",
            "semantic_type": "Datetime",
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        },
        {
            "id": "data_item_collections/430464",
            "key": "430464",
            "name": "fb_data_version",
            "description": {
                "en": "Each new set of records that is added to the data stream gets automatically versioned by Fusionbase."
            },
            "meta": null,
            "created_at": "2021-06-16T12:16:18.568492",
            "updated_at": "2021-06-18T10:46:26.994883",
            "definition": {
                "en": "Version of the record"
            },
            "basic_data_type": "String",
            "semantic_type": null,
            "semantic_tags": null,
            "data_streams": [
                "data_streams/430410"
            ]
        }
    ],
    "source": {
        "_id": "data_sources/47480",
        "stream_specific": {
            "uri": "https://www.kba.de/DE/Home/home_node.html"
        }
    },
    "store_version": null,
    "data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b",
    "data_updated_at": "2023-06-16T10:41:28.220833",
    "data": [
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
        },
        {
            "fb_id": "619f71bc1d0ddf6c7231af2019ecc2a4",
            "kba_reference_number": "012715",
            "manufacturer_recall_code": "21DC10",
            "manufacturer_brand": "HYUNDAI",
            "trade_name": "I30",
            "construction_year_start": 2021,
            "construction_year_end": 2022,
            "affected_vehicles_worldwide": 5858.0,
            "affected_vehicles_germany": 2958.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Durch eine nicht korrekte Ansteuerung der elektrischen Ölpumpe kann es zu einem plötzlichen Druckabfall im Doppelkupplungsgetriebe kommen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "i30 N",
            "remedial_action_by_the_manufacturer": "Ein Softwareupdate wird auf das Getriebesteuergerät (TCU) aufgespielt.",
            "known_incidents": "unknown",
            "release_date": "2023-04-27",
            "support_hotline": "069380767212",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "a80368505a45f8576b1eceea1e72138d",
            "kba_reference_number": "012543",
            "manufacturer_recall_code": "0061460700",
            "manufacturer_brand": "BMW",
            "trade_name": "7, I7",
            "construction_year_start": 2022,
            "construction_year_end": 2023,
            "affected_vehicles_worldwide": 6074.0,
            "affected_vehicles_germany": 580.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Softwarefehler kann zum Verlust der Initialisierung des Airbagsteuergeräts führen. Dadurch würden die Beifahrer-Front- und Knieairbags sowie die aktive Kopfstütze deaktiviert werden.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Die Steuergeräte (Sitzmodul) werden programmiert.",
            "known_incidents": "unknown",
            "release_date": "2023-02-23",
            "support_hotline": "089125016175",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "439068622105a7c514cd708e2094eaad",
            "kba_reference_number": "012459",
            "manufacturer_recall_code": "26V",
            "manufacturer_brand": "IVECO",
            "trade_name": "S-WAY, T-WAY, X-WAY",
            "construction_year_start": 2022,
            "construction_year_end": 2022,
            "affected_vehicles_worldwide": 6279.0,
            "affected_vehicles_germany": 622.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Eine nicht konforme geschweißte Zylinderhalterung an der Kippkabine kann brechen, wenn das Fahrerhaus gekippt wird.",
            "possible_restrictions_of_the_affected_vehicle_versions": "Wenn die Chargenkennzeichnung eine der folgenden ist: NF3 / NF4 / NF5 / NG1 / NG2 / NG3 / NH4 / NH5 / NJ1 / NJ2 / NJ3 / NJ4 / NK1 / NK2 / NK3 / NK4 , muss die Halterung des Zylinders getauscht werden. Ist die Chargenkennzeichnung nicht sichtbar oder können die Zeichen nicht gelesen werden, sit die Halterung des Zylinders auszutauschen.",
            "remedial_action_by_the_manufacturer": "Überprüfung und ggf. Austausch der Halterung der Kippvorrichtung.",
            "known_incidents": "no_incidents",
            "release_date": "2023-02-09",
            "support_hotline": "07314085419",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "b71c2f87c3f4a6310dce0fb54931991f",
            "kba_reference_number": "012284",
            "manufacturer_recall_code": "F-PEDAL",
            "manufacturer_brand": "MERCEDES-BENZ LKW",
            "trade_name": "E-ACTROS, E-AROCS, E- ATEGO",
            "construction_year_start": 2021,
            "construction_year_end": 2022,
            "affected_vehicles_worldwide": 6349.0,
            "affected_vehicles_germany": 2069.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Kurzschluss im Fahrpedalsensor kann zur Unterbrechung der Gasannahme und in der Folge zum Liegenbleiben des Fahrzeugs führen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Überprüfung und ggf. Erneuerung des Fahrpedals",
            "known_incidents": "no_incidents",
            "release_date": "2022-11-16",
            "support_hotline": "+49 69 95 30 73 89",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "0cd42887e6f0bc92f1b52e80fe564632",
            "kba_reference_number": "002000",
            "manufacturer_recall_code": "4",
            "manufacturer_brand": "MASERATI",
            "trade_name": "CILINDRI, GHIBLI, QUATTROPORTE",
            "construction_year_start": 1995,
            "construction_year_end": 1997,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 110.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Möglicher Kraftstoffverlust an den Kraftstoffleitungen im Motorraum",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2008-01-01",
            "support_hotline": null,
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "5e7bee1560328da245cd993fc9c77355",
            "kba_reference_number": "001909",
            "manufacturer_recall_code": "163",
            "manufacturer_brand": "MASERATI",
            "trade_name": "QUATTROPORTE",
            "construction_year_start": 2007,
            "construction_year_end": 2007,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 71.0,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Fehlerhafte Software führt zum Ausfall des Stabilitätsprogramms.",
            "possible_restrictions_of_the_affected_vehicle_versions": "Automatic",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2008-01-01",
            "support_hotline": null,
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "830fd08351a6ce095a2677500357c8f9",
            "kba_reference_number": "003195",
            "manufacturer_recall_code": "R29162",
            "manufacturer_brand": "VOLVO",
            "trade_name": "C30, S40, S60, S80, V50, V60, V70, XC60, XC70",
            "construction_year_start": 2010,
            "construction_year_end": 2010,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 291.0,
            "recall_supervised_by_kba": "unsupervised",
            "recall_description": "Bei Fahrzeugen mit einem Fünfzylinder-Diesel (D5) sind die Kraftstoffleitungen möglicherweise nicht mit dem erforderlichen Anzugsdrehmoment am Motor befestigt worden. Dies kann zu Kraftstoffaustritt und folglich  zu einer verminderten Motorleistung und Kraftstoffgeruch führen. Ausgetretener Kraftstoff kann sich im Motorraum entzünden.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2011-02-21",
            "support_hotline": "02219393393",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        },
        {
            "fb_id": "74044378a70c356b2c570f1dba775b30",
            "kba_reference_number": "008428",
            "manufacturer_recall_code": "18SMD-113",
            "manufacturer_brand": "LEXUS",
            "trade_name": "LS",
            "construction_year_start": 2017,
            "construction_year_end": 2018,
            "affected_vehicles_worldwide": 6600.0,
            "affected_vehicles_germany": 9.0,
            "recall_supervised_by_kba": "unsupervised",
            "recall_description": "Durch eine fehlerhafte Berechnung der Luftmasse kann es unter bestimmten Bedingungen bei der Verwendung  des Start-Stopp-Systems zum Absterben des Motors kommen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "LS500",
            "remedial_action_by_the_manufacturer": "Neuprogrammierung des Motor-Steuergeräts",
            "known_incidents": "unknown",
            "release_date": "2018-12-12",
            "support_hotline": "022341026111",
            "fb_datetime": "2023-06-16T10:41:28.220000",
            "fb_data_version": "3e727ad8-5cf0-43a9-ae28-88c08421187b"
        }
    ]
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
