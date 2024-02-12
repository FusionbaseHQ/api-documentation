---
description: >-
  Navigate the corporate landscape – uncover comprehensive organizational data
  with Fusionbase's Organization Search.
---

# Organization

The Organization Search Endpoint is a dynamic solution for searching organizational data and extracting specific business information. This endpoint is ideal for applications requiring detailed information about various organizations. It provides robust data for developers building applications that require comprehensive business information, such as company names, statuses, addresses, and registration details.

### Example: Using the Organization Search Endpoint

To search for an organization, a GET request is made to the Organization Search Endpoint. The endpoint accepts the organization's name or other identifying details as query parameters. In response, it returns a JSON object containing detailed information about the organization, including its status, address, registration details, and more.

**Request Example:**

```bash
https://api.fusionbase.com/api/v2/search/entities/organization?q=OroraTech%20GmbH
```

Remember to URL encode the query parameter for proper request formatting.

### Expected Response:

The API returns a JSON object containing an array under the `results` key, each entry corresponding to a found organization. For a given query, the response includes details like the Fusionbase entity ID, organization status, address, and registration authority.

Here’s a simplified example based on the provided response:

```json
{
    "results": [
        {
            "entity_type": "ORGANIZATION",
            "entity": {
                "fb_entity_id": "82a68ab9f7151fa0af9bf189c1caa753",
                "status": "ACTIVE",
                "display_address": "Sankt-Martin-Straße 112, 81669 Munich, Germany",
                "registration_authority_entity_name": "München HRB 243843",
                "display_name": "OroraTech GmbH",
                "name": "OroraTech GmbH",
                "source_key": "1051122944"
            },
            "score": 40.53212
        },
        {
            "entity_type": "ORGANIZATION",
            "entity": {
                "fb_entity_id": "64ab4b0ada714a56939c70f5e8a62e8a",
                "status": "ACTIVE",
                "display_address": "Birkenweg, 15834 Rangsdorf, Germany",
                "registration_authority_entity_name": "Potsdam HRB 15953",
                "display_name": "FloraTech GmbH",
                "name": "FloraTech GmbH",
                "source_key": "1051122944"
            },
            "score": 14.997058
        }
    ],
    "total": 10
}

```

#### Breakdown of Response Structure:

* **results**: Contains an array of organization entities matching the search query.
* **entity\_type**: Indicates the type of the entity, here "ORGANIZATION".
* **entity**: Contains detailed information about the organization.
  * **fb\_entity\_id**: A unique identifier for the organization within Fusionbase Data Hub.
  * **status**: Indicates the current status of the organization (e.g., Active, Inactive).
  * **display\_address**: The public address of the organization.
  * **registration\_authority\_entity\_name**: The name of the authority under which the organization is registered.
  * **display\_name**: The public name of the organization.
  * **name**: Official registered name of the organization.
  * **source\_key**: A unique key from the data source.
* **score**: A relevance score for the search result.

The `fb_entity_id` is vital for developers as it allows for consistent referencing of a specific organization within the Fusionbase ecosystem. This unique ID enables the retrieval of more detailed information and supports various operations, such as tracking changes in the organization's status or retrieving related financial or operational data. By leveraging this ID, developers can access a wealth of information to enrich their applications and analyses with in-depth organizational data.
