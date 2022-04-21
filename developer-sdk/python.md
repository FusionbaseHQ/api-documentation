# Python

We also offer a python package to natively integrate Fusionbase into your development enviroment.

## Installation in Python - PyPI release

Fusionbase is on [PyPI](https://test.pypi.org/project/fusionbase-test-package/), so you can use `pip`to install it.

```bash
pip install fusionbase
```

#### Anaconda

The easiest way to install Fusionbase is through conda-forge: `conda install -c fusionbase`.

### Getting Started

Got to [examples](https://github.com/FusionbaseHQ/fb\_user\_\_fusionbase\_py/tree/main/examples) to deep dive into Fusionbase and see various examples on how to use the package.

Here are some examples for a quick start:

#### Data Streams

The Data Stream module lets you conveniently access the data and metadata of all Data Streams. Each stream can be accessed via its unique stream id or label.

**Setup**

```python
# Import Fusionbase
from fusionbase.DataStream import DataStream

# Create a new datastream
# Provide your API Key and the Fusionbase API URI (usually: https://api.fusionbase.com/api/v1)
data_stream = DataStream(auth={"api_key": "*** SECRET CREDENTIALS ***"},
                      connection={"base_uri": "https://api.fusionbase.com/api/v1"})

datastream_key = 28654971
```

**Human readable datastream information:**

```python
# Print a nice table containing the meta data of the stream
data_stream.pretty_meta_data(key=datastream_key)
```

**Get Data:**

```python
data = data_stream.get_data(key=datastream_key)
print(data)
```

**Get Data as a** [**pandas**](https://pandas.pydata.org) **Dataframe:**

```python
df = data_stream.get_dataframe(key=datastream_key)
print(df)
```

#### Data Services

A data service can be seen as an API that returns a certain output for a specific input. For example, our address [normalization service](https://app.fusionbase.com/share/25127186) parses an address and returns the structured and normalized parts of it.

**Setup**

```python
# Import Fusionbase
from fusionbase.DataService import DataService

# Create a new dataservice
# Provide your API Key and the Fusionbase API URI (usually: https://api.fusionbase.com/api/v1)
data_service = DataService(auth={"api_key": "*** SECRET CREDENTIALS ***"},
                      connection={"base_uri": "https://api.fusionbase.com/api/v1"})

dataservice_key = 23622632
```

**Human readable dataservice information:**

```python
# Retrieves the metadata from a Service by giving a Service specific key and prints it nicely to console
data_service.pretty_meta_data(key=dataservice_key)
```

**Human readable dataservice definition:**

```python
# Retrieve the request definition (such as required parameters) from a Service by giving a Service specific key and print it to console.
data_service.pretty_request_definition(key=dataservice_key)
```

**Invoke a dataservice:**

```python
# Invoke a service by providing input data
payload = [
{
	"name": "address_string", # THIS IS THE NAME OF THE INPUT VALUE
	"value": "Agnes-Pockels-Bogen 1, 80992 München" # THE VALUE FOR THE INPUT
}
]

result = data_service.invoke(key=dataservice_key, parameters=payload)

print(result)
```

### Changelog

#### Version 0.1 (2022.04.20)

* Initial release

### Contributing

Contributing to Fusionbase can be in contributions to the code base, sharing your experience and insights in the community on the Forums, or contributing to projects that make use of Fusionbase. Please see the [contributing guide](https://github.com/FusionbaseHQ/fb\_user\_\_fusionbase\_py/blob/main/docs/CONTRIBUTING.md) for more specifics.

### License

Fusionbase is licensed under the [MIT license](https://github.com/FusionbaseHQ/fb\_user\_\_fusionbase\_py/blob/main/LICENSE).

