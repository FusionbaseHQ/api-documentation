# Data Versions

The Fusionbase API offers a sophisticated mechanism for retrieving entries based on their version. This is particularly useful for accessing data that is newer than a specific version or within a range of versions. The `version_boundary` parameter in the API request facilitates this functionality.

Let's assume a Data Stream is updated five times a day and you decide to pull the data just once a day. To get all of the newly added records from your local version up to the most recent version of the Data Stream, simply use the delta endpoint.

{% swagger baseUrl="https://api.fusionbase.com/api/v2/stream/data" method="get" path="/{STREAM_ID}?version_boundary={FB_DATA_VERSION}&format=json" summary="Get records from a certain version" expanded="true" %}
{% swagger-description %}
Get the data of a given Data stream since a specified version
{% endswagger-description %}

{% swagger-parameter in="query" name="format" %}
json or msgpack
{% endswagger-parameter %}

{% swagger-parameter in="path" name="STREAM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="version_boundary" %}
A string or a pair of comma-separated strings representing version IDs.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}
{% endswagger %}

#### Request Structure

To filter data based on version, utilize the `version_boundary` parameter in your GET request to `/api/v2/stream/data/{STREAM_ID}`. This parameter can be set to a single version ID or two version IDs separated by a comma, indicating a range.

#### Version Boundary Properties

* `version_boundary`: A string or a pair of comma-separated strings representing version IDs.
  * Single Version: Retrieves entries newer than the specified version.
  * Two Versions: Retrieves all entries between the two provided versions (inclusive).

#### Examples

1.  **Single Version Filter** To retrieve entries newer than a specific version:

    ```bash
    /api/v2/stream/data/{STREAM_ID}?version_boundary=bd764e07-50a1-4c0e-a102-ae1a259557av&format=json
    ```

    This request returns entries that are newer than the version `bd764e07-50a1-4c0e-a102-ae1a259557av`.
2.  **Range of Versions Filter** To retrieve entries within a specific range of versions:

    ```bash
    /api/v2/stream/data/{STREAM_ID}?version_boundary=version1ID,version2ID&format=json
    ```

    Replace `version1ID` and `version2ID` with the desired start and end versions. This request returns all entries between these two versions.

#### Notes

* Ensure that the version IDs used are valid and correspond to the versions in the data stream.
* The version IDs are case-sensitive and should be used exactly as they appear in the data stream.

#### Error Handling

Errors may occur if the `version_boundary` parameter is not properly structured, if invalid version IDs are used, or if there is a syntax error in the request. In such cases, the API might return an error response.

This documentation provides clear instructions on how to use the `version_boundary` parameter for filtering data by version, with examples for both single version filtering and range filtering.
