---
description: >-
  Event entities encompass comprehensive data gathered from various global
  sources, detailing significant occurrences. These include corporate
  milestones, publications, and key events of businesses.
---

# Event

The `/api/v2/entities/event/get/{FB_ENTITY_ID}` endpoint in the Fusionbase API provides structured information about specific events. This endpoint is crucial for accessing detailed and structured data about events using their unique Fusionbase Entity ID (FB\_ENTITY\_ID).

### Request Headers

* **X-API-KEY**: Your Fusionbase API key. This key is required for authenticating your access to the API.
* **Content-Type**: application/json. This header specifies the format of the request.

### Request Structure

* Access this endpoint via a GET request to `https://api.fusionbase.com/api/v2/entities/event/get/{FB_ENTITY_ID}`.
* Replace `{FB_ENTITY_ID}` with the actual Fusionbase Entity ID of the event you wish to query.

### Request Samples

{% tabs %}
{% tab title="Python" %}
```python
import requests

url = "https://api.fusionbase.com/api/v2/entities/event/get/FB_ENTITY_ID"
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

const url = "https://api.fusionbase.com/api/v2/entities/event/get/FB_ENTITY_ID";
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
        String url = "https://api.fusionbase.com/api/v2/entities/event/get/FB_ENTITY_ID";
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
    url := "https://api.fusionbase.com/api/v2/entities/event/get/FB_ENTITY_ID"
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
"https://api.fusionbase.com/api/v2/entities/event/get/FB_ENTITY_ID" \
-H "X-API-KEY: <YOUR API KEY>" \
-H "Content-Type: application/json"
```
{% endtab %}
{% endtabs %}

### Response Structure

The response is a JSON object containing various details about the event:

* **fb\_entity\_id**: The unique Fusionbase Entity ID of the event.
* **fb\_entity\_version**: A version identifier for the entity's data.
* **entity\_type**: Type of the entity, here "EVENT".
* **entity\_subtype**: The subtype of the entity, e.g., "PUBLICATION".
* **external\_ids**: External identifiers associated with the event (if available).
* **name**: The name of the event in different languages.
* **description**: Short descriptions of the event in different languages.
* **start\_date**, **live\_date**, **end\_date**, **announce\_date**: Relevant dates associated with the event.
* **status**: The current status of the event, e.g., "FINISHED".
* **category**: The category of the event, e.g., "MEMBER\_EXIT\_POSITION".
* **source**: Identifier of the data source from which the information is derived.
* **details**: Detailed information about the event, including roles and linked entities like persons and organizations.
* **origin\_location**, **event\_location**, **effect\_location**: Locations related to the event (if applicable).

### Response Sample

```json
{
    "fb_entity_id": "9befc075e19843dba4ed7dcfc3b70dc5",
    "fb_entity_version": "7fe58d301af8541a1eaee7adc0861120",
    "entity_type": "EVENT",
    "entity_subtype": "PUBLICATION",
    "external_ids": null,
    "name": {
        "en": "Exit of position",
        "de": "Austritt einer Position"
    },
    "description": {
        "short": {
            "en": "A member has exited their position",
            "de": "Ein Mitglied hat seine Position verlassen"
        }
    },
    "start_date": "2013-11-26T00:00:00",
    "live_date": null,
    "end_date": null,
    "announce_date": "2013-11-26T00:00:00",
    "status": "FINISHED",
    "category": "MEMBER_EXIT_POSITION",
    "source": {
        "id": "data_sources/1784627846"
    },
    "details": {
        "value": {
            "role": {
                "name": "Director",
                "original_name_source": "director",
                "responsibilities": null,
                "representation_scheme": null,
                "liability_deposit": null
            }
        },
        "linked_entities": {
            "person": {
                "fb_entity_id": "72a0dbb560e4219e76763d28d6eeadb6",
                "name": {
                    "given": "Nicholas John",
                    "family": "Dell",
                    "maiden": null,
                    "aliases": []
                }
            },
            "organization": {
                "fb_entity_id": "745973f63bb8ce5c36bd16361fc7da3a",
                "name": "The Bristol Law Society"
            }
        }
    },
    "origin_location": null,
    "event_location": null,
    "effect_location": null
}
```
