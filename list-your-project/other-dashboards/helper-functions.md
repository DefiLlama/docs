# Helper functions

#### Custom backfill

For situations where the fetch function can only return `totalVolume` and can return in based on a timestamp you can use the `customBackfill` function that can be found in `volumes/helper/customBackfill`.

#### Obtaining data from subgraphs

If the data is available in a subgraph and it follows a structure similar to uniswap, you can use the folling helper functions to easily query it. More docs about it to be added soon but for now... take a look at other implementations!

* `getChainVolume` from `helpers/getUniSubgraphVolume`
* `getDexChainFees` from `helpers/getUniSubgraphFees`
* `getDexChainBreakdownFees` from `helpers/getUniSubgraphFees`

Or simply use the function `univ2Adapter` to create an adapter with a couple of lines of code. See an example [here](https://github.com/DefiLlama/dimension-adapters/blob/master/dexs/thena/index.ts).

#### Tokens received

If fees or revenue are directly sent to an address, you can calculate the fees by calculating the tokens received by that address in the following way (this is the adapter for basecamp, a pumpfun fork on base):

```typescript
import { FetchOptions, SimpleAdapter } from "../../adapters/types";
import { CHAIN } from "../../helpers/chains";
import { addTokensReceived } from '../../helpers/token';

const fetch: any = async (options: FetchOptions) => {
  const dailyFees = await addTokensReceived({
    options,
    tokens: ["0x4200000000000000000000000000000000000006"],
    targets: ["0xbcb4a982d3c2786e69a0fdc0f0c4f2db1a04e875"]
  })

  return { dailyFees, dailyRevenue: dailyFees }
}

export default {
  version: 2,
  adapter: {
    [CHAIN.BASE]: {
      fetch: fetch,
      start: 0,
    },
  },
} as SimpleAdapter;
```

#### Exact day timestamp

Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the parameter `startOfDay` to get the timestamp of the specific day. For example passing `1663718399 (2022-09-20T23:59:59.000Z)` timestamp will return `1663632000000 (2022-09-20T00:00:00.000Z)`
