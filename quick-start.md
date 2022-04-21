# Quick Start

## Get your API keys&#x20;

API requests are authenticated using API keys which are sent via the custom `X-API-KEY` header. Any request that doesn't include an API key will return an authentication error.

You can view and manage your API keys at the [profile settings](https://app.fusionbase.com/account/profile) page in your Fusionbase account.

{% hint style="info" %}
The base URL for all API requests is

[**https://api.fusionbase.com/api/v1**](https://api.fusionbase.com/api/v1) ****&#x20;
{% endhint %}

## Retrieving a Data Stream

A core value of Fusionbase is to make live easier for data engineers and offer a rich and easy-to-use API endpoint to our Data Streams.

Any datasets that are provided by Fusionbase can be retrieved and queried using a single API endpoint.

{% swagger method="get" path="/data-stream/get/[Stream ID]" baseUrl="https://api.fusionbase.com/api/v1" summary="Retrieving a Data Stream" %}
{% swagger-description %}
This endpoint returns the data and metadata of the specified Data Stream.
{% endswagger-description %}

{% swagger-parameter in="path" name="Stream ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="skip" type="Number" %}
Indicates the number of records that should be skipped from the beginning of the Data Stream.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Number" %}
Indicates the number of records that are retrieved in the result set.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="fields" type="List" %}
The name of the projected column, i.e., the fields that should exist in the result set.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort_keys" type="List" %}
The column names for which the Data Stream should be sorted by.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort_order" %}
The order how the Data Stream should be sorted. This is either 

`desc`

 or 

`asc`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="compressed_file" type="Bool" %}
Indicates wether to generated a gezipped compressed file or not. Default is 

`false`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
{% code title="Sample Response for Stream Automotive Recalls" %}
```javascript
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
    "store_version": null,
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
        },
        {
            "fb_id": "ffea05a076730ce4bd1ca396455a605b",
            "kba_reference_number": "007744",
            "manufacturer_recall_code": "0CPW",
            "manufacturer_brand": "RENAULT",
            "trade_name": "CAPTUR, CLIO, ZOE",
            "construction_year_start": 2017,
            "construction_year_end": 2017,
            "affected_vehicles_worldwide": 2642,
            "affected_vehicles_germany": 258,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Durch fehlerhafte Produktion besteht die Möglichkeit einer Rissbildung mit Bruchgefahr der vorderen Radnaben.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Konformitätsprüfung der Vorderradnabe und ggf. Austausch",
            "known_incidents": "unknown",
            "release_date": "2018-03-23",
            "support_hotline": "contact_manufacturer_or_authorized_workshop",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },         {
            "fb_id": "ffe2aa55ce2677d3b8b9c23aa93f3a0a",
            "kba_reference_number": "010841",
            "manufacturer_recall_code": "AMB2",
            "manufacturer_brand": "PORSCHE",
            "trade_name": "PANAMERA, TAYCAN",
            "construction_year_start": 2021,
            "construction_year_end": 2021,
            "affected_vehicles_worldwide": 3052,
            "affected_vehicles_germany": 134,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Es kann zu einem Bruch des unteren Achslenkers kommen. In der Folge kann eine Beeinträchtigung des Fahrverhaltens und ein potenzieller Kontrollverlust auftreten.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": "Maßnahme: Achslenker an der Vorderachse unten rechts austauschen.",
            "known_incidents": "unknown",
            "release_date": "2021-06-09",
            "support_hotline": "contact_manufacturer_or_authorized_workshop",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ffc66b4952fa6484a0832ee41593f1d9",
            "kba_reference_number": "002295",
            "manufacturer_recall_code": "H23",
            "manufacturer_brand": "CHRYSLER",
            "trade_name": "300C",
            "construction_year_start": 2007,
            "construction_year_end": 2008,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 311,
            "recall_supervised_by_kba": "unsupervised",
            "recall_description": "Die Befestigungsmuttern der Achsantriebswelle können sich lockern und die Antriebswelle kann sich in der Folge von der Radnabe lösen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2008-07-28",
            "support_hotline": null,
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ffc12418d0253afddbb4267b7a2fc08f",
            "kba_reference_number": "006281",
            "manufacturer_recall_code": "S26",
            "manufacturer_brand": "FIAT",
            "trade_name": "500E",
            "construction_year_start": 2012,
            "construction_year_end": 2016,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": null,
            "recall_supervised_by_kba": "unsupervised",
            "recall_description": "Fehlerhafte Diagnose durch das Power Inverter Modul (PIM) kann zu Fehlermeldungen und Leistungsverlust des elektrischen Antriebs führen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2016-07-20",
            "support_hotline": "contact_vehicle_importer_or_dealer",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ffba89a6322fba51f3a7a2bfc49468aa",
            "kba_reference_number": "006657",
            "manufacturer_recall_code": "4694134",
            "manufacturer_brand": "MERCEDES-BENZ",
            "trade_name": "C-KLASSE, E-KLASSE, S-KLASSE, SL",
            "construction_year_start": 2012,
            "construction_year_end": 2015,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 66,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Fehlerhafte Verschweißung der Kontakte im Steuergerät kann zum Brand oder Ausfall der Servolenkung führen.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2016-12-29",
            "support_hotline": "0080012777777",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ff9f9b6e4680e60e3d7f02a562f635e5",
            "kba_reference_number": "003027",
            "manufacturer_recall_code": "R1020",
            "manufacturer_brand": "NISSAN",
            "trade_name": "X-TRAIL, PATROL",
            "construction_year_start": 2000,
            "construction_year_end": 2001,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 1,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Nicht korrekte Entfaltung des Beifahrerairbags.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2010-08-25",
            "support_hotline": "02232572080",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ff910f6ce3e72a2edea187bf8af698d4",
            "kba_reference_number": "002016",
            "manufacturer_recall_code": "7K1A-455",
            "manufacturer_brand": "LEXUS",
            "trade_name": "GS, IS",
            "construction_year_start": 2004,
            "construction_year_end": 2005,
            "affected_vehicles_worldwide": null,
            "affected_vehicles_germany": 1495,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Unzureichende Schweißnaht an den Kraftstoffleitungen kann zum Austritt von Kraftstoff führen. Betroffen sind Fahrzeuge GS300 und IS250.",
            "possible_restrictions_of_the_affected_vehicle_versions": "unknown",
            "remedial_action_by_the_manufacturer": null,
            "known_incidents": "unknown",
            "release_date": "2008-01-01",
            "support_hotline": "0800539877378423",
            "fb_datetime": "2021-06-16T12:16:18.371000",
            "fb_data_version": "ac62a53c-b657-4447-ae3a-70fa29c8bf0e"
        },
        {
            "fb_id": "ff8a36d98940feb98b6c12756cb48774",
            "kba_reference_number": "010892",
            "manufacturer_recall_code": "N571",
            "manufacturer_brand": "LAND ROVER",
            "trade_name": "DEFENDER, DISCOVERY",
            "construction_year_start": 2020,
            "construction_year_end": 2021,
            "affected_vehicles_worldwide": 8389,
            "affected_vehicles_germany": null,
            "recall_supervised_by_kba": "supervised",
            "recall_description": "Aufgrund eines fehlerhaften Abgassystems besteht erhöhte Brandgefahr.",
            "possible_restrictions_of_the_affected_vehicle_versions": null,
            "remedial_action_by_the_manufacturer": "Auspuffbefestigungen festziehen.",
            "known_incidents": "ein Fahrzeugbrand bekannt",
            "release_date": "2021-06-18",
            "support_hotline": "03453032303",
            "fb_datetime": "2021-08-03T09:52:32.645000",
            "fb_data_version": "40983fe7-bcb5-4584-8e30-51a935f7aabf"
        }
    ],
    "created_at": "2021-06-16T12:16:18.367162",
    "updated_at": "2021-12-17T09:30:01.570181"
}
```
{% endcode %}
{% endswagger-response %}
{% endswagger %}

### Code Snippets

Below you find some sample code on how to call the Fusionbase Data Stream API using common languages.

{% hint style="info" %}
Want to have to data directly in Pandas?\


**Stay tuned!** \
We are releasing a Python package to interact natively with Fusionbase soon!
{% endhint %}

{% tabs %}
{% tab title="curl" %}
```bash
curl -X GET "https://api.fusionbase.com/api/v1/data-stream/get/430410" \
-H 'X-API-KEY: YOUR_API_KEY' \
-H 'Content-Type: application/json; charset=utf-8' \

```
{% endtab %}

{% tab title="Python" %}
```python
import requests

headers = {
    'X-API-KEY': 'YOUR_API_KEY',
    'Content-Type': 'application/json; charset=utf-8',
}

response = requests.get('https://api.fusionbase.com/api/v1/data-stream/get/430410', headers=headers)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.io.IOException;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

class Main {

	public static void main(String[] args) throws IOException {
		URL url = new URL("https://api.fusionbase.com/api/v1/data-stream/get/430410");
		HttpURLConnection httpConn = (HttpURLConnection) url.openConnection();
		httpConn.setRequestMethod("GET");

		httpConn.setRequestProperty("X-API-KEY", "YOUR_API_KEY");
		httpConn.setRequestProperty("Content-Type", "application/json; charset=utf-8");

		InputStream responseStream = httpConn.getResponseCode() / 100 == 2
				? httpConn.getInputStream()
				: httpConn.getErrorStream();
		Scanner s = new Scanner(responseStream).useDelimiter("\\A");
		String response = s.hasNext() ? s.next() : "";
		System.out.println(response);
	}
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	client := &http.Client{}
	req, err := http.NewRequest("GET", "https://api.fusionbase.com/api/v1/data-stream/get/430410", nil)
	if err != nil {
		log.Fatal(err)
	}
	req.Header.Set("X-API-KEY", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json; charset=utf-8")
	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close()
	bodyText, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s\n", bodyText)
}
```
{% endtab %}

{% tab title="Node" %}
```javascript
var fetch = require('node-fetch');

fetch('https://api.fusionbase.com/api/v1/data-stream/get/430410', {
    headers: {
        'X-API-KEY': 'YOUR_API_KEY',
        'Content-Type': 'application/json; charset=utf-8'
    }
});
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-API-KEY' => 'YOUR_API_KEY',
    'Content-Type' => 'application/json; charset=utf-8'
);
$response = Requests::get('https://api.fusionbase.com/api/v1/data-stream/get/430410', $headers);
```
{% endtab %}
{% endtabs %}
