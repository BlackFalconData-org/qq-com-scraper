# QQ Music Scraper

Extract structured data from [qq.com](https://qq.com) — Search QQ Music by keyword and export structured song records: song and artist names, album, duration, release date, 128 and 320 kbps, FLAC and OGG file sizes, and the preview window. Up to 600 songs per search, no login or proxy needed.

**[QQ Music Scraper on Apify →](https://apify.com/blackfalcondata/qq-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Incremental mode** — Only get new or changed songs since your last run. Content hash per song — no duplicates, no re-processing.

**Change classification** — Track cross-run duplicate detection across runs. Build audit trails of how songs evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N songs (up to 600). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every song returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured songs from qq.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor songs, track trends, and analyze market dynamics with structured, deduplicated data from qq.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per song is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "周杰伦",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Song, artist or album keywords — Chinese or Latin script both work (e.g. "周杰伦", "Jay Chou", "告白气球"). Use a JSON array for several searches in one run, e.g. ["周杰伦","五月天"]. |
| `maxResults` | integer | `100` | Maximum total songs (0 = as many as the search returns). A single keyword search returns at most 600 songs. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `startUrls` | array | — | One or more QQ Music song-search URLs, for example https://y.qq.com/n/ryqq/search?w=jay%20chou&t=song. Each URL is searched for its keyword. When set, the Search Term field is ignored. Song-detail pages cannot be looked up by link. |

---

## Output fields

Every song returns the same 30-field schema. Missing values are `null` — never omitted.

- `recordType`
- `songId`
- `songMid`
- `songName`
- `singerNames`
- `singers`
- `albumId`
- `albumMid`
- `albumName`
- `durationSec`
- `releaseDate`
- `size128Kbps`
- `size320Kbps`
- `sizeFlacBytes`
- `sizeOggBytes`
- `previewStartMs`
- `previewEndMs`
- `sourceUrl`
- `portalUrl`
- `scrapedAt`
- `trackNumber`
- `isOnlyOnQq`
- `lyricNote`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per song. Here is a real example from a production run:

```json
{
  "recordType": "song",
  "songId": 449200,
  "songMid": "0042QMDR1VzSsx",
  "songName": "说好的幸福呢",
  "singerNames": "周杰伦",
  "singers": [
    {
      "id": 4558,
      "mid": "0025NhlN2yWrP4",
      "name": "周杰伦",
      "name_hilight": "周杰伦"
    }
  ],
  "albumId": 36062,
  "albumMid": "002Neh8l0uciQZ",
  "albumName": "魔杰座",
  "durationSec": 256,
  "releaseDate": "2008-10-14",
  "size128Kbps": 4108738
}
```

*Truncated — full records contain 30 fields. See Output fields for the complete schema.*

**[Try QQ Music Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/qq-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.005 |
| Result | $0.0034 |
| Song record | $0.0034 |

See the [actor on Apify](https://apify.com/blackfalcondata/qq-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape qq.com?**
Use this actor on Apify to extract structured data from qq.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get qq.com data as JSON, CSV, or Excel?**
The actor writes each song to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape qq.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check qq.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each song gets a content hash. On subsequent runs, only new or changed songs are emitted — saving time, compute, and storage. Expired songs can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [QQ Music Scraper](https://apify.com/blackfalcondata/qq-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
