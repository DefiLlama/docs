# API

## Authentication

Interacting with the API that powers our site doesn't require any authentication, the API is completely open and anyone can start using it without having to contact us.

{% swagger baseUrl="https://api.llama.fi" path="/protocols" method="get" summary="Get all protocols" %}
{% swagger-description %}
Returns basic information on all listed protocols, their current TVL and the changes to it in the last hour/day/week.
{% endswagger-description %}

{% swagger-parameter in="path" name="" type="string" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
[
  {
    "id": "1",
    "name": "Uniswap",
    "address": "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984",
    "symbol": "UNI",
    "url": "https:\/\/info.uniswap.org\/",
    "description": "A fully decentralized protocol for automated liquidity provision on Ethereum.\r\n",
    "chain": "Ethereum",
    "logo": null,
    "audits": "2",
    "audit_note": null,
    "gecko_id": "uniswap",
    "cmcId": "7083",
    "category": "Dexes",
    "chains": ["Ethereum"],
    "module": "uniswap/index.js",
    "twitter": "Uniswap",
    "audit_links": [
      "https:\/\/github.com\/Uniswap\/uniswap-v3-core\/tree\/main\/audits",
      "https:\/\/github.com\/Uniswap\/uniswap-v3-periphery\/tree\/main\/audits",
      "https:\/\/github.com\/ConsenSys\/Uniswap-audit-report-2018-12"
    ],
    "slug": "uniswap",
    "tvl": 4294270703.8317223,
    "chainTvls": {
      "Ethereum": 4294270703.8317223
    },
    "change_1h": 2.4355323149076753,
    "change_1d": 4.591169165988049,
    "change_7d": -11.268195007383326
  },
  ...
]
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.llama.fi" path="/protocol/:slug" method="get" summary="Get Protocol" %}
{% swagger-description %}
Returns historical data on the TVL of a protocol along with some basic data on it. The fields `tokensInUsd` and `tokens` are only available for some protocols.
{% endswagger-description %}

{% swagger-parameter in="path" name="slug" type="string" %}
Slug of the protocol to get (eg: uniswap, yearn-finance...).

\


This can be obtained from the /protocols endpoint
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "id": "1",
  "name": "Uniswap",
  "address": "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984",
  "symbol": "UNI",
  "url": "https:\/\/info.uniswap.org\/",
  "description": "A fully decentralized protocol for automated liquidity provision on Ethereum.\r\n",
  "chain": "Ethereum",
  "logo": null,
  "audits": "2",
  "audit_note": null,
  "gecko_id": "uniswap",
  "cmcId": "7083",
  "category": "Dexes",
  "chains": ["Ethereum"],
  "module": "uniswap/index.js",
  "twitter": "Uniswap",
  "audit_links": [
    "https:\/\/github.com\/Uniswap\/uniswap-v3-core\/tree\/main\/audits",
    "https:\/\/github.com\/Uniswap\/uniswap-v3-periphery\/tree\/main\/audits",
    "https:\/\/github.com\/ConsenSys\/Uniswap-audit-report-2018-12"
  ],
  "chainTvls": {
    "Ethereum":
    {
      "tvl": [
        {
          "date": 1588716000,
          "totalLiquidityUSD": 0.9892225217628695
        },
        {
          "date": 1588802400,
          "totalLiquidityUSD": 3.206759871028627
        },
        ...
        {
          "date": 1624406400,
          "tokens": {
            "USDT": 4112596483.605813
            }
          },
        {
          "date": 1624492800,
          "tokens": {
            "USDT": 4237678349.747973
            }
          }
      ]
    }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.llama.fi" path="/charts" method="get" summary="Get charts" %}
{% swagger-description %}
Returns historical values of the total sum of TVLs from all listed protocols.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```javascript
[
  {
    "date": 1608940800,
    "totalLiquidityUSD": "19601373602.55"
  },
  {
    "date": 1609027200,
    "totalLiquidityUSD": "20296613822.14",
    "dailyVolumeUSD": 695240219.5900002
  },
  {
    "date": 1609113600,
    "totalLiquidityUSD": "20351393354.76",
    "dailyVolumeUSD": 54779532.61999893
  },
  ...
]
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.llama.fi" path="/charts/:chain" method="get" summary="Get chain chart" %}
{% swagger-description %}
Returns the total sum of TVLs for projects on a specific chain
{% endswagger-description %}

{% swagger-parameter in="path" name="chain" type="string" %}
Slug of the chain you wish to obtain the aggregated TVLs from. You can get these slugs from the /protocols endpoint
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.llama.fi" path="/tvl/:slug" method="get" summary="Get protocol TVL" %}
{% swagger-description %}
Mainly meant to make life easier for users that import data to spreadsheets
{% endswagger-description %}

{% swagger-parameter in="path" name="slug" type="string" %}
Slug of the protocol to get (eg: uniswap, yearn-finance...).
{% endswagger-parameter %}

{% swagger-response status="200" description="Directly returns the TVL as a number" %}
```
58605464.543818
```
{% endswagger-response %}
{% endswagger %}
