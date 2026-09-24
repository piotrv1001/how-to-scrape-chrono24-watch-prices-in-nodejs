# How to Scrape Chrono24 Watch Prices in Node.js

This example calls our [Chrono24 Listings Scraper](https://apify.com/piotrv1001/chrono24-listings-scraper) on Apify. It does not implement a scraper from scratch.

## What this example does

- Requests three GMT-Master listings with full watch details
- Waits for the Actor run to finish
- Fetches and prints the default dataset
- Retains reference number, condition, and asking price for comparison

## Prerequisites

- Node.js 18 or newer
- An Apify account and API token

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and set `APIFY_TOKEN` to your token. Do not commit `.env`.

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [{ url: 'https://www.chrono24.com/rolex/gmt-master--mod3.htm' }],
    scrapeDetails: true,
    maxItems: 3,
    country: 'us',
};

// Run the Actor and wait for it to finish
const run = await client.actor('piotrv1001/chrono24-listings-scraper').call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

[`sample-output.json`](./sample-output.json) has abbreviated records from our September 24, 2026 three-watch run. The watches have different reference numbers and conditions, so they are not three direct comparables. These are listing asking prices, not completed sale prices.

## Use cases

- Build an asking-price dataset for one watch reference
- Review condition and box-and-papers differences
- Track seller and locale alongside a quoted price
- Shortlist listings for manual inspection

## Try the Actor on Apify

**[Open the Chrono24 Listings Scraper on Apify](https://apify.com/piotrv1001/chrono24-listings-scraper)**

## Related resources

- [How to compare Chrono24 watch prices by reference](https://www.falconscrape.com/blog/how-to-compare-chrono24-watch-prices-by-reference)

## License

MIT
