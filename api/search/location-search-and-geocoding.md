---
description: 'Pinpoint Precision: Explore and Geocode with Ease'
---

# Location Search and Geocoding

The Location Search Endpoint is your comprehensive solution for place searches and address geocoding, akin to the functionality of Google Maps Places Search. Whether you're plotting points of interest or transforming addresses into coordinated data, this endpoint delivers accurate, map-ready results. It's the developer's tool of choice for creating location-aware applications with high precision and reliability.

### Example: Using the Location Search Endpoint for Geocoding

To geocode an address such as "Agnes-Pockels-Bogen 1, 80992 München", you would make a GET request to the Location Search Endpoint. The endpoint takes the address as a query parameter and returns a JSON object with the geocoded location details, including latitude and longitude coordinates.

**Request Example:**

{% code overflow="wrap" %}
```url
https://api.fusionbase.com/api/v2/search/entities/location?q=Agnes-Pockels-Bogen%201,%2080992%20München
```
{% endcode %}

Make sure to URL encode the query parameter to ensure the request is properly formatted.

**Expected Response:**

The API's response will be a JSON object containing an array under the `results` key. Each entry in this array corresponds to a found location. For the provided address, the response includes the Fusionbase entity ID, creation and update timestamps, the entity version, and external identifiers such as OpenStreetMap data. Most importantly, it contains the `coordinate` object with `latitude` and `longitude` keys, providing the precise geocoded location.

Here's a structured example based on the provided response:

```json
{
    "results": [
        {
            "entity": {
                "fb_entity_id": "45d488830421ebc1d9130cc0856b3c5d",
                "fb_datetime": "2023-12-18T16:17:53.570000",
                "fb_entity_version": "9cefd42e59f49e3de4b0b33f983b740c",
                "entity_type": "LOCATION",
                "updated_at": "2023-12-18T16:17:53.570000",
                "created_at": "2023-11-09T16:17:51.381000",
                "external_ids": {
                    "nominatim": {
                        "osm_id": 1787145076,
                        "osm_type": "node",
                        "class": "amenity"
                    }
                },
                "coordinate": {
                    "latitude": 48.1735835,
                    "longitude": 11.5323446
                },
                "location_level": null,
                "address_components": [
                    {
                        "component_type": "house_number",
                        "component_value": "1"
                    },
                    {
                        "component_type": "street",
                        "component_value": "Agnes-Pockels-Bogen"
                    },
                    {
                        "component_type": "postal_code",
                        "component_value": "80992"
                    },
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
                "fb_semantic_id": "location|locality:germany:bavaria:80992:munich:agnes-pockels-bogen:1",
                "formatted_address": "Agnes-Pockels-Bogen 1, 80992 Munich, Germany"
            },
            "entity_type": "LOCATION"
        }
    ]
}
```

The search result from the Fusionbase Location Search Endpoint provides a structured JSON object containing detailed information about the queried location. Here's how the result is organized and how it can be utilized:

1. **`results`:** This is an array that contains one or more location entities that match the search query. In the provided example, there is one matching location entity for the address "Agnes-Pockels-Bogen 1, 80992 München".
2. **`entity`:** Within each item in the `results` array, the `entity` object holds the data for thelocation. This includes unique identifiers, timestamps, and structured location information.
3. **`fb_entity_id`:** This is a crucial element as it uniquely identifies the location within the Fusionbase Data Hub. It's used to reference this specific location in subsequent API calls or to cross-reference within the Fusionbase ecosystem.
4. **`coordinate`:** The `latitude` and `longitude` values provide the geocoded coordinates of the address, which can be used for placing markers on a map, calculating distances, or other geospatial operations.
5. **`address_components`:** This array breaks down the address into individual components such as `house_number`, `street`, `postal_code`, `city`, `state`, and `country`. This granular information is useful for applications that need to process or display parts of an address separately.
6. **`formatted_address`:** A human-readable address string that combines all address components, suitable for display purposes.

The `fb_entity_id` is a key identifier that developers can utilize to maintain a durable reference to a specific location. It allows for the retrieval of extended details and facilitates the execution of further operations, such as resolving relations for the given location within the Fusionbase Data Hub. By leveraging this ID, developers can access related statistical indicators like household income, demographic distributions, or other socio-economic data points associated with that location, thus providing a richer and more detailed data narrative for applications and analyses.

