# Fees adapter



## How to write a fee / revenue adapter

### Adapter examples

#### Using existing volume adapters

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

#### Custom query from api

```typescript
import { Adapter, FetchV2 } from "../adapters/types";
import { getTimestampAtStartOfPreviousDayUTC } from "../utils/date";
import fetchURL from "../utils/fetchURL";
import { CHAIN } from "../helpers/chains";

const feeEndpoint = "https://api-osmosis.imperator.co/fees/v1/total/historical"

interface IChartItem {
  time: string
  fees_spent: number
}

const fetch: FetchV2 = async ({ endTimestamp }) => {
  const dayTimestamp = getTimestampAtStartOfPreviousDayUTC(endTimestamp)
  const historicalFees: IChartItem[] = (await fetchURL(feeEndpoint))

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

const adapter: Adapter = {
  version: 2,
  adapter: {
    [CHAIN.COSMOS]: {
      fetch,
      runAtCurrTime: true,
      start: 1665964800,
    },
  }
}

export default adapter;
```

#### Custom data from subgraph

```typescript
import { Adapter } from "../adapters/types";
import { ARBITRUM, AVAX } from "../helpers/chains";
import { request, gql } from "graphql-request";
import type { ChainEndpoints, FetchV2 } from "../adapters/types"
import { getTimestampAtStartOfDayUTC } from "../utils/date";

const endpoints = {
  [ARBITRUM]: "https://subgraph.satsuma-prod.com/3b2ced13c8d9/gmx/gmx-arbitrum-stats/api",
  [AVAX]: "https://subgraph.satsuma-prod.com/3b2ced13c8d9/gmx/gmx-avalanche-stats/api"
}

const methodology = {
  Fees: "Fees from open/close position (0.1%), swap (0.2% to 0.8%), mint and burn (based on tokens balance in the pool) and borrow fee ((assets borrowed)/(total assets in pool)*0.01%)",
  UserFees: "Fees from open/close position (0.1%), swap (0.2% to 0.8%) and borrow fee ((assets borrowed)/(total assets in pool)*0.01%)",
  HoldersRevenue: "30% of all collected fees goes to GMX stakers",
  SupplySideRevenue: "70% of all collected fees goes to GLP holders",
  Revenue: "Revenue is 30% of all collected fees, which goes to GMX stakers",
  ProtocolRevenue: "Treasury has no revenue"
}

const graphs = (graphUrls: ChainEndpoints) => {
    const fetch: FetchV2 = async ({ chain, startTimestamp }) => {
      const todaysTimestamp = getTimestampAtStartOfDayUTC(startTimestamp)
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
      const userFee = parseInt(graphRes.feeStat.marginAndLiquidation) + parseInt(graphRes.feeStat.swap)
      const finalUserFee = (userFee / 1e30);

      return {
        dailyFees: finalDailyFee.toString(),
        dailyUserFees: finalUserFee.toString(),
        dailyRevenue: (finalDailyFee * 0.3).toString(),
        dailyProtocolRevenue: "0",
        totalProtocolRevenue: "0",
        dailyHoldersRevenue: (finalDailyFee * 0.3).toString(),
        dailySupplySideRevenue: (finalDailyFee * 0.7).toString(),
      };
    };
    return fetch 
};


const adapter: Adapter = {
  version: 2,
  adapter: {
    [ARBITRUM]: {
      fetch: graphs(endpoints),
      start: 1630468800,
      meta: {
        methodology
      }
    },
    [AVAX]: {
      fetch: graphs(endpoints),
      start: 1641445200,
      meta: {
        methodology
      }
    },
  }
}

export default adapter;

```
