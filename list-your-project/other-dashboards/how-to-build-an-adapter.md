# How to build an adapter

And adapter is just some code that:

1. Collects data on a protocol by calling some endpoints or making some blockchain calls
2. Computes a response and returns it.

That's just a typescript file that exports an async function that is given start and end timestamps (and block numbers) and returns an object with metrics for the time range between those two timestamps.

A really simplified version of an adapter could be the following lines:

```typescript
import { FetchOptions, SimpleAdapter } from "../adapters/types";
import { CHAIN } from "../helpers/chains";
import { queryDune } from "../helpers/dune";

const fetch: any = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  const value = (await queryDune("3521814", {
    start: options.startTimestamp,
    end: options.endTimestamp,
    receiver: '9yMwSPk9mrXSN7yDHUuZurAh1sjbJsfpUqjZ7SvVtdco'
  }));
  dailyFees.add('So11111111111111111111111111111111111111112', value[0].fee_token_amount);

  return { dailyFees, dailyRevenue: dailyFees }
}

export default {
  version: 2,
  adapter: {
    [CHAIN.SOLANA]: {
      fetch: fetch,
      start: 0,
    },
  },
};
```

The above adapter is for a protocol that is deployed on `solana`, and will return the daily fees and revenue for the time period between startTimestamp and endTimestamp. Depends on which dashboard the adapter is aiming for, it should return different attributes. We call those attributes dimensions. In the next page you will find a detailed list of all supported dimensions.

### BaseAdapter

In the above example, the object under the key `solana` is what we call a `BaseAdapter` and it contains all the methods and information needed to list, collect data and enable your project.

The attribute `fetch` is the most important part of the BaseAdapter but not the only attribute needed to list your project. Other important attributes needed for an optimal listing are:

* `fetch`: Promise that returns different dimensions of a protocol. The dimensions returned depends on which adapter you would like to list your project (e.g. \`dailyVolume\`  for the [dexs dashboard](https://defillama.com/dexs)).
* `start`: The earliest timestamp we can pass to the fetch function. This tells our servers how far can we get historical data.
* `runAtCurrTime`: Boolean that flags if the adapter takes into account the timestamp and block passed to the fetch function (`runAtCurrTime: false`) or if it can only return the latest data, for example there are some adapters that are only able to return the volume of the past 24h from the moment the adapter is executed (`runAtCurrTime: true`).
* `meta`: Object that contains metadata of the BaseAdapter. The possible attributes are:
  * `methodology`: Object that describes the methodology used to calculate the different dimensions returned. Find an example [here](https://github.com/DefiLlama/dimension-adapters/blob/c03a108f546707ab75ef727d33cef053348757dd/protocols/pancakeswap/index.ts#L43).
  * `hallmarks`: Set of events that greatly affected protocol data and we display on the chart ([example](https://defillama.com/protocol/uniswap)).

Besides these, you'll find the `version` key, this should always be `2` except in the case where the adapter can only run for time ranges that start at 0:00 and end at 0:00 of the next day, in that case version must be `1`. Typically this happens with APIs that return volume for each day and don't allow more precise time periods.

### Some other examples

You can use getLogs() to get all event logs in the timeframe between the two timestamps automatically, in the following case we use it to get the logs emitted by the friendtech contract on every trade, which is then used to calculate fees and revenue (in this case fees is twice the value of revenue, since on every trade, 50% of the fee goes to the protocol and 50% to the room creator).

```typescript
import { Adapter, FetchV2, } from "../adapters/types";
import { CHAIN } from "../helpers/chains";

export default {
  adapter: {
    [CHAIN.BASE]: {
      fetch: (async ({ getLogs, createBalances, }) => {
        const dailyFees = createBalances()
        const dailyRevenue = createBalances()
        const logs = await getLogs({
            target: "0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4",
            eventAbi: 'event Trade(address trader, address subject, bool isBuy, uint256 shareAmount, uint256 ethAmount, uint256 protocolEthAmount, uint256 subjectEthAmount, uint256 supply)'
        })
        logs.map((e: any) => {
            dailyFees.addGasToken(e.protocolEthAmount * 2)
            dailyRevenue.addGasToken(e.protocolEthAmount)
        })
        return { dailyFees, dailyRevenue, }
      }) as FetchV2,
      start: 1691539200,
    },
  },
  version: 2,
} as Adapter
```

Since most adapters follow a similar structure, we've written some helper functions that you might help you writing your adapter. You will find information about these functions in the next pages.
