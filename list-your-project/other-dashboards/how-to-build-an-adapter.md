# How to build an adapter

And adapter is just some code that:

1. Collects data on a protocol by calling some endpoints or making some blockchain calls
2. Computes a response and returns it.

That's just a typescript file that exports an async function that given a [timestamp](important-considerations.md) and/or block numbers returns an object with some information.

A really simplified version of an adapter could be the following lines:

```typescript
export default {
  adapter: {
    ethereum: {
      fetch: () => {
        const { dayVolume, totalVolume } = await getVolumeStats();
        return {
          dailyVolume: dayVolume,
          totalVolume: totalVolume,
        };
      },
    },
    optimism: {
        ...
    }
  },
};
```

The above adapter is for a protocol that is deployed on `ethereum` and `optimism`, and will return the daily and total volume of the protocol. Depends on which dashboard the adapter is aiming for, it should return different attributes. We call those attributes dimensions. In the next page you will find a detailed list of all supported dimensions.

### BaseAdapter

In the above example, the object under the key `ethereum` is what we call a `BaseAdapter` and it contains all the methods and information needed to list, collect data and enable your project.

The attribute `fetch` is the most important part of the BaseAdapter but not the only attribute needed to list your project. Other important attributes needed for an optimal listing are:

* `fetch`: Promise that returns different dimensions of a protocol (important-considerations.md). The dimensions returned depends on which adapter you would like to list your project (e.g. \`dailyVolume\` and \`totalVolume\` for the [dexs dashboard](https://defillama.com/dexs)).
* `start`: Promise that returns a timestamp pointing to the earliest timestamp we can pass to the fetch function. This tells our servers how far can we get historical data.
* `runAtCurrTime`: Boolean that flags if the adapter takes into account the timestamp and block passed to the fetch function (`runAtCurrTime: false`) or if it can only return the latest data, for example there are some adapters that are only able to return the volume of the past 24h from the moment the adapter is executed (`runAtCurrTime: true`).
* `meta`: Object that contains metadata of the BaseAdapter. The possible attributes are:
  * `methodology`: Object that describes the methodology used to calculate the different dimensions returned. Find an example [here](https://github.com/DefiLlama/dimension-adapters/blob/c03a108f546707ab75ef727d33cef053348757dd/protocols/pancakeswap/index.ts#L43).
  * `hallmarks`: Set of events that greatly affected protocol data and we display on the chart ([example](https://defillama.com/protocol/uniswap)).

### Protocol with multiple versions deployed in the same chain

The example we have seen before is a `SimpleAdapter` of a protocol deployed on different chains, but...

* What if we have **multiple versions of our protocol deployed in the same chain**?
* What if our protocol **has different products?** For example a DEX can provide spot trading as well as derivatives trading to their users.

For this situations you can create a `BreakdownAdapter` to have a `BaseAdapter` for each version and chain. Here's a real example:

Find the full code of Uniswap breakdown adapter [here](https://github.com/DefiLlama/dimension-adapters/blob/master/protocols/uniswap/index.ts).

```typescript
const adapter: BreakdownAdapter = {
  version: 2,
  breakdown: {
    v1: {
      [CHAIN.ETHEREUM]: {
        fetch: async ({ endTimestamp, getEndBlock }) => v1Graph(CHAIN.ETHEREUM)(endTimestamp, getEndBlock),
        start: async () => 1541203200,
      },
    },
    v2: {
      [CHAIN.ETHEREUM]: {
        fetch: async ({ endTimestamp, getEndBlock }) => v2Graph(CHAIN.ETHEREUM)(endTimestamp, getEndBlock),
        start: getStartTimestamp({
          endpoints: v2Endpoints,
          chain: CHAIN.ETHEREUM,
        }),
      },
    },
    v3: Object.keys(v3Endpoints).reduce((acc, chain) => {
      acc[chain] = {
        fetch: async ({ endTimestamp, getEndBlock }) => v3Graphs(chain)(endTimestamp, getEndBlock),
        start: getStartTimestamp({
          endpoints: v3Endpoints,
          chain: chain,
          volumeField: VOLUME_USD,
        })
      }
      return acc
    }, {} as BaseAdapter)
  }
}
```

Keys of the breakdown object should be the name of the protocol version. E.g.: "v1" and "v2" or "swap" and "derivatives" or "elastic" and "classic".

By using the above structure (`BreakdownAdapter`) your protocol will be listed as an aggregated of all versions and will include a subrow for each version (see [this ranking](https://defillama.com/fees) for some examples).

As you can see in the above example the adapter is using the `BreakdownAdapter` type to make sure the types of the exported adapter is correct. Please use the appropriate types for a smooth listing.

Since most adapters follow a similar structure, we've written some helper functions that you might help you writing your adapter. You will find information about these functions in the next pages.
