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


### Token balance queries

if you have a known set of tokens and contract addresses, there are few ways to fetch and export it as tvl using `sumTokensExport` 

if single contract and multiple tokens

```
const { sumTokensExport } = require("./helper/unwrapLPs");

module.exports = {
    fantom: {
        tvl: sumTokensExport({ 
          chain: 'fantom', 
          owner: '0x..., 
          tokens: [ '0x...',...   ],
        }),
    }
};
```

if there are multiple contracts to look up:


```
const { sumTokensExport } = require("./helper/unwrapLPs");

module.exports = {
    fantom: {
        tvl: sumTokensExport({ 
          chain: 'fantom', 
          owners: ['0x...', '0x...', ...],
          tokens: [ '0x...',...   ],
        }),
    }
};
```

if all contracts dont share same set of tokens:

```
const { sumTokensExport } = require("./helper/unwrapLPs");

module.exports = {
    fantom: {
        tvl: sumTokensExport({ 
          chain: 'fantom', 
          tokensAndOwners: [
            // [tokenAddress, ownerContractAddress]
            ['0x...', '0x...'],
            ['0x...', '0x...'],
          ],
        }),
    }
};
```

if any of these tokens are LP tokens, set `resolveLP: true` to resolve them into underlying tokens 

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/svivel/index.js" %}
Example adapter
{% endembed %}

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/puli/index.js" %}
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
const { transformBalances } = require("../helper/portedTokens");

const transformedAssetId = await chainTransform(assetId);
sdk.util.sumSingleBalance(balances, assetId, balance);
return transformBalances(chain, balances)
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/aries-markets/index.js" %}
Example Address Transform
{% endembed %}



To count the TVL of LP token balances, the positions must be unwrapped into their underlying tokens.

```
const { sumTokens2 } = require('./helper/unwrapLPs');

const balances = {};
const transform = await transformBscAddress();
...
return sumTokens2({ balances, tokensAndOwners: [...], chain, block, resolveLP: true })
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/drachma/index.js" %}
Example Unwrap Uni V2 Adapter
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

