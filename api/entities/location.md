---
description: Location entities offer precise and structured geographical information
---

# Location

The `/api/v2/entities/location/get/{FB_ENTITY_ID}` endpoint in the Fusionbase API provides structured information about specific locations. This endpoint is crucial for accessing detailed and structured data about locations using their unique Fusionbase Entity ID (FB\_ENTITY\_ID).

### Request Header

* **X-API-KEY**: Your Fusionbase API key. This key authenticates your access to the API.
* **Content-Type**: application/json. This header specifies the format of the request.

### Request Structure

* Access this endpoint via a GET request to `https://api.fusionbase.com/api/v2/entities/location/get/{FB_ENTITY_ID}`.
* Replace `{FB_ENTITY_ID}` with the actual Fusionbase Entity ID of the location you wish to query.

### Request Samples

{% tabs %}
{% tab title="Python" %}
```python
import requests

url = "https://api.fusionbase.com/api/v2/entities/location/get/FB_ENTITY_ID"
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

const url = "https://api.fusionbase.com/api/v2/entities/location/get/FB_ENTITY_ID";
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
        String url = "https://api.fusionbase.com/api/v2/entities/location/get/FB_ENTITY_ID";
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
    url := "https://api.fusionbase.com/api/v2/entities/location/get/FB_ENTITY_ID"
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
{% endtabs %}

### Response Structure

The response is a JSON object containing various details about the location:

* **fb\_entity\_id**: The unique Fusionbase Entity ID of the location.
* **fb\_datetime**: Timestamp indicating the last update or retrieval of data.
* **fb\_entity\_version**: A version identifier for the entity's data.
* **entity\_type**: Type of the entity, here "LOCATION".
* **updated\_at**, **created\_at**: Timestamps of the last update and creation of the entity record.
* **external\_ids**: External identifiers associated with the location.
* **coordinate**: Geographical coordinates of the location (latitude and longitude).
* **location\_level**: The hierarchical level of the location (if available).
* **address\_components**: Detailed address components like city, state, and country.
* **alternative\_names**: Alternative names for the location (if any).
* **fb\_semantic\_id**: A semantic identifier providing a structured representation of the location's key attributes.
* **formatted\_address**: A human-readable address format.

### Response Sample

```json
{
  "fb_entity_id": "bfcc19ddd9edb12efb9cfea181b0dcd3",
  "fb_datetime": "2024-01-24T10:24:58.231000",
  "fb_entity_version": "cbff4cb34ac7130802e07d0679aa417b",
  "entity_type": "LOCATION",
  "updated_at": "2024-01-24T10:24:58.231000",
  "created_at": "2023-11-09T16:14:38.082000",
  "external_ids": {
    "nominatim": {
      "osm_id": 62428,
      "osm_type": "relation",
      "class": "boundary"
    }
  },
  "coordinate": {
    "latitude": 48.1371079,
    "longitude": 11.5753822
  },
  "location_level": null,
  "address_components": [
    {
      "component_type": "city",
      "component_value": "Munich"
    },
    {
      "component_type": "state",
      "component_value": "Bavaria"
    },
    {
      "component_type": "country",
      "component_value": "Germany"
    }
  ],
  "alternative_names": [],
  "fb_semantic_id": "location|city_no_postal_code:germany:bavaria:munich",
  "formatted_address": "Munich, Germany"
}
```

#### Error Handling

The API uses standard HTTP response codes. Errors are indicated with 4xx (client errors) and 5xx (server errors), each accompanied by a message detailing the issue.

#### Notes

* Keep the API key confidential.
