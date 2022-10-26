# How to write a fee / revenue adapter

## Adapter examples

### Using existing volume adapters
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


### Custom query from api
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

### Custom data from subgraph
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