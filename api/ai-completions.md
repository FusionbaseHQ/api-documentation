---
description: >-
  Experience Fusionbase AI: Seamlessly Interact with Every Entity in the
  Acclaimed Fusionbase Data Hub Through Advanced Chat-Based Contextual Access.
---

# AI Completions

{% hint style="danger" %}
The Fusionbase AI API is not yet publicly accessible for all accounts. If you are interested in getting early access, drop us a message via [info@fusionbase.com](mailto:info@fusionbase.com)
{% endhint %}

The `/api/v2/chat/completions` endpoint of the Fusionbase API is designed to generate chat completions based on a given context and a series of messages. This endpoint is particularly useful for obtaining context-aware responses in a conversational format.

### Request Headers

* **Content-Type**: application/json
* **X-API-KEY**: Your Fusionbase API key.

### Request Body

* **model (required)**: String. Specifies the model to be used for generating completions. For example, `"fusion-one"`.
* **linked\_context (optional)**: Array of Objects. Each object represents a context entity linked to the chat.
  * **type**: String. The type of the linked context entity (e.g., "ORGANIZATION", "PERSON").
  * **id**: String. A unique identifier for the context entity.
* **messages (required)**: Array of Objects. Each object represents a message in the conversation.
  * **role**: String. The role of the message sender ("system" or "user").
  * **content**: String. The content of the message.

### Request Samples

{% tabs %}
{% tab title="Python" %}
```python
import requests

url = "https://api.fusionbase.com/api/v2/chat/completions"
headers = {
    "Content-Type": "application/json",
    "X-API-KEY": "<YOUR API KEY>"
}
data = {
    "model": "fusion-one",
    "linked_context": [
        {"type": "ORGANIZATION", "id": "35b263b5408bc85899b5a4fab6be5d75"},
        {"type": "PERSON", "id": "bc6a357dc563ad838e213ff06e0c1c91"}
    ],
    "messages": [
        {"role": "system", "content": "You are an insurance and underwriting expert."},
        {"role": "user", "content": "What are potential insurance risks of the given company?"}
    ]
}

response = requests.post(url, json=data, headers=headers)
print(response.json())
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const axios = require('axios');

const url = "https://api.fusionbase.com/api/v2/chat/completions";
const headers = {
    "Content-Type": "application/json",
    "X-API-KEY": "<YOUR API KEY>"
};
const data = {
    model: "fusion-one",
    linked_context: [
        { type: "ORGANIZATION", id: "35b263b5408bc85899b5a4fab6be5d75" },
        { type: "PERSON", id: "bc6a357dc563ad838e213ff06e0c1c91" }
    ],
    messages: [
        { role: "system", content: "You are an insurance and underwriting expert." },
        { role: "user", content: "What are potential insurance risks of the given company?" }
    ]
};

axios.post(url, data, { headers: headers })
    .then(response => {
        console.log(response.data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```
{% endtab %}

{% tab title="Java" %}
```java
import okhttp3.*;

import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        OkHttpClient client = new OkHttpClient();

        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType, "{\n    \"model\": \"fusion-one\",\n    \"linked_context\": [\n        {\n            \"type\": \"ORGANIZATION\",\n            \"id\": \"35b263b5408bc85899b5a4fab6be5d75\"\n        },\n        {\n            \"type\": \"PERSON\",\n            \"id\": \"bc6a357dc563ad838e213ff06e0c1c91\"\n        }\n    ],\n    \"messages\": [\n        {\n            \"role\": \"system\",\n            \"content\": \"You are an insurance and underwriting expert.\"\n        },\n        {\n            \"role\": \"user\",\n            \"content\": \"What are potential insurance risks of the given company?\"\n        }\n    ]\n}");
        Request request = new Request.Builder()
          .url("https://api.fusionbase.com/api/v2/chat/completions")
          .post(body)
          .addHeader("Content-Type", "application/json")
          .addHeader("X-API-KEY", "<YOUR API KEY>")
          .build();

        Response response = client.newCall(request).execute();
        System.out.println(response.body().string());
    }
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

func main() {
    url := "https://api.fusionbase.com/api/v2/chat/completions"
    requestBody := map[string]interface{}{
        "model": "fusion-one",
        "linked_context": []map[string]string{
            {"type": "ORGANIZATION", "id": "35b263b5408bc85899b5a4fab6be5d75"},
            {"type": "PERSON", "id": "bc6a357dc563ad838e213ff06e0c1c91"},
        },
        "messages": []map[string]string{
            {"role": "system", "content": "You are an insurance and underwriting expert."},
            {"role": "user", "content": "What are potential insurance risks of the given company?"},
        },
    }

    jsonData, err := json.Marshal(requestBody)
    if err != nil {
        panic(err)
    }

    req, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
    req.Header.Set("Content-Type", "application/json")
    req.Header.Set("X-API-KEY", "<YOUR API KEY>")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println("Response: ", string(body))
}

```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://api.fusionbase.com/api/v2/chat/completions \
-H "Content-Type: application/json" \
-H "X-API-KEY: <YOUR API KEY>" \
-d '{
    "model": "fusion-one",
    "linked_context": [
        {
            "type": "ORGANIZATION",
            "id": "35b263b5408bc85899b5a4fab6be5d75"
        },
        {
            "type": "PERSON",
            "id": "bc6a357dc563ad838e213ff06e0c1c91"
        }
    ],
    "messages": [
        {
            "role": "system",
            "content": "You are an insurance and underwriting expert."
        },
        {
            "role": "user",
            "content": "What are potential insurance risks of the given company?"
        }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response Structure

* **id**: A unique 32-character alphanumeric hash identifying the completion request.
* **object**: String, indicating the type of the object, here `"chat.completion"`.
* **created**: Unix timestamp indicating the creation time of the response.
* **model**: String, indicating the model used, revised to match the request's model.
* **usage**: Object detailing the token usage.
  * **prompt\_tokens**: Number of tokens used in the prompt.
  * **completion\_tokens**: Number of tokens used in the completion.
  * **total\_tokens**: Total number of tokens used.
* **choices**: Array of objects, each representing a generated response.
  * **message**: Object containing the role and content of the response.
  * **logprobs**: Null or containing log probabilities for the response.
  * **finish\_reason**: Reason for the completion's end, e.g., "stop".
  * **index**: The index of the choice.

### Response Sample

```json
{
    "id": "1234abcd5678efgh9012ijkl3456mnop",
    "object": "chat.completion",
    "created": 1677858242,
    "model": "fusion-one",
    "usage": {
        "prompt_tokens": 13,
        "completion_tokens": 7,
        "total_tokens": 20
    },
    "choices": [
        {
            "message": {
                "role": "assistant",
                "content": "The potential insurance risks for the given company include..."
            },
            "logprobs": null,
            "finish_reason": "stop",
            "index": 0
        }
    ]
}
```

#### Error Handling

The API uses standard HTTP response codes. Errors are indicated with 4xx (client errors) and 5xx (server errors), each accompanied by a message detailing the issue.

#### Notes

* Keep the API key confidential.
* Context and message relevance is crucial for accurate results.
