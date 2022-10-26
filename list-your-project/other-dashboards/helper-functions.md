# Helper functions

#### Custom backfill

For situations where the fetch function can only return `totalVolume` and can return in based on a timestamp you can use the `customBackfill` function that can be found in `volumes/helper/customBackfill`.

#### Obtaining data from subgraphs

If the data is available in a subgraph and it follows a structure similar to uniswap, you can use the folling helper functions to easily query it. More docs about it to be added soon but for now... take a look at other implementations!

* `getChainVolume` from `volumes/helper/getUniSubgraphVolume`
* `getStartTimestamp` from `volumes/helper/getStartTimestamp`
* `getDexChainFees` from `fees/helpers/getUniSubgraphFees`
* `getDexChainBreakdownFees` from `fees/helpers/getUniSubgraphFees`
* `getStartTimestamp` from `fees/helpers/getStartTimestamp` or simply use the function `univ2Adapter` to create an adapter with a couple of lines of code. See an example [here](https://github.com/DefiLlama/adapters/blob/master/volumes/apeswap/index.ts).

#### Exact day timestamp

Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the function `startOfTodayTimestamp` to get the timestamp of the specific day. For example passing `1663718399 (2022-09-20T23:59:59.000Z)` timestamp will return `1663632000000 (2022-09-20T00:00:00.000Z)`
