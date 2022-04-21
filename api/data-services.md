---
description: >-
  Fusionbase not only provides stream access to a large number of data sources
  but also to a large number of intelligent data services.
---

# Data Services

A data service can be seen as an API that returns a certain output for a specific input. \
For example, our [address normalization service](https://app.fusionbase.com/share/25127186) parses an address and returns the structured and normalized parts of it.

{% hint style="info" %}
All services work the exact same way - No need to learn and adapt to a tonne of different API services and documentations.
{% endhint %}

There is only a single endpoint to access services through Fusionbase:\
[https://api.fusionbase.com/api/v1/data-service/invoke](https://api.fusionbase.com/api/v1/data-service/invoke)

{% swagger method="post" path="invoke" baseUrl="https://api.fusionbase.com/api/v1/data-service/" summary="Invoke data service" %}
{% swagger-description %}
Takes the service specific input values and returns the result of the service. 
{% endswagger-description %}

{% swagger-parameter in="body" name="data_service_key" type="String" required="true" %}
The service ID as specified on the service's API page.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="inputs" type="List" %}
Service input parameter name and value pairs as list of dictionary.
{% endswagger-parameter %}
{% endswagger %}

#### Example

All services are used in the exact same way. Let's take the [address normalization service](https://app.fusionbase.com/share/25127186) as an example on how to use them via the API.

![Fusionbase Address Normalization Service UI](<../.gitbook/assets/Bildschirmfoto 2021-12-17 um 15.58.17.png>)

As we can see, the address normalizer takes a single input value which is named `address_string` . The service has the ID `23622632`

A curl request to invoke the service with the input _Agnes-Pockels-Bogen 1, 80992 München_ for the __ `address_string` __ would look like the following:

```bash
curl -X "POST" "https://api.fusionbase.com/api/v1/data-service/invoke" \
-H 'X-API-KEY: YOUR_API_KEY' \
-H 'Content-Type: application/json; charset=utf-8' \
-d $'{
"inputs": [
{
	"name": "address_string", # THIS IS THE NAME OF THE INPUT VALUE
	"value": "Agnes-Pockels-Bogen 1, 80992 München" # THE VALUE FOR THE INPUT
}],
"data_service_key": "23622632" # THE ID OF THE SERVICE
}'
```
