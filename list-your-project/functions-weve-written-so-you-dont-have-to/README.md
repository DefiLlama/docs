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

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/velaro/index.js" %}
Example adapter
{% endembed %}

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/aevo-xyz/index.js" %}
Example adapter
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

{% hint style="info" %}
DefiLlama uses a wide variety of sources to price tokens, such as CoinGecko and chain calls to price exotic tokens such as Curve and uniswap LPs. If you find that a token is missing and it's not getting priced in your adapter, just let us know in our discord!
{% endhint %}

To count the TVL of LP token balances, the positions must be unwrapped into their underlying tokens.

```
const { sumTokens2 } = require('../helper/unwrapLPs');

const balances = {};
...
return sumTokens2({ balances, tokensAndOwners: [...], api, resolveLP: true })
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/drachma/index.js" %}
Example Unwrap Uni V2 Adapter
{% endembed %}

### Getting Block Heights

For lesser known EVM chains sometimes the block height wont be available in the third parameter passed to the adapter's TVL function. In this case you can use getBlock to fetch the block height.

```
const { getBlock } = require('../helper/http');
block = await getBlock(timestamp, chain, chainBlocks);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/atlendis/index.js" %}
Example Get Block Adapter
{% endembed %}
