<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Owler Scraper API by Piloterr – scrape Owler data as JSON" width="100%"></a>
</p>

# Owler Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Owler Scraper API** lets you extract public data from Owler as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Owler Company Info API** — Retrieve detailed company intelligence from Owler, including revenue, employee count, CEO info, competitors, funding, and industry sectors.
- **Owler Search API** — Search for companies on Owler by name or keyword, returning a list of matching companies with slugs, logos, domains, and profile URLs.

Use it for lead generation, CRM enrichment, market mapping, investment research or job market analytics.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Owler Company Info API](#owler-company-info-api) | `/v2/owler/company/info` | 1 | Retrieve detailed company intelligence from Owler, including revenue, employee count, CEO info, competitors, funding, and industry sectors. |
| [Owler Search API](#owler-search-api) | `/v2/owler/search` | 1 | Search for companies on Owler by name or keyword, returning a list of matching companies with slugs, logos, domains, and profile URLs. |

## Table of contents

- [How it works](#how-it-works)
- [Owler Company Info API](#owler-company-info-api)
- [Owler Search API](#owler-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Owler Company Info API

Retrieve detailed company intelligence from Owler, including revenue, employee count, CEO info, competitors, funding, and industry sectors.

`GET | POST https://api.piloterr.com/v2/owler/company/info` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | The company to look up on Owler. Multiple formats are accepted: |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/owler/company/info",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "https://owler.com/company/tesla",
    },
    timeout=60,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type OwlerCompanyInfo = {
  cg?: {
    founded?: string;
    revenue?: number;
    ownership?: string;
    ceo_detail?: {
      ceo_pic?: string;
      last_name?: string;
      ceo_rating?: number;
      first_name?: string;
      designation?: string;
    };
    headquarters?: {
      city?: string;
      state?: string;
      country?: string;
      state_display_name?: string;
    };
    revenue_range?: string;
    total_funding?: number;
    employee_count?: number;
    employee_range?: string;
    industry_sectors?: string[];
    formatted_funding?: string;
    formatted_revenue?: string;
    company_basic_info?: {
      logo?: string;
      cp_link?: string;
      website?: string;
      team_name?: string;
      company_id?: number;
      short_name?: string;
    };
    formatted_employee_count?: string;
  }[];
};

const params = new URLSearchParams({
  "query": "https://owler.com/company/tesla",
});

const response = await fetch(`https://api.piloterr.com/v2/owler/company/info?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as OwlerCompanyInfo;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/owler/company/info
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "https://owler.com/company/tesla"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "cg": [
    {
      "founded": "1916",
      "revenue": 69444000000,
      "ownership": "Public",
      "ceo_detail": {
        "ceo_pic": "https://image4.owler.com/ceo/Robert K._Ortberg_owler_20240807_190640_original.png",
        "last_name": "example",
        "ceo_rating": 67,
        "first_name": "example",
        "designation": "President & CEO"
      },
      "headquarters": {
        "city": "Arlington",
        "state": "US-VA",
        "country": "USA",
        "state_display_name": "Virginia"
      },
      "revenue_range": "$50B - 100B",
      "total_funding": 45000000000,
      "employee_count": 156000,
      "employee_range": "100,000 - 9,999,999",
      "industry_sectors": [
        "Defense",
        "Aviation"
      ],
      "formatted_funding": "45B",
      "formatted_revenue": "69.4B",
      "company_basic_info": {
        "logo": "https://image4.owler.com/logo/boeing_owler_20190615_044954_small.png",
        "cp_link": "https://www.owler.com/company/boeing",
        "website": "https://www.boeing.com/",
        "team_name": "boeing",
        "company_id": 124397,
        "short_name": "Boeing"
      },
      "formatted_employee_count": "156,000"
    }
  ]
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `cg[]` | object[] |  |
| `cg[].founded` | string |  |
| `cg[].revenue` | integer |  |
| `cg[].ownership` | string |  |
| `cg[].ceo_detail` | object |  |
| `cg[].headquarters` | object |  |
| `cg[].revenue_range` | string |  |
| `cg[].total_funding` | integer |  |
| `cg[].employee_count` | integer |  |
| `cg[].employee_range` | string |  |
| `cg[].industry_sectors` | string[] |  |
| `cg[].formatted_funding` | string |  |
| `cg[].formatted_revenue` | string |  |
| `cg[].company_basic_info` | object |  |
| `cg[].formatted_employee_count` | string |  |

## Owler Search API

Search for companies on Owler by name or keyword, returning a list of matching companies with slugs, logos, domains, and profile URLs.

`GET | POST https://api.piloterr.com/v2/owler/search` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | The company name or keyword to search for on Owler. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/owler/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "tesla",
    },
    timeout=60,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type OwlerSearch = {
  id?: string;
  url?: string;
  logo?: string;
  name?: string;
  slug?: string;
  domain?: string;
}[];

const params = new URLSearchParams({
  "query": "tesla",
});

const response = await fetch(`https://api.piloterr.com/v2/owler/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as OwlerSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/owler/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "tesla"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
[
  {
    "id": "15254378",
    "url": "https://www.owler.com/company/nortonlifelock",
    "logo": "https://s3.amazonaws.com/owler-image/logo/nortonlifelock_owler_20211115_162831_original.png",
    "name": "NortonLifeLock, Inc.",
    "slug": "nortonlifelock",
    "domain": "nortonlifelock.com"
  },
  {
    "id": "145118",
    "url": "https://www.owler.com/company/norton-rose-fulbright",
    "logo": "https://s3.amazonaws.com/owler-image/logo/norton-rose-fulbright_owler_20241206_222941_original.jpg",
    "name": "Norton Rose Fulbright, LLP",
    "slug": "norton-rose-fulbright",
    "domain": "nortonrosefulbright.com"
  }
]
```

### Response fields

| Field | Type | Description |
|---|---|---|

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Company Info | Search |
|---|---|---|
| cURL (Bash) | [`company-info`](examples/curl/company_info.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`company-info`](examples/php/company_info.php) | [`search`](examples/php/search.php) |
| C# | [`company-info`](examples/csharp/CompanyInfo.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`company-info`](examples/go/company_info.go) | [`search`](examples/go/search.go) |
| Java | [`company-info`](examples/java/CompanyInfoExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`company-info`](examples/ruby/company_info.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`company-info`](examples/nodejs/company_info.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Lead generation** — Build target account lists from public company data.
- **CRM enrichment** — Keep firmographics, websites and descriptions up to date.
- **Market mapping** — Map competitors and segments across countries and industries.
- **Investment research** — Screen companies, funding and signals for deal flow.
- **Job market analytics** — Track hiring, roles and salaries where available.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Owler plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Owler?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/owler/company/info` **1**, `/v2/owler/search` **1**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Owler API return?

Each endpoint returns structured JSON: **Owler Company Info** — `cg[]`, `cg[].founded`, `cg[].revenue`, `cg[].ownership`, `cg[].ceo_detail`, `cg[].headquarters`; **Owler Search** — see the reference.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Related scrapers

- [Company Database Scraper API](https://github.com/piloterr/company-data-api)
- [Crunchbase Scraper API](https://github.com/piloterr/crunchbase-scraper)
- [Europages Scraper API](https://github.com/piloterr/europages-scraper)
- [Fiverr Scraper API](https://github.com/piloterr/fiverr-scraper)
- [g2-scraper](https://github.com/piloterr/g2-scraper)
- [github-scraper](https://github.com/piloterr/github-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Owler is a trademark of its respective owner. This repository is not affiliated with or endorsed by Owler. Examples are released under the MIT license.</sub>
