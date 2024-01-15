---
description: Efficient, Targeted Data and Service Discovery
---

# Streams & Services

The Streams & Services Search Endpoint is a specialized API endpoint designed to streamline searches specifically for Data Streams and Data Services within the Fusionbase Data Hub. It efficiently narrows down results to these two categories, delivering precise data sets and related services. Perfect for developers who need to target their search for datasets, like statistics or environmental data, and services that complement data processing and analytics, providing a focused and detailed data selection.



{% swagger method="get" path="/data?q=<QUERY>" baseUrl="https://app.fusionbase.com/api/v2/search" summary="Data Search" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="q" type="String" %}
This parameter accepts the user's search string, defining the specific data or information to be retrieved by the Data Search Endpoint.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Search response" %}
```json
{
    "results": [
        {
            "linked_data_id": "",
            "linked_context_id": "4994292",
            "linked_data_value": "Geodata Lookups for Germany",
            "entity": {
                "key": "4994292",
                "name": {
                    "en": "Geodata Lookups for Germany"
                },
                "display_name": null,
                "source": {
                    "name": "Fusionbase"
                },
                "meta": {
                    "coverage": {
                        "geographical": {
                            "iso_alpha_3": [
                                "DEU"
                            ]
                        }
                    }
                }
            },
            "score": 0.7391094863414764,
            "distance": 0.26089051365852356,
            "entity_type": "STREAM"
        },
        {
            "linked_data_id": "",
            "linked_context_id": "3390386",
            "linked_data_value": "IPv4 Geolocation Mappings for Germany - Timeseries",
            "entity": {
                "key": "3390386",
                "name": {
                    "en": "IPv4 Geolocation Mappings for Germany - Timeseries"
                },
                "display_name": null,
                "source": {
                    "name": "Fusionbase"
                },
                "meta": {
                    "coverage": {
                        "geographical": {
                            "iso_alpha_3": [
                                "DEU"
                            ]
                        }
                    }
                }
            },
            "score": 0.6845411658287048,
            "distance": 0.31545883417129517,
            "entity_type": "STREAM"
        }
    }
]

```
{% endswagger-response %}
{% endswagger %}

To assist with integrating the Streams & Services Search API into your application, we provide examples in various programming languages. Each example illustrates how to execute a properly encoded GET request to the API. Remember to replace `<QUERY>` with your URL-encoded search query and `YOUR_API_KEY` with the actual API key provided to you.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X "GET" "https://api.fusionbase.com/api/v2/search/data?q=$(urlencode "<QUERY>")" \
     -H 'X-API-KEY: YOUR_API_KEY' \
     -H 'Content-Type: application/json; charset=utf-8'
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import urllib.parse

query = urllib.parse.quote("<QUERY>")
url = f"https://api.fusionbase.com/api/v2/search/data?q={query}"
headers = {
    'X-API-KEY': 'YOUR_API_KEY',
    'Content-Type': 'application/json; charset=utf-8'
}

response = requests.get(url, headers=headers)
print(response.json())
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const axios = require('axios');
const querystring = require('querystring');

const query = querystring.escape("<QUERY>");
const url = `https://api.fusionbase.com/api/v2/search/data?q=${query}`;
const headers = {
    'X-API-KEY': 'YOUR_API_KEY',
    'Content-Type': 'application/json; charset=utf-8'
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
```javascript
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;

public class StreamsServicesAPIExample {
    public static void main(String[] args) {
        try {
            String query = URLEncoder.encode("<QUERY>", StandardCharsets.UTF_8.toString());
            URL url = new URL("https://api.fusionbase.com/api/v2/search/data?q=" + query);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");
            connection.setRequestProperty("X-API-KEY", "YOUR_API_KEY");
            connection.setRequestProperty("Content-Type", "application/json; charset=utf-8");

            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuffer content = new StringBuffer();
            while ((inputLine = in.readLine()) != null) {
                content.append(inputLine);
            }
            in.close();
            System.out.println(content.toString());
        } catch (Exception e) {
            e.printStackTrace();
        }
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
    "net/url"
    "io/ioutil"
)

func main() {
    // Prepare the base URL and the query parameter
    baseURL := "https://api.fusionbase.com/api/v2/search/data?q="
    queryParam := "<QUERY>" // replace with your actual query
    encodedQuery := url.QueryEscape(queryParam)
    requestURL := baseURL + encodedQuery

    // Create a new HTTP client and request
    client := &http.Client{}
    req, err := http.NewRequest("GET", requestURL, nil)
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }

    // Add headers to the request
    req.Header.Add("X-API-KEY", "YOUR_API_KEY") // replace with your actual API key
    req.Header.Add("Content-Type", "application/json; charset=utf-8")

    // Perform the request
    resp, err := client.Do(req)
    if err != nil {
        fmt.Println("Error making request:", err)
        return
    }
    defer resp.Body.Close
    
    // Read and print the response body
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Println("Error reading response:", err)
        return
    }
    fmt.Println(string(body))
}
```
{% endtab %}
{% endtabs %}

These code snippets provide a template for making a GET request to the Streams & Services Search endpoint of the Fusionbase Data Hub API. By encoding the query parameter, you ensure that the request is correctly formatted and can be processed efficiently by the API.
