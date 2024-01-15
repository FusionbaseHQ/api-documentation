---
description: AI-Enhanced, Comprehensive Data Discovery
---

# (All in One) Fusion Search

The Fusion Search Endpoint is an AI-enhanced API endpoint offering comprehensive searches across all Fusionbase Data Hub entities. It adeptly interprets user intent to deliver relevant, ranked results and, where applicable, knowledge graphs. Ideal for developers requiring an overarching, context-rich data overview.

{% swagger method="get" path="/search/fusion?q={QUERY}" baseUrl="https://api.fusionbase.com/api/v2" summary="Fusion Search" expanded="true" %}
{% swagger-description %}
Search accross all Fusionbase data entities with a single query.
{% endswagger-description %}

{% swagger-parameter in="query" name="q" type="String" %}
This parameter accepts the user's search string, defining the specific data or information to be retrieved by the Fusion Search Endpoint.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
The Fusion Search API response primarily consists of a `results` section, categorizing matched entities like organizations, persons, and streams with detailed information. When applicable, a `knowledge_graph` is included, offering direct answers to the query, and a `ranks` list details the relevance order of entity types.

````json
```json
{
    "knowledge_graph": {
        "intent": "RELATION",
        "from_entity_type": "ORGANIZATION",
        "from_entity_id": "546bb06ee922e3b09dc50c50068044a3",
        "relation_id": "67533702",
        "relation_parameters": {}
    },
    "results": {
        "persons": [],
        "organizations": [
            {
                "entity_type": "ORGANIZATION",
                "entity": {
                    "fb_entity_id": "546bb06ee922e3b09dc50c50068044a3",
                    "status": "ACTIVE",
                    "display_address": "Aidenbachstraße 140, 81479 Munich, Germany",
                    "registration_authority_entity_name": "München HRB 137024",
                    "display_name": "Health Care Insurance Versicherungsservice GmbH, Zweigniederlassung München",
                    "old_display_name_matched": null,
                    "name": "Health Care Insurance Versicherungsservice GmbH, Zweigniederlassung München"
                },
                "score": 10.070869
            },
            {
                "entity_type": "ORGANIZATION",
                "entity": {
                    "fb_entity_id": "b868b2702752d55edd2c25115724f621",
                    "status": "ACTIVE",
                    "display_address": "Schockenriedstraße 17, 70565 Stuttgart, Germany",
                    "registration_authority_entity_name": "Stuttgart HRB 23323",
                    "display_name": "Health Insurance Service AG",
                    "old_display_name_matched": null,
                    "name": "Health Insurance Service AG"
                },
                "score": 26.590382
            },
            {
                "entity_type": "ORGANIZATION",
                "entity": {
                    "fb_entity_id": "5e1c322419666b33fd0d89442f46766c",
                    "status": "ACTIVE",
                    "display_address": null,
                    "registration_authority_entity_name": "07391552",
                    "display_name": "Health Insurance Services (Sw) Limited",
                    "old_display_name_matched": null,
                    "name": "Health Insurance Services (Sw) Limited"
                },
                "score": 24.026272
            },
            {
                "entity_type": "ORGANIZATION",
                "entity": {
                    "fb_entity_id": "3228a8cd0c6d720672d4e97b837a73cc",
                    "status": "ACTIVE",
                    "display_address": null,
                    "registration_authority_entity_name": "05191059",
                    "display_name": "Health Insurance Matters Ltd.",
                    "old_display_name_matched": null,
                    "name": "Health Insurance Matters Ltd."
                },
                "score": 26.590382
            },
            {
                "entity_type": "ORGANIZATION",
                "entity": {
                    "fb_entity_id": "9419edbe466b488ed13b6e568e83e032",
                    "status": "ACTIVE",
                    "display_address": null,
                    "registration_authority_entity_name": "07350776",
                    "display_name": "The Health Insurance Bureau Limited",
                    "old_display_name_matched": null,
                    "name": "The Health Insurance Bureau Limited"
                },
                "score": 24.026272
            }
        ],
        "locations": [],
        "relations": [
            {
                "entity": {
                    "id": "64395606",
                    "key": "64395606",
                    "name": {
                        "en": "Multiscale Global and Local Statistical Indicators for Locations",
                        "de": "Standortbezogene Globale und Lokale Statistische Indikatoren"
                    },
                    "description": {
                        "en": "Comprehensive statistical indicators across various levels of granularity worldwide, encompassing a wide range of demographic, economic, and social data for countries, regions, and municipalities.",
                        "de": "Umfassende statistische Indikatoren auf verschiedenen Granularitätsstufen weltweit, die eine breite Palette an demographischen, wirtschaftlichen und sozialen Daten für Länder, Regionen und Gemeinden abdecken."
                    },
                    "parameter": {
                        "name": {
                            "de": "Deutschland: Gesamtzahl der Krankenhäuser nach Region",
                            "en": "Germany: Total Number of Hospitals by Region"
                        },
                        "value": {
                            "indicator_name": "TOTAL_HOSPITALS"
                        },
                        "source": {
                            "name": "Bundesinstitut für Bau-, Stadt- und Raumforschung (BBSR)"
                        }
                    },
                    "meta": {
                        "coverage": {
                            "geographical": {
                                "iso_alpha_3": [
                                    "DEU"
                                ]
                            }
                        },
                        "granularity": {
                            "geographical": [
                                2,
                                6
                            ]
                        }
                    }
                },
                "score": 0.6493748426437378,
                "distance": 0.3506251573562622,
                "entity_type": "RELATION"
            },
            {
                "entity": {
                    "id": "64395606",
                    "key": "64395606",
                    "name": {
                        "en": "Multiscale Global and Local Statistical Indicators for Locations",
                        "de": "Standortbezogene Globale und Lokale Statistische Indikatoren"
                    },
                    "description": {
                        "en": "Comprehensive statistical indicators across various levels of granularity worldwide, encompassing a wide range of demographic, economic, and social data for countries, regions, and municipalities.",
                        "de": "Umfassende statistische Indikatoren auf verschiedenen Granularitätsstufen weltweit, die eine breite Palette an demographischen, wirtschaftlichen und sozialen Daten für Länder, Regionen und Gemeinden abdecken."
                    },
                    "parameter": {
                        "name": {
                            "en": "Global: Global Jobs Indicators Database (JOIN)"
                        },
                        "value": {
                            "indicator_name": "GLOBAL_JOBS_INDICATORS_DATABASE__JOIN"
                        },
                        "source": {
                            "name": "World Bank"
                        }
                    },
                    "meta": {
                        "coverage": {
                            "geographical": {
                                "iso_alpha_3": [
                                    "TUR",
                                    "BWA",
                                    "MHL",
                                    "ETH",
                                    "COM",
                                    "DEU",
                                    "SDN",
                                    "BGR",
                                    "AUS",
                                    "NIC",
                                    "MWI",
                                    "PAK",
                                    "BTN",
                                    "TJK",
                                    "SLV",
                                    "CRI",
                                    "VNM",
                                    "GNB",
                                    "TGO",
                                    "EGY",
                                    "LSO",
                                    "BLR",
                                    "TCD",
                                    "BDI",
                                    "CZE",
                                    "DOM",
                                    "KIR",
                                    "CIV",
                                    "DNK",
                                    "MOZ",
                                    "AFG",
                                    "NAM",
                                    "POL",
                                    "IRQ",
                                    "RWA",
                                    "CHL",
                                    "ARM",
                                    "PRI",
                                    "HTI",
                                    "SSD",
                                    "ZWE",
                                    "ALB",
                                    "PER",
                                    "ITA",
                                    "NLD",
                                    "FRA",
                                    "UGA",
                                    "KEN",
                                    "MLI",
                                    "MNG",
                                    "JOR",
                                    "TZA",
                                    "CPV",
                                    "MYS",
                                    "GHA",
                                    "GBR",
                                    "LBR",
                                    "BIH",
                                    "STP",
                                    "AUT",
                                    "SEN",
                                    "IDN",
                                    "ECU",
                                    "USA",
                                    "MDG",
                                    "MNE",
                                    "GTM",
                                    "SVK",
                                    "ISL",
                                    "TUV",
                                    "PNG",
                                    "MMR",
                                    "DJI",
                                    "IND",
                                    "PHL",
                                    "NOR",
                                    "YEM",
                                    "VUT",
                                    "URY",
                                    "JAM",
                                    "MAR",
                                    "ROU",
                                    "BEL",
                                    "ARG",
                                    "GUY",
                                    "ESP",
                                    "LKA",
                                    "NPL",
                                    "SOM",
                                    "MDV",
                                    "SYC",
                                    "LAO",
                                    "VEN",
                                    "BHS",
                                    "PRT",
                                    "MUS",
                                    "MEX",
                                    "IRN",
                                    "TUN",
                                    "HRV",
                                    "BEN",
                                    "CAF",
                                    "RUS",
                                    "TTO",
                                    "KOR",
                                    "PLW",
                                    "HUN",
                                    "MLT",
                                    "ZMB",
                                    "CHN",
                                    "GAB",
                                    "IRL",
                                    "BLZ",
                                    "CAN",
                                    "COG",
                                    "SRB",
                                    "FSM",
                                    "UZB",
                                    "BFA",
                                    "CYP",
                                    "LUX",
                                    "MKD",
                                    "GIN",
                                    "NGA",
                                    "BRB",
                                    "ZAF",
                                    "AZE",
                                    "MDA",
                                    "LTU",
                                    "CHE",
                                    "COL",
                                    "PAN",
                                    "FIN",
                                    "KHM",
                                    "NER",
                                    "UKR",
                                    "BRA",
                                    "BOL",
                                    "GMB",
                                    "SLE",
                                    "KGZ",
                                    "SWZ",
                                    "AGO",
                                    "GRC",
                                    "SLB",
                                    "LVA",
                                    "PRY",
                                    "KAZ",
                                    "MRT",
                                    "EST",
                                    "CMR",
                                    "COD",
                                    "SVN",
                                    "SWE",
                                    "TLS",
                                    "THA",
                                    "FJI",
                                    "GEO",
                                    "BGD",
                                    "TON",
                                    "HND"
                                ]
                            }
                        },
                        "granularity": {
                            "geographical": [
                                1
                            ]
                        }
                    }
                },
                "score": 0.6404404044151306,
                "distance": 0.3595595955848694,
                "entity_type": "RELATION"
            }
        ],
        "streams": [
            {
                "linked_data_id": "",
                "linked_context_id": "55012638",
                "linked_data_value": "USA: ACS1 - Imputation of Direct-purchase Health Insurance",
                "entity": {
                    "key": "55012638",
                    "name": {
                        "en": "USA: ACS1 - Imputation of Direct-purchase Health Insurance"
                    },
                    "display_name": null,
                    "source": {
                        "name": "United States Census Bureau"
                    },
                    "meta": {
                        "coverage": {
                            "geographical": {
                                "iso_alpha_3": [
                                    "USA"
                                ]
                            }
                        }
                    }
                },
                "score": 0.7111876904964447,
                "distance": 0.2888123095035553,
                "entity_type": "STREAM"
            },
            {
                "linked_data_id": "",
                "linked_context_id": "55000993",
                "linked_data_value": "USA: ACS1 - Imputation of Health Insurance Coverage",
                "entity": {
                    "key": "55000993",
                    "name": {
                        "en": "USA: ACS1 - Imputation of Health Insurance Coverage"
                    },
                    "display_name": null,
                    "source": {
                        "name": "United States Census Bureau"
                    },
                    "meta": {
                        "coverage": {
                            "geographical": {
                                "iso_alpha_3": [
                                    "USA"
                                ]
                            }
                        }
                    }
                },
                "score": 0.7104124128818512,
                "distance": 0.2895875871181488,
                "entity_type": "STREAM"
            }
        ],
        "services": []
    },
    "ranks": [
        "STREAM",
        "RELATION",
        "ORGANIZATION"
    ]
}
```
````


{% endswagger-response %}
{% endswagger %}

To help you integrate the Fusion Search API into your application, we provide examples in various programming languages. Each example demonstrates how to make a properly encoded GET request to the API. Ensure you replace `<QUERY>` with your URL-encoded search query and `YOUR_API_KEY` with your actual API key.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X "GET" "https://api.fusionbase.com/api/v2/search/fusion?q=<QUERY>" \
     -H 'X-API-KEY: YOUR_API_KEY' \
     -H 'Content-Type: application/json; charset=utf-8'
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import urllib.parse

query = urllib.parse.quote("<QUERY>")
url = f"https://api.fusionbase.com/api/v2/search/fusion?q={query}"
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
const url = `https://api.fusionbase.com/api/v2/search/fusion?q=${query}`;
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
```java
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;

public class FusionbaseAPIExample {
    public static void main(String[] args) {
        try {
            String query = URLEncoder.encode("<QUERY>", StandardCharsets.UTF_8.toString());
            URL url = new URL("https://api.fusionbase.com/api/v2/search/fusion?q=" + query);
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
    baseURL := "https://api.fusionbase.com/api/v2/search/fusion?q="
    query := url.QueryEscape("<QUERY>")
    requestURL := baseURL + query

    client := &http.Client{}
    req, _ := http.NewRequest("GET", requestURL, nil)
    req.Header.Add("X-API-KEY", "YOUR_API_KEY")
    req.Header.Add("Content-Type", "application/json; charset=utf-8")

    resp, err := client.Do(req)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()
    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```
{% endtab %}
{% endtabs %}

Each code snippet is an example of how to call the Fusion Search API in a specific programming language, ensuring the query is correctly URL-encoded to maintain the integrity of the request.
