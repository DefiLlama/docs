# How to write an SDK adapter

You should start by forking the [DefiLlama/DefiLlama-Adapters](https://github.com/DefiLlama/DefiLlama-Adapters) repository on github. Afterwards, just create a new folder on \`/projects\` and write your adapter there.

### Adapters 101

At it's core, an adapter is just some code that takes in a UNIX timestamp and an Ethereum block height and returns the balances of assets locked in a protocol, including all the decimals (that is, the way it's stored on chain).

For example, the following adapter would return a TVL that increases by 1 WBTC and 3 BAT every block:

```javascript
const BigNumber = require("bignumber.js")

const wbtcContract = "0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599"
const batContract = "0x0d8775f648430679a709e98d2b0cb6250d2887ef"

async function tvl(timestamp, block) {
  // WBTC has a 8 decimals
  const wbtcBalance = BigNumber(block).times(1e8)
  // BAT has 18 decimals
  const batBalance = BigNumber(block).times(3).times(1e18)
  
  // toFixed(0) just converts the numbers into strings
  return { 
    [wbtcContract]: wbtcBalance.toFixed(0),
    [batContract]: batBalance.toFixed(0)
  };
}

module.exports = {
  ethereum:{
    tvl,
  },
  tvl,
};
```

If we were to evaluate it to get the TVL at block height 12 we would then get the following output:

```javascript
{
  '0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599': '1200000000',
  '0x0d8775f648430679a709e98d2b0cb6250d2887ef': '36000000000000000000'
}
```

Which our SDK would then convert into their USD equivalent and sum to obtain total TVL.

So, all in all, adapters just do that, take in a time and return a list of assets locked at that moment, specified by the address of their token, and their respective balances as strings and with all the decimals.

### Basic adapter

Now let's move on to a real adapter, here's the one we use for PoolTogether:

{% code title="projects/pooltogether/index.js" %}
```javascript
const sdk = require("@defillama/sdk");
const { request, gql } = require("graphql-request");
const abi = require('./abi.json')

const graphUrl =
  'https://api.thegraph.com/subgraphs/name/pooltogether/pooltogether-v3_1_0';
const graphQuery = gql`
query GET_POOLS($block: Int) {
  prizePools(
    block: { number: $block }
  ) {
    id
    underlyingCollateralSymbol
    underlyingCollateralToken
    compoundPrizePool{
      cToken
    }
  }
}
`;

async function tvl(timestamp, block) {
  let balances = {};

  const { prizePools } = await request(
    graphUrl,
    graphQuery,
    {
      block,
    }
  );
  const lockedTokens = await sdk.api.abi.multiCall({
    abi: abi['accountedBalance'],
    calls: prizePools.map(pool => ({
      target: pool.id
    })),
    block
  });
  lockedTokens.output.forEach((call, index) => {
    const underlyingToken = prizePools[index].underlyingCollateralToken;
    const underlyingTokenBalance = call.output;
    sdk.util.sumSingleBalance(balances, underlyingToken, underlyingTokenBalance);
  })
  return balances
}


module.exports = {
  ethereum:{
    tvl,
  },
  tvl
}
```
{% endcode %}

What we are doing here is getting all their pools from their subgraph, then making an Ethereum call to each of them to get how many tokens are locked in each and then just returning each of these tokens along with the balances we got.

On top of all this, it's also important to notice that all calls are done through an SDK, this allows our adapters to be compatible with DeFi Pulse's and makes it so you only have to write them once and can submit them to multiple pages.

### SDK Reference

Check it out [here](https://github.com/ConcourseOpen/DeFi-Pulse-Adapters/blob/master/docs/sdk.md).

### Examples

Check the following page for several examples of adapters along with input/outputs:

{% content-ref url="../speedrun.md" %}
[speedrun.md](../speedrun.md)
{% endcontent-ref %}

### Testing

Once you are done writing it you can verify that it returns the correct value by running the following code:

```bash
$ npm install
# Replace with your adapter's name
$ node test.js projects/pooltogether/index.js 
```

### Submit 🎉

Just submit a PR to [the adapter repository on Github](https://github.com/DefiLlama/DefiLlama-Adapters)! 
