# How to write a fee / revenue adapter

## Adapter Properties

An adapter is a typescript file that exports an async function that, when run, returns the daily volume of a protocol at a given time, split by chain and protocol version (eg: uniswap v2, v3...).

Similar to volume adapters, the given time will be the timestamp of the last second of the day of which we want the data, so the adapter should return the data 24h before the given timestamp. All times will be UTC time. As an example, if passed `1663718399` timestamp should return the volume that happened during `20/09/2022 UTC`.

A fee adapter could be either a `BaseFeeAdapter` or a `FeeBreakdownAdapter`. A `FeeBreakdownAdapter` adapter is a set of `BaseFeeAdapter` and is used to define adapters for a protocol that has multiple versions (I.e. Uniswap v1, v2, v3). 

```typescript
export type BaseFeeAdapter = {
  fees: BaseAdapter;
  adapterType?: string;
};

export type BreakdownAdapter = {
  [x: string]: BaseAdapter;
};

export type FeeBreakdownAdapter = {
  breakdown: BreakdownAdapter;
  adapterType?: string;
};

export type BaseAdapter = {
  [chain: string]: {
    start: number | (() => Promise<number>)
    fetch: Fetch;
    runAtCurrTime?: boolean;
    customBackfill?: Fetch;
  };
};
```

`BaseAdapter` properties:
- `fetch`: Promise that returns the daily and total fees / revenue given a timestamp or a block number. Returns an object with the `dailyFees`, `dailyRevenue`, `totalFees`, `totalRevenue`, the `timestamp` of the fees and the `block`. If the adapter only returns `dailyFees` or `dailyRevenue`, our servers will calculate the total based on cumulative data (currently, any field that is left blank is default set to `0`. This will change in the future when the historical backfill is completed.)

```typescript
export type FetchResult = {
  block?: number;
  dailyFees?: string;
  totalFees: string;
  dailyRevenue?: string;
  totalRevenue: string;
  timestamp: number;
};
```
- `start`: Promise that returns a timestamp indicating the start of data availability. To indicate how far needs to be refilled.
- `customBackfill`: Promise that returns the daily and total fees given a timestamp or a block number. Used when logic to get historical volume values is different than the `fetch` promise.


### Helper functions
###### Obtaining data from existing volume adapters
If the data is already available as a volume adapter, you can use the following helper functions to easily query the volume adapter and the fee portions directly.
- `getDexChainFees` from `fees/helpers/getUniSubgraphFees`
- `getStartTimestamp` from `fees/helpers/getStartTimestamp`

###### Exact day timestamp
Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the function `getTimestampAtStartOfDayUTC` and `getTimestampAtStartOfPreviousDayUTC` to get the timestamp of a specific day or previous day.

###### Custom backfill
- TBD, work in progress



### Adapter examples
```typescript
import customBackfill from "../../helper/customBackfill";
import { DEFAULT_TOTAL_VOLUME_FACTORY, DEFAULT_TOTAL_VOLUME_FIELD, getChainVolume } from "../../helper/getUniSubgraphVolume";
import { CHAIN } from "../../helper/chains";
import type { ChainEndpoints, SimpleVolumeAdapter } from "../../dexVolume.type";
import type { Chain } from "@defillama/sdk/build/general";

// Subgraphs endpoints
const endpoints: ChainEndpoints = {
  [CHAIN.ETHEREUM]: "https://api.thegraph.com/subgraphs/name/dex",
  [CHAIN.POLYGON]: "https://api.thegraph.com/subgraphs/name/dex-polygon",
  [CHAIN.ARBITRUM]: "https://api.thegraph.com/subgraphs/name/dex-arbitrum",
};

// Fetch function to query the subgraphs
const graphs = getChainVolume({
  graphUrls: endpoints,
  totalVolume: {
    factory: DEFAULT_TOTAL_VOLUME_FACTORY,
    field: DEFAULT_TOTAL_VOLUME_FIELD,
  },
  dailyVolume: {
    factory: DAILY_VOLUME_FACTORY,
    field: "D_VOL_FIELD",
  },
  hasDailyVolume: false,
});

const adapter: SimpleVolumeAdapter = {
  volume: Object.keys(endpoints).reduce((acc, chain) => {
    return {
      ...acc,
      [chain]: {
        fetch: graphs(chain as Chain),
        start: async () => 1570665600,
        customBackfill: customBackfill(CHAIN.ETHEREUM, graphs),
      }
    }
  }, {} as SimpleVolumeAdapter['volume'])
};

export default adapter;

```

## Testing

After adding your fee adapter, you can test if it works using the following command

`npm run test-fees <adapter-name> <timestamp>`

Examples:
```
npm run test-fees uniswap
npm run test-fees uniswap 1662110960016
```