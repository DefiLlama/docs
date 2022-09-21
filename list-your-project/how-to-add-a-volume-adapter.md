# How to write a volume adapter

## Volume adapter

An adapter is a typescript file that exports an async function that, when run, returns the daily volume of a protocol at a given time, split by chain and protocol version (eg: uniswap v2, v3...).

The given time will be the timestamp of the last second of the day of which we want the volume, so the adapter should return the volume of the 24h before of the given timestamp. All times will be UTC time. As an example, if passed `1663718399` timestamp should return the volume that happened during `20/09/2022 UTC`.

A volume adapter could be either a `SimpleVolumeAdapter` or a `BreakdownVolumeAdapter`. A `BreakdownVolumeAdapter` adapter is a set of `SimpleVolumeAdapter` and is used to define adapters for a protocol that has multiple versions (I.e. Uniswap v1, v2, v3).


```typescript
type SimpleVolumeAdapter = {
  volume: Adapter
};

type BreakdownAdapter = {
  [version: string]: Adapter
};

type Adapter = {
  [chain: string]: {
    start: () => Promise<number>
    fetch: Fetch;
    runAtCurrTime?: boolean;
    customBackfill?: Fetch;
  }
};
```



`Adapter` properties:
- `fetch`: Promise that returns the daily and total volume given a timestamp or a block number. Returns an object with the `dailyVolume` and/or `totalVolume`, the `timestamp` of the volume and the `block` of the volume. If the adapter only returns totalVolume, our servers will calculate the `s` based on previous day `totalVolume`.
```typescript
type FetchResult = {
  block?: number;
  dailyVolume?: string;
  totalVolume: string;
  timestamp: number;
};
```
- `start`: Promise that returns a timestamp indicating the start of data availability. To indicate how far needs to be refilled.
- `customBackfill`: Promise that returns the daily and total volume given a timestamp or a block number. Used when logic to get historical volume values is different than the `fetch` promise.

### Helper functions
###### Custom backfill
For situations where the fetch function can only return `totalVolume` and can return in based on a timestamp you can use the `customBackfill` function that can be found in `volumes/helper/customBackfill`.
###### Obtaining data from subgraphs
If the data is available in a subgraph and it follows a structure similar to uniswap, you can use the folling helper functions to easily query it. More docs about it to be added soon but for now... take a look at other implementations/the code!
- `getChainVolume` from `volumes/helper/getUniSubgraphVolume`
- `getStartTimestamp` from `volumes/helper/getStartTimestamp`
###### Exact day timestamp
Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the function `startOfTodayTimestamp` to get the timestamp of the specific day. For example passing `1663718399 (2022-09-20T23:59:59.000Z)` timestamp will return `1663632000000 (2022-09-20T00:00:00.000Z)`



### Adapter example
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

After adding your volume adapter, you can test if it works using the following command

`npm run test-dex <adapter-name> <timestamp>`

Examples:
```
npm run test-dex uniswap
npm run test-dex uniswap 1662110960016
```