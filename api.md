# API

### Authentication

Interacting with the API that powers our site doesn't require any authentication, the API is completely open and anyone can start using it without having to contact us.

{% api-method method="get" host="https://api.llama.fi" path="/protocols" %}
{% api-method-summary %}
Get All Protocols
{% endapi-method-summary %}

{% api-method-description %}
Returns basic information on all listed protocols, their current TVL and the changes to it in the last hour/day/week.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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

{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.llama.fi" path="/protocol/:slug" %}
{% api-method-summary %}
Get Protocol
{% endapi-method-summary %}

{% api-method-description %}
Returns historical data on the TVL of a protocol along with some basic data on it. The fields \`tokensInUsd\` and \`tokens\` are only available for some protocols.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="slug" type="string" required=true %}
Slug of the protocol to get \(eg: uniswap, yearn-finance...\).  
This can be obtained from the /protocols endpoint
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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

{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.llama.fi" path="/charts" %}
{% api-method-summary %}
Get historical values of total TVL
{% endapi-method-summary %}

{% api-method-description %}
Returns historical values of the total sum of TVLs from all listed protocols.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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

{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.llama.fi" path="/tvl/:slug" %}
{% api-method-summary %}
Get protocol TVL
{% endapi-method-summary %}

{% api-method-description %}
Mainly meant to make life easier for users that import data to spreadsheets
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="slug" type="string" required=false %}
Slug of the protocol to get \(eg: uniswap, yearn-finance...\).
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Directly returns the TVL as a number
{% endapi-method-response-example-description %}

```
58605464.543818
```

{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}
