# General EVM contract calls

### Balance Calls

use `sumTokens2` method from helper to return token balances

For example, you have a vault with USDC and ETH tokens in it in arbitrum

```javascript


const { sumTokensExport, sumTokens } = require('./helper/unwrapLPs')

const owner = '0x...' // vault address
const tokens = [
  '0xff970a61a04b1ca14834a43f5de4533ebddb5cc8', // USDC
  '0x0000000000000000000000000000000000000000', // ETH
]

module.exports = {
  arbitrum: {
    tvl: sumTokensExport({ owner, tokens })
  }
}

// or 

module.exports = {
  arbitrum: {
    tvl: async (_, _1, _2, { api }) => {
      return sumTokens2({ owner, tokens, api, })
    }
  }
}
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/twindex/index.js" %}
Example ERC20 Token Balance Adapter
{% endembed %}


### Custom Contract Calls

Contract calls are the most common ways of recording TVL.

```javascript
const response = await api.call({<params>})
```

Often after a single contract call you'll also want to add the balance to your balances object, which can be easily done with sumSingleBalance.

```
await sdk.util.sumSingleBalance(balances, tokenAddress, balanceOfToken);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/manarium/index.js" %}

When you have lots of calls to functions with the same ABI, it's easier to use multiCall and sumMultiBalanceOf.&#x20;

```javascript
const tokensInacBTC = [
  '0x2260fac5e5542a773aa44fbcfedf7c193bc2c599',
  '0xEB4C2781e4ebA804CE9a9803C67d0893436bB27D'
];

const acBTCTokenHolder = '0x73FddFb941c11d16C827169Bb94aCC227841C396';

const underlyingacBTC = await api.multiCall({
  calls: tokensInacBTC.map(token => ({
    target: token,
    params: [acBTCTokenHolder]
  })),
  abi: 'erc20:balanceOf',
  withMetadata: true,
});

sdk.util.sumMultiBalanceOf(balances, underlyingacBTC);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/acoconut/index.js" %}
Example MultiCall Adapter
{% endembed %}

