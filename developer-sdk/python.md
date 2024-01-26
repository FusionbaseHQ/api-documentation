# Python

{% hint style="warning" %}
Please be aware that the Python SDK is undergoing significant updates in preparation for its first major release. This upcoming version will introduce breaking changes. We advise caution when using the current version of the SDK, as future updates may require adjustments in your existing code.
{% endhint %}

### Installation in Python - PyPI release

Fusionbase is on PyPI, so you can use `pip` to install it.

```bash
pip install fusionbase
```

If you want to use all features, you must make sure that pandas and numpy are installed.

```bash
pip install pandas
pip install numpy
```

Fusionbase by default uses the standard JSON library of Python to serialize and locally store data. However, you can use the faster `orjson` library as a drop-in replacement.&#x20;

Therefore, just install `orjson` and Fusionbase will automatically detect and use it.

```bash
pip install orjson
```

### Getting Started

Got to [examples](https://github.com/FusionbaseHQ/fusionbase-python/tree/main/examples) to deep dive into Fusionbase and see various examples on how to use the package.

Here are some Examples for a quick start:

#### Data Streams

The Data Stream module lets you conveniently access data and metadata of all Data Streams. Each stream can be accessed via its unique stream id or label.



**Setup**

```python
# Import Fusionbase
from fusionbase import Fusionbase

# Create a new datastream
# Provide your API Key
fusionbase = Fusionbase(auth={"api_key": "*** SECRET CREDENTIALS ***"})

# If you prefer to have extended logging output and information 
# Like a progress bar for downloading datastreams etc.
# Turn on the log 
fusionbase = Fusionbase(auth={"api_key": "*** SECRET CREDENTIALS ***"}, log=True)

# Get the datastream with the key "28654971"
data_stream_key = "28654971"
data_stream = fusionbase.get_datastream(data_stream_key)
```



**Human readable datastream information**

```python
# Print a nice table containing the meta data of the stream
data_stream.pretty_meta_data()
```



**Getting the data**

The samples below show how to retrieve the data of a datastream as a list of dictionaries. Each element in the list represents one row within the dataset.&#x20;

Note that the data can by hierarchical.

```python
# The following returns the full datastream as a list of dictionaries
# It uses a local cache if available
data = data_stream.get_data()
print(data)

# Get always the latest data from Fusionbase
data = data_stream.get_data(live=True)
print(data)

# If you only need a subset of the columns
# You'll gain much performance by only selecting those columns
data = data_stream.get_data(fields=["NAME_OF_COLUMN_1", "NAME_OF_COLUMN_N"])
print(data)

# If you need only an excerpt of the data you can use skip and limit
# The sample below gets the 10 first rows
data = data_stream.get_data(skip=0, limit=10)
print(data)

```



**Get Data as a** [**pandas**](https://pandas.pydata.org/) **DataFrame**

If you are working with pandas, it is probably the most convenient way to load to data directly as a pandas DataFrame.

```python
# Load the data from Fusionbase, cache it and put it in a pandas DataFrame
df = data_stream.as_dataframe()
print(df)

# Force ignoring the cache and make sure to get the latest data
df = data_stream.as_dataframe(live=True)
print(df)
```



**Storing the data**

Large datasets potentially do not fit into the memory. Therefore, it is possible to get the data of a stream directly as partitioned files.&#x20;

The folder structure is automatically created and always like `./{ID-OF-THE-STREAM}/data/*`

```python
from pathlib import Path

# Store as JSON files
data_stream.as_json_files(storage_path=Path("./data/"))

# Store as CSV files
data_stream.as_csv_files(storage_path=Path("./data/"))

# Store as Pickle files
data_stream.as_pickle_files(storage_path=Path("./data/"))
```

####

#### Data Services

A data service can be seen as an API that returns a certain output for a specific input. For example, our address [normalization service](https://app.fusionbase.com/share/25127186) parses an address and returns the structured and normalized parts of it.

**Setup**

```python
# Import Fusionbase
from fusionbase.Fusionbase import Fusionbase

# Create a new dataservice
# Provide your API Key and the Fusionbase API URI (usually: https://api.fusionbase.com/api/v1)
fusionbase = Fusionbase(auth={"api_key": "*** SECRET CREDENTIALS ***"})

data_service_key = "50527318"
data_service = fusionbase.get_dataservice(data_service_key)
```

**Human readable data service information:**

```python
# Retrieves the metadata from a Service by giving a Service specific key and prints it nicely to console
data_service.pretty_meta_data()
```

**Human readable data service definition:**

```python
# Retrieve the request definition (such as required parameters) from a Service by giving a Service specific key and print it to console.
data_service.pretty_request_definition()
```

**Invoke a data service:**

```python
# Invoke a service by providing input data

# The following lines of code are equivalent
# Services can be invoked directly by their parameter names
result = data_service.invoke(company_name="OroraTech GmbH", zip_code="81669")

print(result)
```
