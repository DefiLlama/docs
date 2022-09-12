# How to write an SDK adapter

### Adapters 101

An adapter is just some code that takes in a UNIX timestamp and chain block heights, and returns the balances of assets locked in a protocol, including all the decimals (that is, the way it's stored on chain). Our SDK will convert all raw asset balances into their USD equivalent and sum to obtain total TVL, so you need minimal processing inside the adapter.

### Basic adapter

Below, you can see the one we use for Mint Club on Binance Smart Chain (BSC). Let's walk through it to get a better understanding of how it works.

{% code title="projects/mint-club/index.js" %}
```javascript
const sdk = require('@defillama/sdk');
const { transformBscAddress } = require('../helper/portedTokens');
const MINT_TOKEN_CONTRACT = '0x1f3Af095CDa17d63cad238358837321e95FC5915';
const MINT_CLUB_BOND_CONTRACT = '0x8BBac0C7583Cc146244a18863E708bFFbbF19975';

async function tvl(timestamp, block, chainBlocks) {
  const balances = {};
  const transform = await transformBscAddress();

  const collateralBalance = (await sdk.api.abi.call({
    abi: 'erc20:balanceOf',
    chain: 'bsc',
    target: MINT_TOKEN_CONTRACT,
    params: [MINT_CLUB_BOND_CONTRACT],
    block: chainBlocks['bsc'],
  })).output;

  await sdk.util.sumSingleBalance(balances, transform(MINT_TOKEN_CONTRACT), collateralBalance)

  return balances;
}

module.exports = {
  timetravel: true,
  misrepresentedTokens: false,
  methodology: 'counts the number of MINT tokens in the Club Bonding contract.',
  start: 1000235,
  bsc: {
    tvl,
  }
}; 
```
{% endcode %}

The adapter consists of 3 main sections. First, any dependencies we want to use. Next, an async function containing the code for calculating TVL (where the bulk of the code usually is). Finally, the module exports.

#### Line 6 - Input Parameters:

1. The first param taken by the function (line 6) will be a timestamp. In your testing this will be the current timestamp, but when we back fill chart data for your protocol, past timestamps will also be input.
2. Next is the Ethereum mainnet block height corresponding the timestamp in the first param.
3. Last is an optional object containing block heights for other EVM chains. This is not needed if your project is only on Ethereum mainnet. In this example it was required because Mint Club is on BSC.

#### Line 7 - Initialising The Balances Object:

SDK adapters always export balances objects, which is a dictionary where all the keys are either token addresses or Coingecko token IDs. On this line we just initialise the balances object to be empty.

If a token balance has an address key, the DefiLlama SDK will manage any raw to real amount conversion for you (so you don't need to worry about erc20 decimals). If a token balance has a Coingecko ID key, you will need to process the decimals and use a real token amount in the balances object. Example:\\

```
{ 
    'polygon:0xc2132d05d31c914a87c6611c10748aeb04b58e8f' : 456245893460345345234,
    '0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2' : 985678356234523456,
    'wrapped-bitcoin': 4.002342
}
```

#### Line 8 - Address Transform Function:

Many assets have been deployed on multiple chains. It's hard for CoinGecko to keep up with asset addresses for every chain, so sometimes we have to transform the addresses to ones known by CoinGecko. We have managed most cases for you in the transform\_\_Address() functions, found in projects/helper/portedTokens.js (notice line 2).

![](<../.gitbook/assets/Screenshot 2022-02-08 at 16.11.38.png>)

{% hint style="info" %}
DefiLlama uses a wide variety of sources to price tokens, such as CoinGecko and chain calls to price exotic tokens such as Curve and uniswap LPs. If you find that a token is missing and it's not getting priced in your adapter, just let us know in our discord!
{% endhint %}

#### Line 10 - On Chain Function Calls

Here we use the SDK to get the erc20 token balance of a contract, but this sdk.api.abi.call() function can be used to call all sorts of contract functions. Parameters used:

* abi - Because we have used a common erc20 function for Mint Club, we're able to use a string for the 'abi' parameter. However for other contract functions you will need to pass a [JSON ABI](https://www.quicknode.com/guides/solidity/what-is-an-abi) (can find these on etherscan).
* chain - An optional parameter (defaults to Ethereum) which determines which chain the contract call is made on.
* target - The target address of the contract call.
* params - Optional, must take the same amount of params expected by the on-chain contract function.
* block - The block height that the contract call will be executed on. This should always correspond to the chain parameter, and in this case we use the chainBlocks object to get the bsc block height (remember, the second param on line 6 is the Ethereum mainnet block, not BSCs, which we are interested in for Mint Club).

#### Line 18 - Adding Data To The Balances Object

In the SDK we have utilities to add data to the balances dictionary. sdk.util.sumSingleBalance() takes 3 parameters:

1. The object you want to add token balances to.
2. The token key you want to add to. We will transform the MINT token address so that we can fetch the CoinGecko price.
3. The balance we want to add. (NB: If we were using a CoinGecko ID in position 2, we'd need to divide collateralBalance by 10 \*\* \<MINT token decimals> to convert the raw balance to a real balance).

#### Line 23 - Module Exports

The module exports must be constructed correctly, and use the correct keys, so that the DefiLlama UI can show your data. Nest chain TVL (and separate types of TVL like staking, pool2 etc) inside the chain key (eg 'bsc', 'ethereum').

Please also let us know:

* timetravel (bool) - if we can backfill data with your adapter. Most SDK adapters will allow this, but not all. For example, if you fetch a list of live contracts from an API before querying data on-chain, timetravel should be 'false'.
* misrepresentedTokens (bool) - if you have used token substitutions at any point in the adapter this should be 'true'.
* methodology (string) - this is a small description that will explain to DefiLlama users how the adapter works out your protocol's TVL.
* start (number) - the earliest timestamp the adapter will work at.
* hallmarks (array of \[number, string]) - set of events that greatly affected protocol TVL and we display on the chart ([example](https://defillama.com/protocol/uniswap)).

### Testing

Once you are done writing it you can verify that it returns the correct value by running the following code:

```bash
$ npm install
# Replace with your adapter's name
$ node test.js projects/mint-club/index.js 
```

If the adapter runs successfully, the console will show you a breakdown of your project's TVL in USD. If it all looks accurate, you're ready to submit.

### Submit 🎉

Just submit a PR to [the adapter repository on Github](https://github.com/DefiLlama/DefiLlama-Adapters)!
