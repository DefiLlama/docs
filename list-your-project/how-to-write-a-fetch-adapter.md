# How to write a fetch adapter

Fetch adapters export a function, called `fetch`, which returns a project's total TVL (in USD) as a number. The following basic adapter would just always return a TVL of 100$:

```javascript
async function fetch() {
  return 100;
}

module.exports = {
  fetch
}
```

Fetch adapters only allow us to get the TVL at the current time, so it's impossible to fill old values on a protocol's TVL chart or recompute them, thus leading to charts that look jumpy. To solve this we introduced SDK adapters, which allow us to retrieve a protocol's TVL at any point in time.

{% hint style="info" %}
Fetch adapters can only be used for projects on non-EVM chains. Where possible, [SDK adapters](how-to-write-an-sdk-adapter.md) are preferred to fetch adapters because on-chain calls are more transparent.
{% endhint %}

## Examples

{% code title="projects/balancer.js" %}
```javascript
const retry = require('async-retry')
const { GraphQLClient, gql } = require('graphql-request')

async function fetch() {
  var endpoint ='https://api.thegraph.com/subgraphs/name/balancer-labs/balancer';
  var graphQLClient = new GraphQLClient(endpoint)

  var query = gql`
  {
    balancers(first: 5) {
      totalLiquidity,
      totalSwapVolume
    }
  }
  `;
  const results = await retry(async bail => await graphQLClient.request(query))
  return parseFloat(results.balancers[0].totalLiquidity);
}

module.exports = {
  fetch
}
```
{% endcode %}

{% code title="projects/ren.js" %}
```javascript
const retry = require('async-retry')
const axios = require("axios");
const BigNumber = require("bignumber.js");

async function fetch() {
  let price_feed = await retry(async bail => await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd&include_market_cap=true&include_24hr_vol=true&include_24hr_change=true'))
  let response = await retry(async bail => await axios.get('https://api.etherscan.io/api?module=stats&action=tokensupply&contractaddress=0xeb4c2781e4eba804ce9a9803c67d0893436bb27d&apikey=H6NGIGG7N74TUH8K2X31J1KB65HFBH2E82'))
  let tvl = new BigNumber(response.data.result).div(10 ** 8).toFixed(2);
  return (tvl * price_feed.data.bitcoin.usd);
}

module.exports = {
  fetch
}
```
{% endcode %}

{% code title="projects/thorchain.js" %}
```javascript
const retry = require('async-retry')
const axios = require("axios");
const BigNumber = require("bignumber.js");

async function fetch() {
  var price_feed = await retry(async bail => await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=thorchain&vs_currencies=usd&include_market_cap=true&include_24hr_vol=true&include_24hr_change=true'))

  var res = await retry(async bail => await axios.get('https://chaosnet-midgard.bepswap.com/v1/network'))
  var tvl = await new BigNumber((parseFloat(res.data.totalStaked) * 2) + parseFloat(res.data.bondMetrics.totalActiveBond) + parseFloat(res.data.bondMetrics.totalStandbyBond)).div(10 ** 8).toFixed(2);
  tvl = tvl * price_feed.data.thorchain.usd;
  return tvl;
}

module.exports = {
  fetch
}
```
{% endcode %}
