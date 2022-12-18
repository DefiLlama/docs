# How to build an adapter

And adapter is just some code that:

1. Collects data on a protocol by calling some endpoints or making some blockchain calls
2. Computes a response based on the dashboard the adapter is aiming for and returns it.

That's just a typescript file that exports an async function that given a timestamp and/or block numbers returns an object with some information.

A really simplified version of an adapter could be the follwing:

```typescript
export default {
  adapter: {
    ethereum: {
      fetch: (timestamp: number) => {
        const { dayVolume, totalVolume } = await getVolumeStats();
        return {
          timestamp,
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

That adapter is for a protocol that is deployed on `ethereum` and `optimism`, and will return the daily and total volume of the protocol, in this case to be added to the `dexs` or `aggregators` dashboard. Depends on which dashboard the adapter is aiming for, it shuold return different attributes. To know which attributes the adapter should return find in this the corresponding `FetchResult` type [file](https://github.com/DefiLlama/adapters/blob/master/adapters/types.ts) or check the below list.

### Fetch return attributes and BaseAdapter attributes

**Fetch return attributes by dashboard**

* Dexs: `dailyVolume: string` and `totalVolume: string`.
* Aggregators: `dailyVolume: string` and `totalVolume: string`.
* Fees: `totalFees: string`, `dailyFees: string`, `totalRevenue: string`, `dailyRevenue: string`
* Incentives: `tokens: Object<string>`
* Options: `totalPremiumVolume: string`, `totalNotionalVolume: string`, `dailyPremiumVolume: string`, `dailyNotionalVolume: string`

In the above example, the object under the key `ethereum` it's what we call a `BaseAdapter` and it contains all the methods and information needed to list collect the data of your project of a specific chain. The attribute `fetch` is the most important but not the only attribute necessary to do that. Here you will find a list of all the necessary attributes for an optimal listing:

* `fetch`: Promise that returns the FetchResult object based on the type of adapter given a timestamp and a block number.
* `start`: Promise that returns a timestamp indicating the earliest timestamp we can pass to the fetch function. This tells our servers if we can get historical data and how far we can do that.
* `runAtCurrTime`: Boolean that indicates if the adapter takes into account the timestamp and block passed to the fetch function (`runAtCurrTime: false`) or if it's only able to return data of the execution time, for example there are some adapters that are only able to return the volume of the past 24h from the moment the adapter is executed (`runAtCurrTime: true`).
* `meta`: Object that contains metadata of the BaseAdapter. The posible attributes are:
  * `metodology`: Important to add. In this attribute should be some lines describing how the adapter calculates the results returned.
  * `hallmarks`: Set of events that greatly affected protocol data and we display on the chart ([example](https://defillama.com/protocol/uniswap)).

### Protocol with multiple versions deployed in the same chain

The example we have seen before is a `SimpleAdapter` of a protocol deployed on different chains, but what if we have multiple versions of our protocol deployed in the same chain? For this situations you can add a `BreakdownAdapter` to properly have an `BaseAdapter` for each version and chain. Here's a real example:

Find the full code of Uniswap breakdown adapter [here](https://github.com/DefiLlama/adapters/tree/master/volumes/uniswap).

```typescript
const adapter: BreakdownAdapter = {
  breakdown: {
    v1: {
      [CHAIN.ETHEREUM]: {
        fetch: v1Graph(CHAIN.ETHEREUM),
        start: async () => 1541203200,
      },
    },
    v2: {
      [CHAIN.ETHEREUM]: {
        fetch: v2Graph(CHAIN.ETHEREUM),
        start: getStartTimestamp({
          endpoints: v2Endpoints,
          chain: CHAIN.ETHEREUM,
        }),
      },
    },
    v3: Object.keys(v3Endpoints).reduce((acc, chain) => {
      acc[chain] = {
        fetch: v3Graphs(chain),
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

As you could see in the above example the adapter is using the `BreakdownAdapter` type to make sure the types of the exported adapter is correct. Please use the appropiate types for a smooth listing.

Since most adapters follow a similar structure, we've written some helper functions that you might help you writting your adapter. More information about the functions here.
