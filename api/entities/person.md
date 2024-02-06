---
description: >-
  Person entities are structured data derived from official business registry
  resources, such as the German "Handelsregister," providing detailed and
  authenticated information about individuals.
---

# Person

The `/api/v2/entities/person/get/{FB_ENTITY_ID}` endpoint in the Fusionbase API is designed to provide structured information about specific individuals. This endpoint is crucial for accessing detailed and structured data about persons using their unique Fusionbase Entity ID (FB\_ENTITY\_ID).

#### Request Headers

* **X-API-KEY**: Your Fusionbase API key. This key is required for authenticating your access to the API.
* **Content-Type**: application/json. This header specifies the format of the request.

#### Request Structure

* The endpoint is accessed via a GET request to `https://api.fusionbase.com/api/v2/entities/person/get/{FB_ENTITY_ID}`.
* `{FB_ENTITY_ID}` should be replaced with the actual Fusionbase Entity ID of the person you are querying.

### Request Sample

{% tabs %}
{% tab title="Python" %}
```python
import requests

url = "https://api.fusionbase.com/api/v2/entities/person/get/FB_ENTITY_ID"
headers = {
    "X-API-KEY": "<YOUR API KEY>",
    "Content-Type": "application/json"
}

response = requests.get(url, headers=headers)
print(response.json())
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const axios = require('axios');

const url = "https://api.fusionbase.com/api/v2/entities/person/get/FB_ENTITY_ID";
const headers = {
    "X-API-KEY": "<YOUR API KEY>",
    "Content-Type": "application/json"
};

axios.get(url, { headers })
    .then(response => {
        console.log(response.data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class Main {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        String url = "https://api.fusionbase.com/api/v2/entities/person/get/FB_ENTITY_ID";
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("X-API-KEY", "<YOUR API KEY>")
                .header("Content-Type", "application/json")
                .GET()
                .build();

        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                .thenApply(HttpResponse::body)
                .thenAccept(System.out::println)
                .join();
    }
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    url := "https://api.fusionbase.com/api/v2/entities/person/get/FB_ENTITY_ID"
    req, _ := http.NewRequest("GET", url, nil)

    req.Header.Add("X-API-KEY", "<YOUR API KEY>")
    req.Header.Add("Content-Type", "application/json")

    res, _ := http.DefaultClient.Do(req)

    defer res.Body.Close()
    body, _ := ioutil.ReadAll(res.Body)

    fmt.Println(string(body))
}
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X GET \
"https://api.fusionbase.com/api/v2/entities/person/get/FB_ENTITY_ID" \
-H "X-API-KEY: <YOUR API KEY>" \
-H "Content-Type: application/json"
```
{% endtab %}
{% endtabs %}

### Response Structure

The response is a JSON object containing various details about the person:

* **fb\_entity\_id**: The Fusionbase Entity ID uniquely identifying the person.
* **fb\_datetime**: Timestamp indicating when the data was last updated or retrieved.
* **fb\_entity\_version**: A version identifier for the entity's data.
* **entity\_type**: Indicates the type of entity, here "PERSON".
* **entity\_subtype**: The subtype of the entity, e.g., "INDIVIDUAL".
* **updated\_at**, **created\_at**: Timestamps marking the last update and creation of the entity record.
* **external\_ids**: External identifiers associated with the person.
* **name**: An object containing various components of the person's name, such as given name, family name, maiden name (if applicable), and aliases.
* **locations**: Information about the person's known locations, such as home address, including coordinates and address components.
* **birth\_date**: The birth date of the person, including whether the exact day or only the month is known.
* **source**: Identifier of the data source from which the information is derived.
* **fb\_semantic\_id**: A semantic identifier providing a structured representation of the person's key attributes.

### Response Sample

```json
{
  "fb_entity_id": "35a263b5408b085899b5a4fab6be5d75",
  "fb_datetime": "2023-11-24T12:33:49.162000",
  "fb_entity_version": "8b98f7dd22a477db37f3d0741be8239d",
  "entity_type": "PERSON",
  "entity_subtype": "INDIVIDUAL",
  "updated_at": "2023-11-24T12:33:49.162000",
  "created_at": "2023-11-24T12:33:49.162000",
  "external_ids": {
    "fb_german_business_registry_entity_id": "35a263b5408b085899b5a4fab6be5d75"
  },
  "name": {
    "given": "Oliver",
    "family": "Bäte",
    "maiden": null,
    "aliases": []
  },
  "locations": {
    "home": {
      "fb_entity_id": "6823b26e115adfa60fe49caea0485615",
      "fb_datetime": "2023-11-09T16:14:38.553000",
      "fb_entity_version": "f05bf4207b19eaa9e8d8c9b6f2afd387",
      "entity_type": "LOCATION",
      "entity_subtype": "CITY_NO_POSTAL_CODE",
      "updated_at": "2023-11-09T16:14:38.553000",
      "created_at": "2023-11-09T16:14:38.553000",
      "external_ids": {
        "nominatim": {
          "osm_id": 62578,
          "osm_type": "relation",
          "class": "boundary"
        }
      },
      "coordinate": {
        "latitude": 50.938361,
        "longitude": 6.959974
      },
      "location_level": null,
      "address_components": [
        {
          "component_type": "city",
          "component_value": "Cologne"
        },
        {
          "component_type": "state",
          "component_value": "North Rhine-Westphalia"
        },
        {
          "component_type": "country",
          "component_value": "Germany"
        }
      ],
      "elevation": null,
      "alternative_names": [],
      "fb_semantic_id": "location|city_no_postal_code:germany:north_rhine-westphalia:cologne",
      "formatted_address": "Cologne, Germany"
    }
  },
  "birth_date": {
    "value": "1965-03-01T00:00:00",
    "is_month": false
  },
  "source": {
    "id": "data_sources/1051122944"
  },
  "fb_semantic_id": "person|individual|name:given:oliver|name:family:bäte|birth_date:value:1965-03-01t00:00:00|birth_date:is_month:false|locations:home:location|city_no_postal_code:germany:north_rhine-westphalia:cologne|source:data_sources/1051122944"
}
```

#### Error Handling

The API uses standard HTTP response codes. Errors are indicated with 4xx (client errors) and 5xx (server errors), each accompanied by a message detailing the issue.

#### Notes

* Keep the API key confidential.
