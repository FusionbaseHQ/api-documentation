---
description: >-
  Organization entities represent legally established business or corporate
  entities. Each organization entity typically corresponds to a unique company,
  nonprofit, government body, or any other type.
---

# Organizations

The `/api/v2/entities/organization/get/{FB_ENTITY_ID}` endpoint in the Fusionbase API provides structured information about specific organizations. This endpoint is essential for retrieving detailed and structured data about companies using their unique Fusionbase Entity ID (FB\_ENTITY\_ID).

### Request Header

* **X-API-KEY**: Your Fusionbase API key. This key authenticates your access to the API.
* **Content-Type**: application/json. This header specifies the format of the request body.

### Request Structure

* The endpoint is accessed via a GET request to `https://api.fusionbase.com/api/v2/entities/organization/get/{FB_ENTITY_ID}`.
* `{FB_ENTITY_ID}` should be replaced with the actual Fusionbase Entity ID of the organization you wish to query.

### Request Sample

{% tabs %}
{% tab title="Python " %}
```python
import requests

url = "https://api.fusionbase.com/api/v2/entities/organization/get/FB_ENTITY_ID"
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

const url = "https://api.fusionbase.com/api/v2/entities/organization/get/FB_ENTITY_ID";
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
        String url = "https://api.fusionbase.com/api/v2/entities/organization/get/FB_ENTITY_ID";
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
    url := "https://api.fusionbase.com/api/v2/entities/organization/get/FB_ENTITY_ID"
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
"https://api.fusionbase.com/api/v2/entities/organization/get/FB_ENTITY_ID" \
-H "X-API-KEY: <YOUR API KEY> " \
-H "Content-Type: application/json"
```
{% endtab %}
{% endtabs %}

### Response Structure

Due to the extensive nature of the response, only key elements are highlighted here:

* **fb\_entity\_id**: Unique identifier for the organization within Fusionbase.
* **fb\_datetime**: Timestamp of the data retrieval or update.
* **fb\_entity\_version**: Version identifier of the entity's data.
* **entity\_type**: Type of the entity, e.g., "ORGANIZATION".
* **entity\_subtype**: Subtype of the entity, e.g., "CORPORATION".
* **updated\_at**, **created\_at**: Timestamps for the last update and creation of the entity record.
* **name**: Official name of the organization.
* **description**: Detailed description of the organization, often including the business scope.
* **address**: Comprehensive address information, including geographical coordinates.
* **jurisdiction**: Jurisdiction information represented by ISO country codes.
* **contact**: Contact information like websites and phone numbers.
* **founding\_date**: The date when the organization was founded.
* **classifications**: Various classifications of the organization, including industry codes and legal forms.
* **legal**: Legal registration details of the organization.

### Response Sample

```json
{
  "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
  "fb_datetime": "2024-02-01T16:12:18.849000",
  "fb_entity_version": "55afcba0d74181bd508e0207e3fbddfc",
  "entity_type": "ORGANIZATION",
  "entity_subtype": "CORPORATION",
  "updated_at": "2024-02-01T16:12:18.849000",
  "created_at": "2023-11-24T10:30:02.502000",
  "external_ids": {
    "fb_german_business_registry_entity_id": "75e8887e25587ed64cc8e1733a6c7160"
  },
  "status": {
    "active": true,
    "status": "ACTIVE",
    "status_detail": null
  },
  "name": "ADOS GmbH",
  "other_names": [
    "Ados Gesellschaft mit beschränkter Haftung"
  ],
  "description": {
    "registry": {
      "de": "Gegenstand des Unternehmens ist die Herstellung und der Verkauf der unter dem Namen \"ADOS\" geschützten Artikel sowie der Erwerb, die Veräußerung, die Verwertung von Patenten und Erfinderrechten und die Herstellung, Anschaffung und der Vertrieb von anderen Gegenständen des Gewerbefleisses."
    }
  },
  "address": {
    "fb_entity_id": "b69ea5ed95c054a3ad9b3d75335386a7",
    "fb_datetime": "2023-11-23T16:00:34.569000",
    "fb_entity_version": "036c16c81f808da80a4099ca1ad85d22",
    "updated_at": "2023-11-23T16:00:34.569000",
    "created_at": "2023-11-23T16:00:34.569000",
    "external_ids": {
      "nominatim": {
        "osm_id": 163641824,
        "osm_type": "way",
        "class": "building"
      }
    },
    "coordinate": {
      "latitude": 50.7697654,
      "longitude": 6.1196758544025
    },
    "location_level": null,
    "address_components": [
      {
        "component_type": "house_number",
        "component_value": "23-25"
      },
      {
        "component_type": "street",
        "component_value": "Trierer Straße"
      },
      {
        "component_type": "postal_code",
        "component_value": "52078"
      },
      {
        "component_type": "city",
        "component_value": "Aachen"
      },
      {
        "component_type": "county",
        "component_value": "Städteregion Aachen"
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
    "alternative_names": [],
    "fb_semantic_id": "location|locality:germany:north_rhine-westphalia:52078:aachen:trierer_strasse:23-25",
    "formatted_address": "Trierer Straße 23-25, 52078 Aachen, Germany"
  },
  "other_addresses": null,
  "jurisdiction": {
    "iso_alpha3": "DEU"
  },
  "contact": {
    "websites": {
      "primary": "https://www.ados.de/"
    },
    "phone_numbers": {
      "primary": "0241 97690"
    }
  },
  "founding_date": "1961-02-20T00:00:00",
  "cessation_date": null,
  "classifications": {
    "web": [
      {
        "source": "Google",
        "value": {
          "de": "Hersteller elektronischer Geräte"
        }
      }
    ],
    "industry_codes": null,
    "legal_form": null
  },
  "source": {
    "id": "data_sources/1051122944"
  },
  "fb_semantic_id": "organization|corporation|registration_authority_id:r3101|registration_authority_name:r3101|registration_type:hrb|registration_number:5|registration_authority_location_id:2077c7f46e95a3740492448c2beb2011|source:data_sources/1051122944",
  "legal": {
    "registration_authority": {
      "local": {
        "registration_authority_id": "R3101",
        "registration_authority_name": "Aachen",
        "registration_authority_entity_id": "R3101 HRB 5",
        "registration_authority_entity_name": "Aachen HRB 5",
        "registration_type": "HRB",
        "registration_number": "5",
        "registration_id_extra": null,
        "registration_authority_location": {
          "fb_entity_id": "2077c7f46e95a3740492448c2beb2011",
          "fb_datetime": "2023-11-17T07:29:40.987000",
          "fb_entity_version": "feddb22b0fa9166a41602a99f63601af",
          "entity_type": "LOCATION",
          "entity_subtype": "CITY_NO_POSTAL_CODE",
          "updated_at": "2023-11-17T07:29:40.987000",
          "created_at": "2023-11-09T16:14:31.050000",
          "external_ids": {
            "nominatim": {
              "osm_id": 62564,
              "osm_type": "relation",
              "class": "boundary"
            }
          },
          "coordinate": {
            "latitude": 50.776351,
            "longitude": 6.083862
          },
          "location_level": null,
          "address_components": [
            {
              "component_type": "city",
              "component_value": "Aachen"
            },
            {
              "component_type": "county",
              "component_value": "Städteregion Aachen"
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
          "fb_semantic_id": "location|city_no_postal_code:germany:north_rhine-westphalia:aachen",
          "formatted_address": "Aachen, Germany"
        }
      }
    }
  }
}
```

#### Error Handling

The API uses standard HTTP response codes. Errors are indicated with 4xx (client errors) and 5xx (server errors), each accompanied by a message detailing the issue.

#### Notes

* Keep the API key confidential.
