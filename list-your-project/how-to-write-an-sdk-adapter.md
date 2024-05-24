# How to write an SDK adapter

### Adapters 101

An adapter is just some code that takes in a UNIX timestamp and chain block heights, and returns the balances of assets locked in a protocol, including all the decimals (that is, the way it's stored on chain). Our SDK will convert all raw asset balances into their USD equivalent and sum to obtain total TVL, so you need minimal processing inside the adapter.

### Basic adapter

Below, you can see the one we use for Mint Club on Binance Smart Chain (BSC). Let's walk through it to get a better understanding of how it works.

{% code title="projects/mint-club/index.js" %}
```javascript
const MINT_TOKEN_CONTRACT = '0x1f3Af095CDa17d63cad238358837321e95FC5915';
const MINT_CLUB_BOND_CONTRACT = '0x8BBac0C7583Cc146244a18863E708bFFbbF19975';

async function tvl(api) {
  const collateralBalance = await api.call({
    abi: 'erc20:balanceOf',
    target: MINT_TOKEN_CONTRACT,
    params: [MINT_CLUB_BOND_CONTRACT],
  });

  api.add(MINT_TOKEN_CONTRACT, collateralBalance)
}

module.exports = {
  methodology: 'counts the number of MINT tokens in the Club Bonding contract.',
  start: 1000235,
  bsc: {
    tvl,
  }
}; 
```
{% endcode %}

The adapter consists of 3 main sections. First, any dependencies we want to use. Next, an async function containing the code for calculating TVL (where the bulk of the code usually is). Finally, the module exports.

#### Line 4 - Input Parameter:

It is an injected `sdk.ChainApi` object with which you can interact with a given chain through `call/multiCall/batchCall` method based on your need, also stores tvl balances

{% hint style="info" %}
DefiLlama uses a wide variety of sources to price tokens, such as CoinGecko and chain calls to price exotic tokens such as Curve and uniswap LPs. If you find that a token is missing and it's not getting priced in your adapter, just let us know in our discord!
{% endhint %}

#### Line 5 - On Chain Function Calls

Here we use the SDK to get the erc20 token balance of a contract, but this api.call() function can be used to call all sorts of contract functions. Parameters used:

* abi - Because we have used a common erc20 function for Mint Club, we're able to use a string for the 'abi' parameter. However for other contract functions you will need to pass a [JSON ABI (or human-readable abi string)](https://www.quicknode.com/guides/solidity/what-is-an-abi) (can find these on etherscan).
* target - The target address of the contract call.
* params - Optional, must take the same amount of params expected by the on-chain contract function.

#### Line 11 - Adding Data To The Balances Object

In the SDK we have utilities to add data to the balances dictionary. api.add() takes 2 parameters:

1. The token key you want to add to. We will transform the MINT token address so that we can fetch the CoinGecko price.
2. The balance we want to add. (NB: If we were using a CoinGecko ID in position 2, we'd need to divide collateralBalance by 10 \*\* \<MINT token decimals> to convert the raw balance to a real balance).

Note: if you want to add balances of multiple tokens at the same time, you can do so by running `api.addTokens(tokens, balances)`

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/2cbe2f0c40848c3cf3d683dfb62d8a5077939ba4/projects/CthulhuFinance/index.js#L27" %}
Example add tokens
{% endembed %}

#### Line 23 - Module Exports

The module exports must be constructed correctly, and use the correct keys, so that the DefiLlama UI can show your data. Nest chain TVL (and separate types of TVL like staking, pool2 etc) inside the chain key (eg 'bsc', 'ethereum').

Please also let us know:

* timetravel (bool \[default: true]) - if we can backfill data with your adapter. Most SDK adapters will allow this, but not all. For example, if you fetch a list of live contracts from an API before querying data on-chain, timetravel should be 'false'.
* misrepresentedTokens (bool \[default: false]) - if you have used token substitutions at any point in the adapter this should be 'true'.
* methodology (string) - this is a small description that will explain to DefiLlama users how the adapter works out your protocol's TVL.
* start (number - optional) - the earliest timestamp the adapter will work at.
* hallmarks (array of \[number, string]) - set of events that greatly affected protocol TVL and we display on the chart ([example](https://defillama.com/protocol/uniswap)).

### Testing

Once you are done writing it you can verify that it returns the correct value by running the following code:

```bash
$ npm install
# if you want debug logs
$ export LLAMA_DEBUG_MODE="true" 
# Replace with your adapter's name
$ node test.js projects/mint-club/index.js 
```

If the adapter runs successfully, the console will show you a breakdown of your project's TVL in USD. If it all looks accurate, you're ready to submit.

### Submit 🎉

Just submit a PR to [the adapter repository on Github](https://github.com/DefiLlama/DefiLlama-Adapters)!
