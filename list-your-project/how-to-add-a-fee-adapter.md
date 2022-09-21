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
- `getDexChainBreakdownFees` from `fees/helpers/getUniSubgraphFees`
- `getStartTimestamp` from `fees/helpers/getStartTimestamp`

###### Exact day timestamp
Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the function `getTimestampAtStartOfDayUTC` and `getTimestampAtStartOfPreviousDayUTC` to get the timestamp of a specific day or previous day.

###### Custom backfill
- TBD, work in progress


### Adapter examples

## Using existing volume adapters
```typescript
import { getDexChainFees } from "../helpers/getUniSubgraphFees";
import volumeAdapter from "@defillama/adapters/volumes/adapters/pancakeswap";
import { FeeAdapter, BaseAdapter } from "../utils/adapters.type";

const TOTAL_FEES = 0.0025;
const PROTOCOL_FEES = 0.0003;

const feeAdapter: BaseAdapter = getDexChainFees({
  totalFees: TOTAL_FEES,
  protocolFees: PROTOCOL_FEES,
  volumeAdapter
});

const adapter: FeeAdapter = {
    fees: feeAdapter
};

export default adapter;
```


## Custom query from api
```typescript
import { FeeAdapter } from "../utils/adapters.type";
import { getTimestampAtStartOfPreviousDayUTC } from "../utils/date";
import { fetchURL } from "@defillama/adapters/projects/helper/utils";
import axios from "axios"

const feeEndpoint = "https://api-osmosis.imperator.co/fees/v1/total/historical"

interface IChartItem {
  time: string
  fees_spent: number
}

const fetch = async (timestamp: number) => {
  const dayTimestamp = getTimestampAtStartOfPreviousDayUTC(timestamp)
  const historicalFees: IChartItem[] = (await fetchURL(feeEndpoint))?.data

  const totalFee = historicalFees
    .filter(feeItem => (new Date(feeItem.time).getTime() / 1000) <= dayTimestamp)
    .reduce((acc, { fees_spent }) => acc + fees_spent, 0)

  const dailyFee = historicalFees
    .find(dayItem => (new Date(dayItem.time).getTime() / 1000) === dayTimestamp)?.fees_spent

  return {
    timestamp: dayTimestamp,
    totalFees: `${totalFee}`,
    dailyFees: dailyFee ? `${dailyFee}` : undefined,
    totalRevenue: "0",
    dailyRevenue: "0",
  };
};

const getStartTimestamp = async () => {
  const historicalVolume: IChartItem[] = (await axios.get(feeEndpoint))?.data
  return (new Date(historicalVolume[0].time).getTime()) / 1000
}

const adapter: FeeAdapter = {
  fees: {
    cosmos: {
      fetch,
      runAtCurrTime: true,
      start: getStartTimestamp,
    },
  }
}

export default adapter;
```

## Custom data from subgraph
```typescript
import { FeeAdapter } from "../utils/adapters.type";
import { ARBITRUM, AVAX } from "../helpers/chains";
import { request, gql } from "graphql-request";
import { IGraphUrls } from "../helpers/graphs.type";
import { Chain } from "../utils/constants";
import { getTimestampAtStartOfDayUTC } from "../utils/date";

const endpoints = {
  [ARBITRUM]: "https://api.thegraph.com/subgraphs/name/gmx-io/gmx-stats",
  [AVAX]: "https://api.thegraph.com/subgraphs/name/gmx-io/gmx-avalanche-stats"
}

const graphs = (graphUrls: IGraphUrls) => {
  return (chain: Chain) => {
    return async (timestamp: number) => {
      const todaysTimestamp = getTimestampAtStartOfDayUTC(timestamp)
      const searchTimestamp = chain == "arbitrum" ? todaysTimestamp : todaysTimestamp + ":daily"

      const graphQuery = gql
      `{
        feeStat(id: "${searchTimestamp}") {
          mint
          burn
          marginAndLiquidation
          swap
        }
      }`;

      const graphRes = await request(graphUrls[chain], graphQuery);

      const dailyFee = parseInt(graphRes.feeStat.mint) + parseInt(graphRes.feeStat.burn) + parseInt(graphRes.feeStat.marginAndLiquidation) + parseInt(graphRes.feeStat.swap)
      const finalDailyFee = (dailyFee / 1e30);

      return {
        timestamp,
        totalFees: "0",
        dailyFees: finalDailyFee.toString(),
        totalRevenue: "0",
        dailyRevenue: (finalDailyFee * 0.3).toString(),
      };
    };
  };
};


const adapter: FeeAdapter = {
  fees: {
    [ARBITRUM]: {
        fetch: graphs(endpoints)(ARBITRUM),
        start: 1630468800,
    },
    [AVAX]: {
        fetch: graphs(endpoints)(AVAX),
        start: 1641445200,
    },
  }
}

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