---
description: >-
  Some Data Streams are too large to get all the records via the API in a single
  call. In such cases, the API lets you paginate the results via the skip and
  limit parameters.
---

# Pagination

{% swagger method="get" path="/get/[STREAM_ID]?skip=0&limit=10" baseUrl="https://api.fusionbase.com/api/v1/data-stream" summary="Pagination" %}
{% swagger-description %}
Some Data Streams are too large to get all the records via the API in a single call. In such cases the API lets you paginate the results via the 

`skip`

 and 

`limit`

parameters.
{% endswagger-description %}

{% swagger-parameter in="path" name="STREAM_ID" required="true" %}
The ID of the Data Stream
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Integer" %}
Indicates the number of records that are retrieved in the result set.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="skip" type="Integer" %}
Indicates the number of records that should be skipped from the beginning of the Data Stream.
{% endswagger-parameter %}
{% endswagger %}

**Example**\
****`?skip=`<mark style="color:blue;">10</mark>`&limit=`<mark style="color:red;">10</mark>\
\[<mark style="color:blue;">1</mark>,<mark style="color:blue;">2</mark>,<mark style="color:blue;">3</mark>,<mark style="color:blue;">4</mark>,<mark style="color:blue;">5</mark>,<mark style="color:blue;">6</mark>,<mark style="color:blue;">7</mark>,<mark style="color:blue;">8</mark>,<mark style="color:blue;">9</mark>,<mark style="color:blue;">10</mark>,<mark style="color:red;">11</mark>,<mark style="color:red;">12</mark>,<mark style="color:red;">13</mark>,<mark style="color:red;">14</mark>,<mark style="color:red;">15</mark>,<mark style="color:red;">16</mark>,<mark style="color:red;">17</mark>,<mark style="color:red;">18</mark>,<mark style="color:red;">19</mark>,<mark style="color:red;">20</mark>,21,22,23,24,25,...]

Assuming that each number stands for a record in the Data Stream, the <mark style="color:blue;">blue</mark> ones are <mark style="color:blue;">skipped</mark> and not in the result set. The <mark style="color:red;">red</mark> ones, defined by the <mark style="color:red;">limit</mark> parameter, represent the result set.
