# Boozt Product Scraper [💰$1.8]

Extract structured data from [boozt.com](https://boozt.com) — boozt.com fashion search results into structured product rows. Each product carries its price and deal price, sizes and stock status as the store publishes them. Incremental mode emits only new or changed products between runs, ready for price monitoring.

**[Boozt Product Scraper [💰$1.8] on Apify →](https://apify.com/blackfalcondata/boozt-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from boozt.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from boozt.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "kjole",
  "location": "dk/da",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
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
| `query` | string | — | Product search keywords, e.g. a product type or brand. Required if no startUrls provided. |
| `location` | string | — | Boozt store as country/language, e.g. dk/da, se/sv, no/no, fi/fi. Defaults to dk/da. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). Boozt orders search results by relevance, not by date. With Incremental Mode on, a run only compares the first N results in that order, so new products ranked beyond N are not seen: set 0 to compare the full result set. |
| `startUrls` | array | `[]` | Paste Boozt search result URLs (https://www.boozt.com/dk/da/search/result?search_key=kjole). Each URL becomes its own search; the store and search term are read from the URL and results are deduplicated across URLs. Only the store and search term are used: any filter, sort or page parameters in the URL are ignored. When given, start URLs replace the search term and store fields. |
| `includeDetails` | boolean | `true` | Open each product page for description, material, categories and per-size availability with GTIN codes. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. Only the first Max Results results (in Boozt's relevance order) are compared; set Max Results to 0 to track the full result set. stateKey is optional and defaults to a value derived from the search inputs (queries, locations, startUrls, country), so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit records that haven't changed. |
| `emitExpired` | boolean | `false` | When incremental, also emit one row per product that is no longer found (changeType EXPIRED, with the product details remembered from earlier runs). Needs a full scan of the results, so the run reads every result page; rows count against Max Results and your cost limit. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose brand, title, colour and store match a product that has expired from an earlier run (the same item relisted under a new id). |
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
| `descriptionFormat` | enum | `"all"` | Choose which representation of the product description to include (needs Include Full Details). `all` keeps every variant; the others keep only the selected one. If a product has no HTML description, its plain-text description is kept instead so a row is never left without one. |

---

## Output fields

Every product returns the same 54-field schema. Missing values are `null` — never omitted.

- `listingId`
- `id`
- `title`
- `name`
- `entityName`
- `brand`
- `location`
- `url`
- `portalUrl`
- `color`
- `price`
- `originalPrice`
- `currency`
- `discount`
- `priceFormatted`
- `priceSek`
- `dealPrice`
- `dealDiscount`
- `dealText`
- `sizes`
- `inStock`
- `stockStatus`
- `imageUrl`
- `imageUrls`
- `eanId`
- `gtin13`
- `variantId`
- `swatchId`
- `colorVariants`
- `tags`
- `labels`
- `sponsored`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `material`
- `categories`
- `productGroupId`
- `sizeOffers`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`
- `previousPrice`
- `priceChange`
- `priceChangePct`
- `priceChangedAt`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "b8b4eab72ae84a6e77a5d8b4c0da953e29a06011687d9d180c2d2fc08c53df6c",
  "id": "32680499",
  "title": "FRCORY DR 2",
  "name": "FRCORY DR 2",
  "entityName": "Fransa",
  "brand": "Fransa",
  "location": "DK",
  "url": "https://www.boozt.com/dk/da/fransa/frcory-dr-2_32680499",
  "portalUrl": "https://www.boozt.com/dk/da/fransa/frcory-dr-2_32680499",
  "color": "PORT ROYALE",
  "price": 500,
  "originalPrice": null
}
```

*Truncated — full records contain 54 fields. See Output fields for the complete schema.*

**[Try Boozt Product Scraper [💰$1.8] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/boozt-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0018 |

See the [actor on Apify](https://apify.com/blackfalcondata/boozt-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape boozt.com?**
Use this actor on Apify to extract structured data from boozt.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get boozt.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape boozt.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check boozt.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Boozt Product Scraper [💰$1.8]](https://apify.com/blackfalcondata/boozt-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
