# Functions we've written so you don't have to

### Exports Helpers

Exporting empty TVL - if your project has filtered TVL only, here's an easy way to export core TVL as empty.

```
module.exports = {
    bsc: {
        tvl: () => ({}),
        staking
    }
};
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/arcx.js" %}
Example adapter
{% endembed %}

### General API Calls

Using the retry function can be useful to make graphQL and REST API requests more reliable, by repeating the call a number of times when an unsuccessful response is received.

```
const retry = require('./helper/retry');

const pools = (
  await retry(async (bail) => await graphQLClient.request(query))
).pools.map((pool) => pool.id);

const programList = await retry(
  async (bail) =>
    await axios.get("https://api.btcpx.io/api/v1/prxy-staking/list/0/10")
);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/wbtc.js" %}
Example Adapter
{% endembed %}

### Solana Helpers

getTokenBalance is used for getting a solana account's balance of a particular token.

```
const { getTokenBalance } = require("../helper/solana");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/solend/index.js" %}
Example Solana Adapter
{% endembed %}

### Transforming Tokens That Aren't On CoinGecko

We value tokens through CoinGecko. If you export token balances for addresses that aren't listed on CoinGecko, you'll need to transform the addresses to something that is on CoinGecko. Usually the best way to do this is through the getChainTransform helper.

```
const { getChainTransform } = require("../helper/portedTokens");

const chainTransform = await getChainTransform(chain);
const transformedAssetId = await chainTransform(assetId);
sdk.util.sumSingleBalance(balances, transformedAssetId, balance);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/railgun/index.js" %}
Example Address Transform
{% endembed %}



To count the TVL of LP token balances, the positions must be unwrapped into their underlying tokens.

```
const { unwrapUniswapLPs } = require('./helper/unwrapLPs');

const balances = {};
const transform = await transformBscAddress();

await unwrapUniswapLPs(
  balances,
  [
    {balance: balance1, token: tokenAddress1}, 
    {balance: balance2, token: tokenAddress2}
  ],
  chainBlocks['bsc'],
  'bsc',
  transform
);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/tomb/index.js" %}
Example Unwrap Uni V2 Adapter
{% endembed %}

If you have a list of contracts that hold a range of different assets, it could be easiest to use sumTokensAndLPsSharedOwners. This will check all contracts for all tokens, and try to unwrap all uni v2 LP tokens.

```
const { sumTokensAndLPsSharedOwners } = require("./helper/unwrapLPs");
const balances = {};

await sumTokensAndLPsSharedOwners(
    balances,
    [
      [token1, isLP],
      [token2, isLP],
    ],
    [contracts],
    block,
    chain,
    transform
  );
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/euphoria/index.js" %}
Example Sum Tokens and LPs Adapter
{% endembed %}

### Getting Block Heights

For lesser known EVM chains sometimes the block height wont be available in the third parameter passed to the adapter's TVL function. In this case you can use getBlock to fetch the block height.

```
const { getBlock } = require('./helper/getBlock');
block = await getBlock(timestamp, chain, chainBlocks);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/bitBTC.js" %}
Example Get Block Adapter
{% endembed %}

