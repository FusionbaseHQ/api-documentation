# Quick Start

Welcome to the Fusionbase Public API, your gateway to programmatically accessing a rich ecosystem of data streams. Before diving into the API, we recommend beginning your journey on our website to explore the vast array of data available to you.

### Step 1: Explore Data on the Fusionbase Website

#### Discovering Data Streams

Visit the [Fusionbase Data](https://fusionbase.com) Hub to browse through the datasets. Here you can:

* **Search** for topics that interest you.
* **Preview** data to understand its structure and contents.
* **Identify** the datasets that can provide the insights you need.

Take your time to explore. Once you find the data that aligns with your goals, note the Stream ID – you'll use this when interacting with the API.

### Step 2: Programmatically Access Data with the API

#### Authenticate with Your API Key

To start using the API:

1. Obtain your API key from the [API Keys Management](https://fusionbase.com/en/account/user/api-keys) page in your Fusionbase account.
2. Use the `X-API-KEY` header to authenticate your API requests.

#### Base URL for API Requests

For all your API calls, use the base URL:

```
https://api.fusionbase.com/api/v2
```

#### Retrieving Your Selected Data Stream (or Any Other Data Entity)

After identifying the data you want from our website, you can retrieve it programmatically via the API endpoint:

```bash
https://api.fusionbase.com/api/v2/stream/data/[Stream ID]
```

Replace `[Stream ID]` with the ID of the data stream you've selected.

#### Code Snippets for API Access

We provide a variety of code snippets to help you make your first API request in the language of your choice. Visit our [Code Snippets Section](api/data-streams/getting-a-data-stream/) for examples in Python, NodeJS, Java, C#, and more.

### Step 3: Integrate Data into Your Applications

Once you have successfully made requests using our sample code, it's time to integrate Fusionbase data streams into your applications. This allows you to automate data retrieval and keep your applications enriched with the latest data.

### Need Help?

If you encounter any issues or have questions as you explore and use the Fusionbase API, our support team is ready to assist you. Contact us via our [support email](mailto:support@fusionbase.com).
