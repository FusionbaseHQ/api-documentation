# Python

The Fusionbase Python SDK provides a powerful and intuitive interface for accessing Fusionbase data, entities, and services.

## Installation

Fusionbase is available on PyPI:

```bash
pip install fusionbase
```

### Optional Dependencies

Install with additional features:

```bash
# For pandas DataFrame support
pip install fusionbase[pandas]

# For all optional features
pip install fusionbase[all]
```

## Getting Started

Visit our [examples repository](https://github.com/FusionbaseHQ/fusionbase-python/tree/main/examples) for comprehensive examples and use cases.

### Basic Setup

```python
from fusionbase import Fusionbase

# Initialize with API key from environment variable (FUSIONBASE_API_KEY)
client = Fusionbase()

# Or provide the API key explicitly
client = Fusionbase(api_key="your_api_key_here")

# Using context manager (recommended)
with Fusionbase() as client:
    # Your code here
    pass
```

### Configuration

```python
from fusionbase import Fusionbase, FusionbaseConfig
from fusionbase.core.config import CacheConfig, RetryConfig

# Custom configuration
config = FusionbaseConfig(
    timeout=30.0,
    cache=CacheConfig(
        enabled=True,
        ttl_seconds=3600,  # 1 hour
    ),
    retry=RetryConfig(
        enabled=True,
        max_attempts=3,
    ),
)

client = Fusionbase(config=config)
```

## Data Streams

Data Streams provide access to structured datasets. Each stream can be accessed via its unique stream ID.

### Accessing a Data Stream

```python
from fusionbase import Fusionbase

with Fusionbase() as client:
    # Get a data stream by ID
    stream = client.streams.from_id("28654971")

    # Or using the convenience method
    stream = client.get_datastream("28654971")
```

### Stream Metadata

```python
# Get metadata about the stream
metadata = stream.get_metadata()

print(f"Name: {metadata.display_name}")
print(f"Total records: {metadata.meta.entry_count}")
print(f"Columns: {metadata.column_names}")
```

### Retrieving Data

```python
# Get data as a list of dictionaries
data = stream.get_data()

# With pagination
data = stream.get_data(skip=0, limit=100)

# Select specific columns
data = stream.get_data(project_fields=["column1", "column2"])

# Get data as a pandas DataFrame
df = stream.get_data(return_type="dataframe")
```

### Filtering Data

```python
from fusionbase.data.datastream import FilterOperator

# Create a filter
filter = stream.create_filter("column_name", FilterOperator.EQUALS, "value")

# Apply filters
data = stream.get_data(filters=[filter], limit=100)
```

### Searching Within a Stream

```python
# Search for data within a stream
results = stream.search_data(q="search term", limit=10)

# With pagination
results = stream.search_data(q="search term", skip=0, limit=20)
```

### Iterating Over Large Datasets

```python
# Process data in chunks
for chunk in stream.iter_chunks(chunk_size=1000):
    for record in chunk:
        # Process each record
        pass

# Iterate as pandas DataFrames
for df_chunk in stream.iter_chunks_pandas(chunk_size=1000):
    # Process each DataFrame chunk
    pass
```

### Exporting Data

```python
from pathlib import Path

# Export to JSON
stream.export_to_file(Path("./data.json"), limit=1000)

# Export to CSV
stream.export_to_file(Path("./data.csv"), file_format="csv")

# Export to JSON Lines
stream.export_to_file(Path("./data.jsonl"), file_format="jsonl")
```

### Async Operations

```python
import asyncio
from fusionbase import Fusionbase

async def main():
    client = Fusionbase()

    try:
        stream = client.streams.from_id("28654971")

        # Async data retrieval
        data = await stream.aget_data(limit=100)

        # Async search
        results = await stream.asearch_data(q="search term")

        # Async metadata
        metadata = await stream.aget_metadata()
    finally:
        await client.aclose()
        client.close()

asyncio.run(main())
```

## Data Services

Data Services are APIs that process input and return structured output, such as address normalization or company enrichment.

### Accessing a Data Service

```python
from fusionbase import Fusionbase

with Fusionbase() as client:
    # Get a service by ID
    service = client.services.from_id("50527318")

    # Or using the convenience method
    service = client.get_dataservice("50527318")
```

### Service Metadata

```python
# Get service metadata
metadata = service.get_metadata()

print(f"Name: {metadata.display_name}")
print(f"Credit cost: {metadata.cost}")

# View required parameters
for param in metadata.service_input_definition:
    if param.required:
        print(f"Required: {param.name} ({param.type})")
```

### Invoking a Service

```python
# Invoke with keyword arguments
result = service.invoke(
    entity_name="OroraTech GmbH",
    postal_code="81669",
    street="St.-Martin-Strasse 112",
    city="Munich"
)

# Or with a dictionary
result = service.invoke({
    "entity_name": "OroraTech GmbH",
    "postal_code": "81669"
})
```

### Batch Invocation

```python
import asyncio
from fusionbase import Fusionbase

async def batch_process():
    client = Fusionbase()

    try:
        batch_inputs = [
            {"entity_name": "Company A", "postal_code": "12345"},
            {"entity_name": "Company B", "postal_code": "67890"},
        ]

        results = await client.services.abatch_invoke_parallel(
            "50527318",
            batch_inputs
        )

        for result in results:
            print(result)
    finally:
        await client.aclose()
        client.close()

asyncio.run(batch_process())
```

## Entities

The SDK provides access to various entity types: Locations, Organizations, Persons, Events, and Relations.

### Locations

```python
from fusionbase import Fusionbase

with Fusionbase() as client:
    # Fetch a location by ID
    location = client.entities.locations.from_id("location_id")

    print(f"Address: {location.formatted_address}")
    print(f"City: {location.city}")
    print(f"Country: {location.country}")

    if location.coordinate:
        print(f"Coordinates: {location.coordinate.latitude}, {location.coordinate.longitude}")
```

### Organizations

```python
with Fusionbase() as client:
    # Fetch an organization by ID
    org = client.entities.organizations.from_id("organization_id")

    print(f"Name: {org.name}")
    print(f"Status: {org.status.status}")
    print(f"Active: {org.is_active}")

    if org.address:
        print(f"Address: {org.address.formatted_address}")

    if org.primary_website:
        print(f"Website: {org.primary_website}")
```

### Persons

```python
with Fusionbase() as client:
    # Fetch a person by ID
    person = client.entities.persons.from_id("person_id")

    print(f"Name: {person.given_name} {person.family_name}")

    if person.home_location:
        print(f"Home: {person.home_location.formatted_address}")
```

### Async Entity Access

```python
import asyncio
from fusionbase import Fusionbase

async def fetch_entities():
    client = Fusionbase()

    try:
        # Fetch multiple entities concurrently
        location, org, person = await asyncio.gather(
            client.entities.locations.afrom_id("location_id"),
            client.entities.organizations.afrom_id("org_id"),
            client.entities.persons.afrom_id("person_id"),
        )
    finally:
        await client.aclose()
        client.close()

asyncio.run(fetch_entities())
```

## Search

The SDK provides powerful search capabilities across different entity types.

### Organization Search

```python
from fusionbase import Fusionbase
from fusionbase.search.organization_search import OrganizationSearchParams

with Fusionbase() as client:
    # Basic search
    results = client.search.organizations.search(q="OroraTech")

    print(f"Found {len(results.items)} results")

    # With search parameters
    params = OrganizationSearchParams(
        q="GmbH",
        limit=10,
        skip=0
    )
    results = client.search.organizations.search(params)

    # Access results (lazy loaded)
    for org_ref in results.items:
        org = org_ref.get()  # Loads the full entity
        print(f"Name: {org.name}")
```

### Person Search

```python
from fusionbase.search.person_search import PersonSearchParams

with Fusionbase() as client:
    params = PersonSearchParams(q="John", limit=5)
    results = client.search.persons.search(params)

    for person_ref in results.items:
        person = person_ref.get()
        print(f"Name: {person.given_name} {person.family_name}")
```

### Location Search

```python
from fusionbase.search.location_search import LocationSearchParams

with Fusionbase() as client:
    params = LocationSearchParams(q="Munich", limit=5)
    results = client.search.locations.search(params)

    for loc_ref in results.items:
        location = loc_ref.get()
        print(f"Address: {location.formatted_address}")
```

### Fusion Search (Cross-Entity)

```python
from fusionbase.search.fusion_search import FusionSearchParams

with Fusionbase() as client:
    params = FusionSearchParams(q="technology company munich")
    results = client.search.fusion.search(params)

    # Results are grouped by entity type
    for entity_type, entities in results.results.items():
        print(f"\n{entity_type.upper()}: {len(entities)} results")
```

### Async Search

```python
import asyncio
from fusionbase import Fusionbase
from fusionbase.search.organization_search import OrganizationSearchParams

async def search_async():
    client = Fusionbase()

    try:
        params = OrganizationSearchParams(q="tech", limit=10)
        results = await client.search.organizations.asearch(params)

        for org_ref in results.items:
            org = await org_ref.aget()
            print(f"Name: {org.name}")
    finally:
        await client.aclose()
        client.close()

asyncio.run(search_async())
```

## Error Handling

```python
from fusionbase import Fusionbase
from fusionbase.exceptions import (
    FusionbaseError,
    APIError,
    AuthenticationError,
    AuthorizationError,
    ResourceNotFoundError,
    ValidationError,
)

with Fusionbase() as client:
    try:
        location = client.entities.locations.from_id("invalid_id")

    except ResourceNotFoundError as e:
        print(f"Not found: {e.resource_type} with ID {e.resource_id}")

    except AuthenticationError as e:
        print(f"Authentication failed: {e}")

    except AuthorizationError as e:
        print(f"Not authorized: {e}")

    except ValidationError as e:
        print(f"Validation error: {e}")

    except APIError as e:
        print(f"API error ({e.status_code}): {e}")

    except FusionbaseError as e:
        print(f"Fusionbase error: {e}")
```

## Environment Variables

The SDK supports the following environment variables:

| Variable | Description |
|----------|-------------|
| `FUSIONBASE_API_KEY` | Your Fusionbase API key |

## Additional Resources

- [GitHub Repository](https://github.com/FusionbaseHQ/fusionbase-python)
- [Examples](https://github.com/FusionbaseHQ/fusionbase-python/tree/main/examples)
- [API Reference](https://developer.fusionbase.com)
