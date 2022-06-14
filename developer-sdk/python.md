## Installation in Python - PyPI release

Fusionbase is on PyPI, so you can use `pip` to install it.

```bash
pip install fusionbase
```

If you want to use all features, you must make sure that pandas and numpy are installed.

```bash
pip install pandas
pip install numpy
```

## Getting Started

Got to [examples](https://github.com/FusionbaseHQ/fusionbase-python/tree/main/examples) to deep dive into Fusionbase and see various examples on how to use the package.

Here are some Examples for a quick start:

### Data Streams
The Data Stream module lets you conveniently access data and metadata of all Data Streams. Each stream can be accessed via its unique stream id or label.

**Setup**
```python
# Import Fusionbase
from fusionbase.Fusionbase import Fusionbase

# Create a new datastream
# Provide your API Key and the Fusionbase API URI (usually: https://api.fusionbase.com/api/v1)
fusionbase = Fusionbase(auth={"api_key": "*** SECRET CREDENTIALS ***"},
                      connection={"base_uri": "https://api.fusionbase.com/api/v1"})

data_stream_key = "28654971"
data_stream = fusionbase.get_datastream(data_stream_key)
```

**Human readable datastream information:**
```python
# Print a nice table containing the meta data of the stream
data_stream.pretty_meta_data()
```

**Get Data:**
```python
data = data_stream.get_data()
print(data)
```

**Get Data as a [pandas](https://pandas.pydata.org/) Dataframe:**
```python
df = data_stream.get_dataframe()
print(df)
```

### Data Services
A data service can be seen as an API that returns a certain output for a specific input. 
For example, our address [normalization service](https://app.fusionbase.com/share/25127186) parses an address and returns the structured and normalized parts of it.

**Setup**
```python
# Import Fusionbase
from fusionbase.Fusionbase import Fusionbase

# Create a new dataservice
# Provide your API Key and the Fusionbase API URI (usually: https://api.fusionbase.com/api/v1)
fusionbase = Fusionbase(auth={"api_key": "*** SECRET CREDENTIALS ***"},
                      connection={"base_uri": "https://api.fusionbase.com/api/v1"})

data_service_key = "23622632"
data_service = fusionbase.get_dataservice(data_service_key)
```

**Human readable dataservice information:**
```python
# Retrieves the metadata from a Service by giving a Service specific key and prints it nicely to console
data_service.pretty_meta_data()
```

**Human readable dataservice definition:**
```python
# Retrieve the request definition (such as required parameters) from a Service by giving a Service specific key and print it to console.
data_service.pretty_request_definition()
```

**Invoke a dataservice:**

```python
# Invoke a service by providing input data

# The following lines of code are equivalent
# Services can be invoked directly by their parameter names
result = data_service.invoke(address_string="Agnes-Pockels-Bogen 1, 80992 München")


# Or using a list of parameter key and value pairs
payload = [
    {
        "name": "address_string",  # THIS IS THE NAME OF THE INPUT VALUE
        "value": "Agnes-Pockels-Bogen 1, 80992 München"  # THE VALUE FOR THE INPUT
    }
]
result = data_service.invoke(parameters=payload)

print(result)
```
