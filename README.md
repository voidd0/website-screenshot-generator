# Website Screenshot Generator: export to CSV or JSON with Python, Node.js or curl

Capture a public web page as a PNG, JPEG or WEBP image: any viewport size, full page, retina scaling, optional cookie-banner hiding. Batch mode: process up to 1000 items per run.

This repository has ready scripts for the [Website Screenshot Generator: URL to PNG, JPEG or WEBP](https://apify.com/v0iddo/url-screenshot) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/url-screenshot), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "url": "https://example.com"
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `url` | string | Public http(s) URL (robots.txt is honoured), or an inline data:text/html URL |
| `width` | integer | Viewport width in CSS px |
| `height` | integer | Viewport height in CSS px |
| `full_page` | boolean | Capture the whole scrollable page (capped at 16384 CSS px / 50 megapixels) |
| `format` | string | File format of the result. |
| `quality` | integer | jpeg / webp quality |
| `delay_ms` | integer | Extra wait after load, before the shot |
| `device_scale_factor` | string | Pixel density (2 = retina) (a number) |
| `block_cookie_banners` | boolean | Hide common cookie-consent dialogs (best effort) |
| `items` | array | Optional. A list of input objects with the same fields as above; every object is processed in its own right and the top-level fields act as shared defaults. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "index": 0,
  "file_url": "https://api.apify.com/v2/key-value-stores/LqTDdcQS8PgkubKvV/records/result-001-screenshot.png?signature=VtU9nHH9mOQRCopF7wsF",
  "file_key": "result-001-screenshot.png",
  "filename": "screenshot.png",
  "mime": "image/png",
  "bytes": 71152,
  "width": 1280,
  "height": 800,
  "format": "png",
  "final_url": "https://example.com/",
  "http_status": 200,
  "title": "Example Domain",
  "load_state": "networkidle",
  "truncated": false,
  "blocked_requests": 0,
  "credits": 3,
  "ms": 2044
}
```

Fields: `index`, `file_url`, `file_key`, `filename`, `mime`, `bytes`, `width`, `height`, `format`, `final_url`, `http_status`, `title`, `load_state`, `truncated`, `blocked_requests`, `credits`, `ms`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/url-screenshot').call({"url": "https://example.com"});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~url-screenshot/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"url": "https://example.com"}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$3.40 per 1,000 results**, nothing per run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
