# Relations

The Relations Endpoint is a powerful feature of Fusionbase API, designed to resolve and map the complex web of relationships between various entities and data features. This endpoint is particularly useful for understanding the network of connections surrounding an entity, be it a person or an organization. It is an invaluable tool for applications focusing on network analysis, relationship mapping, and comprehensive data linkage.

### &#x20;Example: Using the Relations Endpoint

To resolve relations for a specific entity, a POST request is made to the Relations Endpoint with the relation ID and entity ID. This endpoint efficiently maps the network of connections related to the given entity, providing insights into their professional and organizational links.

**Request Example:**

```bash
POST https://api.fusionbase.com/api/v2/relation/resolve/<RELATION_ID>/<ENTITY_ID>
```

Let's take `3138484719` as the relation ID and `75e8887e25587ed64cc8e1733a6c7160` as the entity ID to execute a sample request.

The relation with ID is the "Network" relation, showing all connected other entities,

### Expected Response:

The API response is a JSON array, where each object represents a relationship linked to the queried entity. It includes details about the entities involved, the type of relationship, and relevant metadata.

Here’s a simplified example based on the provided response:

```json
[
    {
        "label": "ENTITY_NETWORK",
        "entity_from": {
            "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
            "entity_type": "ORGANIZATION",
            "display_name": "ADOS GmbH"
        },
        "entity": {
            "entity_type": "FEATURE",
            "entity_subtype": "NETWORK",
            "fb_semantic_id": "feature|network|organization:75e8887e25587ed64cc8e1733a6c7160",
            "value": {
                "root": {
                    "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
                    "entity_type": "ORGANIZATION",
                    "display_name": "ADOS GmbH"
                },
                "links": [
                    {
                        "depth": 1,
                        "relation_id": "135ff9ab592bb1b41ab948b3ad95f91f",
                        "label": "MANAGING_DIRECTOR",
                        "entity_from": {
                            "fb_entity_id": "8a00ab6520e0b64eb83345586ba1613e",
                            "entity_type": "PERSON",
                            "display_name": "Herbert Rütgers"
                        },
                        "entity_to": {
                            "fb_entity_id": "75e8887e25587ed64cc8e1733a6c7160",
                            "entity_type": "ORGANIZATION",
                            "display_name": "ADOS GmbH"
                        },
                        "meta": {
                            "start_date": "2002-06-03",
                            "end_date": "2020-07-06"
                        }
                    }
                    // Additional links omitted for brevity
                ]
            }
        }
    }
]

```

#### Breakdown of Response Structure:

* **label**: Describes the type of relationship.
* **entity\_from**: The originating entity of the relation.
* **entity**: Contains the details of the relationship and linked entities.
  * **entity\_type** & **entity\_subtype**: Specify the type and subtype of the entity.
  * **fb\_semantic\_id**: A unique identifier for the relationship feature.
  * **value**: An object containing details about the root entity and its links.
    * **root**: The primary entity from which the network originates.
    * **links**: An array of related entities and their respective relationships.
      * **depth**: Indicates the level of connection relative to the root entity.
      * **relation\_id**: A unique identifier for the specific relationship.
      * **label**: The label describing the nature of the relationship (e.g., MANAGING\_DIRECTOR).
      * **entity\_from** & **entity\_to**: The entities involved in the relationship.
      * **meta**: Metadata about the relationship, such as start and end dates.

The Relations Endpoint offers a detailed and comprehensive view of the networks surrounding an entity, providing crucial insights for businesses and researchers. By mapping out these relationships, the endpoint aids in understanding complex organizational structures, professional networks, and the dynamics of business ecosystems.
