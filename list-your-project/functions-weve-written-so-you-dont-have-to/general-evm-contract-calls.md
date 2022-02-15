# General EVM contract calls

### Balance Calls

erc20.balanceOf can be used to get token balances

```
  return {
    [tokenAddress]: (
      await sdk.api.erc20.balanceOf({
        tokenAddress,
        tokenHolderAddress,
        block,
        chain
      })
    ).output,
  };
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/twindex/index.js" %}
Example ERC20 Token Balance Adapter
{% endembed %}

eth.getBalance is for getting gas token balances (can be used on chains that aren't Ethereum)

```
const balance_okt = (
  await sdk.api.eth.getBalance({
    target,
    block,
    chain: 'okexchain'
  })
).output;
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/typhoon.js" %}
Example Gas Token Balance Adapter
{% endembed %}

### Custom Contract Calls

Contract calls are the most common ways of recording TVL.

```
const sdk = require('@defillama/sdk');

const response = (await sdk.api.abi.call({<params>})).output
```

Often after a single contract call you'll also want to add the balance to your balances object, which can be easily done with sumSingleBalance.

```
await sdk.util.sumSingleBalance(balances, tokenAddress, balanceOfToken);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/manarium/index.js" %}

When you have lots of calls to functions with the same ABI, it's easier to use multiCall and sumMultiBalanceOf.&#x20;

```
const tokensInacBTC = [
  '0x2260fac5e5542a773aa44fbcfedf7c193bc2c599',
  '0xEB4C2781e4ebA804CE9a9803C67d0893436bB27D'
];

const acBTCTokenHolder = '0x73FddFb941c11d16C827169Bb94aCC227841C396';

const underlyingacBTC = await sdk.api.abi.multiCall({
  calls: tokensInacBTC.map(token => ({
    target: token,
    params: [acBTCTokenHolder]
  })),
  abi: 'erc20:balanceOf',
  block,
});

sdk.util.sumMultiBalanceOf(balances, underlyingacBTC);
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/acoconut/index.js" %}
Example MultiCall Adapter
{% endembed %}

