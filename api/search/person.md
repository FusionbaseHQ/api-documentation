---
description: >-
  Discover the professional world – delve into detailed profiles and connections
  with Fusionbase's Person Search.
---

# Person

The Person Search Endpoint is a specialized tool designed for retrieving detailed personal profiles from official business registry sources. This powerful endpoint is essential for applications requiring in-depth information about individuals, particularly in business contexts, such as their roles in organizations, location, and professional relationships. It is a valuable resource for developers building applications for business intelligence, network analysis, or compliance purposes.

### Example: Using the Person Search Endpoint

To search for an individual, a GET request is made to the Person Search Endpoint. This endpoint accepts the person's name or other identifiers as query parameters. It returns a JSON object with comprehensive personal data, extracted exclusively from official business registry sources.

**Request Example:**

```bash
https://api.fusionbase.com/api/v2/search/entities/person?q=Kevin%20Goßling
```

It is important to URL encode the query parameter to ensure proper request formatting.

### Expected Response:

The API response is a JSON object with an array under the `results` key, where each entry represents a person entity. The response includes detailed personal information such as the Fusionbase entity ID, name, location, organizational affiliations, and more.

Here’s a simplified example based on the provided response:

```json
{
    "results": [
        {
            "entity_type": "PERSON",
            "entity": {
                "fb_entity_id": "8e03af05947fdff52a182084b5ce0a24",
                "entity_type": "PERSON",
                "entity_subtype": "INDIVIDUAL",
                "name": {
                    "given": "Kevin",
                    "family": "Goßling"
                },
                "locations": {
                    "home": {
                        "formatted_address": "Munich, Germany"
                    }
                },
                "relations": {
                    "ORGANIZATION": [
                        {
                            "label": "MANAGING_DIRECTOR",
                            "entity_to": {
                                "fb_entity_id": "ff525267e6ff5f67a6dbe0af29b7e5cc",
                                "display_name": "Fusionbase GmbH"
                            },
                            "meta": {
                                "start_date": "2019-10-24"
                            }
                        }
                    ]
                },
                "source_id": "data_sources/1051122944"
            },
            "score": 19.639364
        }
    ],
    "total": 1
}

```

#### Breakdown of Response Structure:

* **results**: An array of person entities matching the search query.
* **entity\_type**: Specifies the type of the entity, here "PERSON".
* **entity**: Contains the detailed information about the person.
  * **fb\_entity\_id**: A unique identifier for the person within the Fusionbase Data Hub.
  * **name**: The individual’s name, including given and family names.
  * **locations**: Information about the person's location, such as home address.
  * **relations**: Details of the individual’s affiliations or roles in organizations, including position labels and related organization details.
  * **source\_id**: The source identifier from which the data is derived.
* **score**: A relevance score for the search result.

The Person Search Endpoint is a powerful tool for developers to access rich, verified personal data. The unique `fb_entity_id` enables developers to reference specific individuals consistently within the Fusionbase ecosystem. This capability is crucial for applications that require tracking of individual profiles over time, understanding their business network, or conducting due diligence. The endpoint's comprehensive data set provides a solid foundation for sophisticated applications requiring reliable and detailed personal information.
