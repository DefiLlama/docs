# API

### Authentication

Interacting with the API that powers our site doesn't require any authentication, the API is  completely open and anyone can start using it without having to contact us.

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
    "tvl": "5050848297.03",
    "change_1h": 0,
    "change_1d": -0.7085764798524287,
    "change_7d": 12.845304274849878
  },
  ...
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.llama.fi" path="/protocol/:name" %}
{% api-method-summary %}
Get Protocol
{% endapi-method-summary %}

{% api-method-description %}
Returns historical data on the TVL of a protocol along with some basic data on it. The fields \`tokensInUsd\` and \`tokens\` are only available for some protocols.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="name" type="string" required=true %}
ID of the protocol to get \(eg: uniswap, WBTC...\).  
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
  "chains": ["Ethereum"],
  "logo": "https://icons.llama.fi/eq0vIOD1_400x400.jpg",
  "audits": "2",
  "audit_note": null,
  "gecko_id": "uniswap",
  "cmcId": "7083",
  "category": "Dexes",
  "tvl": [
    {
      "date": 1611360000,
      "totalLiquidityUSD": "3225225307.35"
    },
    {
      "date": 1611446400,
      "totalLiquidityUSD": "3166055013.42"
    },
    ...
  ],
  "tokensInUsd": [
    {
      "date":1616713200,
      "tokens":{
        "DPI":129070.08340457824,
        "WETH":89700.8000047377
      }
    },
    ...
  ],
  "tokens":[
    {
      "date":1616713200,
      "tokens":{
        "DPI":341.1262424986834
        "WETH":55.61780506546786
      }
    },
    ...
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

{% api-method method="get" host="https://api.llama.fi" path="/tvl/:name" %}
{% api-method-summary %}
Get protocol TVL
{% endapi-method-summary %}

{% api-method-description %}
Mainly meant to make life easier for users that import data to spreadsheets
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="name" type="string" required=false %}
ID of the protocol \(eg: uniswap\)
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

