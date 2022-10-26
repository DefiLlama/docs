# How to write a volume adapter

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