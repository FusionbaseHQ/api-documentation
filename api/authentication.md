# Authentication

API requests are authenticated using API keys which are sent via the custom `X-API-KEY` header. Any request that doesn't include an API key will return an authentication error.

You can view and manage your API keys at the [profile settings](https://app.fusionbase.com/account/profile) page in your Fusionbase account.

All API requests must be made over HTTPS. Calls made over plain HTTP will be redirected to HTTPS. API requests without authentication will fail.

{% hint style="warning" %}
Your API keys carry many privileges, so be sure to keep them secure! **Do not share** your secret API keys in **publicly** accessible areas such as GitHub, client-side code, and so forth.
{% endhint %}

## Creating API Keys <a href="#api-key-generation" id="api-key-generation"></a>

Go to your [account profile](https://app.fusionbase.com/account/profile) and press the _**New API Key**_ button.

<mark style="color:red;">Do not disclose and share your API key publicly.</mark>

![Account profile page](../.gitbook/assets/screen\_api\_key.png)

## Authenticate Requests <a href="#authenticate-requests" id="authenticate-requests"></a>

The API key is added via the `x-api-key` property in the request header.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.fusionbase.com/api/v1/data-stream/get/430410" \
-H 'X-API-KEY: 'YOUR_API_KEY \
-H 'Content-Type: application/json; charset=utf-8' \

```
{% endtab %}

{% tab title="Python" %}
```python
import requests

headers = {
    'x-api-key': 'YOUR_API_KEY',
    'Content-Type': 'application/json',

response = requests.get('https://api.fusionbase.com/api/v1/data-stream/get/430410', headers=headers)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.io.IOException;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

class Main {

	public static void main(String[] args) throws IOException {
		URL url = new URL("https://api.fusionbase.com/api/v1/data-stream/get/430410");
		HttpURLConnection httpConn = (HttpURLConnection) url.openConnection();
		httpConn.setRequestMethod("GET");

		httpConn.setRequestProperty("X-API-KEY", "YOUR_API_KEY");
		httpConn.setRequestProperty("Content-Type", "application/json; charset=utf-8");

		InputStream responseStream = httpConn.getResponseCode() / 100 == 2
				? httpConn.getInputStream()
				: httpConn.getErrorStream();
		Scanner s = new Scanner(responseStream).useDelimiter("\\A");
		String response = s.hasNext() ? s.next() : "";
		System.out.println(response);
	}
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	client := &http.Client{}
	req, err := http.NewRequest("GET", "https://api.fusionbase.com/api/v1/data-stream/get/430410", nil)
	if err != nil {
		log.Fatal(err)
	}
	req.Header.Set("X-API-KEY", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json; charset=utf-8")
	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close()
	bodyText, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s\n", bodyText)
}
```
{% endtab %}

{% tab title="Node" %}
```javascript
var fetch = require('node-fetch');

fetch('https://api.fusionbase.com/api/v1/data-stream/get/430410', {
    headers: {
        'X-API-KEY': 'YOUR_API_KEY',
        'Content-Type': 'application/json; charset=utf-8'
    }
});
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-API-KEY' => 'YOUR_API_KEY',
    'Content-Type' => 'application/json; charset=utf-8'
);
$response = Requests::get('https://api.fusionbase.com/api/v1/data-stream/get/430410', $headers);\
```
{% endtab %}
{% endtabs %}

