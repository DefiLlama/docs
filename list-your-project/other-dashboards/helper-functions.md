---
hidden: true
---

# Helper functions

{% hint style="info" %}
**Note (Updated: 2025-05-01):** This page might contain outdated information. For the most up-to-date documentation, please visit the main [Building Dimension Adapters](./) guide.
{% endhint %}

## Helper Functions

This document provides detailed information about the various helper functions available to simplify the creation of adapters. Each section includes example implementations and links to real adapters using these functions.

### Token Tracking Helpers

#### addTokensReceived

Tracks ERC20 token transfers received by specified addresses. Supports filtering by sender/receiver and custom token transformations. It attempts to use an indexer first for performance, with a fallback to log processing.

```typescript
import { addTokensReceived } from '../../helpers/token';

const fetch: any = async (options: FetchOptions) => {
  const dailyFees = await addTokensReceived({
    options,
    tokens: ["0x4200000000000000000000000000000000000006"], // WETH on Base
    targets: ["0xbcb4a982d3c2786e69a0fdc0f0c4f2db1a04e875"] // Treasury
  })

  return { dailyFees, dailyRevenue: dailyFees }
}
```

[Example Implementation - Synthetix](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/synthetix.ts)

#### addGasTokensReceived

Tracks native token transfers (like ETH) received by specified multisig addresses. This helper is particularly useful for protocols that collect fees in the chain's native token.

```typescript
import { addGasTokensReceived } from '../../helpers/token';

const fetch = async (options: FetchOptions) => {
  const dailyFees = await addGasTokensReceived({
    options,
    multisigs: ["0x123...abc", "0x456...def"] // Treasury multisig addresses
  });
  
  return { dailyFees, dailyRevenue: dailyFees };
}
```

#### getETHReceived

Tracks native token transfers on EVM chains, supporting multiple chains through Allium database queries.

```typescript
import { getETHReceived } from '../../helpers/token';

const fetch = async (options: FetchOptions) => {
  const balances = options.createBalances();
  await getETHReceived({
    options,
    balances,
    target: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045" // Treasury address
  });
  
  return { dailyFees: balances, dailyRevenue: balances };
}
```

[Example Implementation - DexTools](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/dextools.ts)

#### getSolanaReceived

Fetches token transfers to specified Solana addresses within a given time range, with ability to exclude specific sender addresses or transaction signers.

```typescript
import { getSolanaReceived } from '../../helpers/token';

const fetch = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  await getSolanaReceived({
    options,
    balances: dailyFees,
    target: "9yMwSPk9mrXSN7yDHUuZurAh1sjbJsfpUqjZ7SvVtdco", // Treasury
    blacklists: ["3xxxx..."] // Optional senders to exclude
  });
  
  return { dailyFees, dailyRevenue: dailyFees };
}
```

[Example Implementation - Jito](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/jito.ts)

### EVM Data Helpers

#### getLogs

Retrieves event logs from blockchains based on specified filters like target addresses, event signatures, and topics. Essential for tracking on-chain events.

```typescript
const fetch = async ({ getLogs, createBalances }) => {
  const dailyFees = createBalances();
  const logs = await getLogs({
    target: "0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4",
    eventAbi: 'event Trade(address trader, address subject, bool isBuy, uint256 shareAmount, uint256 ethAmount, uint256 protocolEthAmount, uint256 subjectEthAmount, uint256 supply)'
  });
  
  logs.forEach(log => {
    dailyFees.addGasToken(log.protocolEthAmount);
  });
  
  return { dailyFees };
}
```

[Example Implementation - Ostium](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/ostium/index.ts)

#### queryIndexer

Executes queries against DeFiLlama's indexers to retrieve token transfers, events, and other blockchain data.

```typescript
import { queryIndexer } from '../../helpers/indexer';

const fetch = async (options: FetchOptions) => {
  const transfers = await queryIndexer({
    chain: options.chain,
    fromTimestamp: options.startTimestamp, 
    toTimestamp: options.endTimestamp,
    filter: { to: "0x123...abc" }
  });
  
  const dailyFees = options.createBalances();
  transfers.forEach(t => dailyFees.add(t.token, t.value));
  
  return { dailyFees };
}
```

[Example Implementation - Sudoswap V2](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/sudoswap-v2.ts)

### Protocol-Specific Helpers

#### compoundV2Export

Creates an adapter for Compound V2-like protocols, taking config parameters and returning an object that tracks fees, revenue, and distribution among holders and suppliers.

```typescript
import { compoundV2Export } from '../helpers/compound';

export default compoundV2Export({
  reserveFactor: 0.1, // 10% of interest goes to protocol
  markets: {
    [CHAIN.ETHEREUM]: {
      comptroller: '0x123...abc',
      // ... other market parameters
    }
  }
});
```

[Example Implementation - Strike](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/strike/index.ts)

#### uniV2Exports / getUniV2LogAdapter

Generates adapter configurations for Uniswap V2-style DEXes across multiple chains.

```typescript
import { uniV2Exports } from '../helpers/uniswap';

export default uniV2Exports({
  [CHAIN.BSC]: {
    factories: [
      '0x123...abc' // Factory address
    ],
    fees: {
      type: 'fixed',
      feesPercentage: 0.3 // 0.3% fee
    }
  },
  // Other chains
});
```

[Example Implementation - Nile Exchange V1](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/nile-exchange-v1/index.ts)

[Example Implementation - Hydrometer](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/hydrometer/index.ts)

[Example Implementation - ABCDEFX](https://github.com/DefiLlama/dimension-adapters/blob/master/dexs/abcdefx/index.ts)

#### uniV3Exports

Creates adapters for Uniswap V3-style DEXes, supporting variable fees and multiple pools.

```typescript
import { uniV3Exports } from '../helpers/uniswap';

export default uniV3Exports({
  [CHAIN.ETHEREUM]: {
    factory: '0x1F98431c8aD98523631AE4a59f267346ea31F984',
    // Optional custom fee handling or additional configurations
  }
});
```

[Example Implementation - 2thick](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/2thick.ts)

### Query Engine Helpers

#### queryDuneSql

Executes SQL queries against the Dune Analytics database.

```typescript
import { queryDuneSql } from '../helpers/dune';

const fetch = async (options: FetchOptions) => {
  const results = await queryDuneSql(
    options,
    `SELECT SUM(fee_amount) as fees
    FROM ethereum.transactions
    WHERE to = '0x123...abc'
    AND block_time BETWEEN to_timestamp(${options.startTimestamp}) AND to_timestamp(${options.endTimestamp})`
  );
  
  const dailyFees = options.createBalances();
  dailyFees.addGasToken(results[0].fees);
  
  return { dailyFees };
}
```

[Example Implementation - Pumpswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/pump-swap/index.ts)

#### queryAllium

Queries the Allium database for blockchain data across multiple chains.

```typescript
import { queryAllium } from '../helpers/allium';

const fetch = async (options: FetchOptions) => {
  const result = await queryAllium(`
    SELECT SUM(value) as revenue
    FROM ethereum.transactions
    WHERE to_address = '0x123...abc'
    AND block_timestamp BETWEEN TO_TIMESTAMP_NTZ(${options.startTimestamp}) AND TO_TIMESTAMP_NTZ(${options.endTimestamp})
  `);
  
  const dailyFees = options.createBalances();
  dailyFees.addGasToken(result[0].revenue);
  
  return { dailyFees };
}
```

#### getGraphDimensions2

Alternative implementation of getGraphDimensions with a simplified approach for newer adapter versions.

```typescript
import { getGraphDimensions2 } from '../helpers/getUniSubgraphVolume';

const adapter = getGraphDimensions2({
  graphUrls: {
    [CHAIN.ETHEREUM]: 'https://api.thegraph.com/subgraphs/name/protocol/subgraph',
  },
  feesPercent: 0.3,
  dailyFeeField: 'feesUSD',
  // Additional configuration options
});
```

[Example Implementation - Quickswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/quickswap.ts)

### Chain-Specific Helpers

#### fetchTransactionFees

Retrieves the total transaction fees for a specific blockchain network within a given time range.

```typescript
import { fetchTransactionFees } from '../helpers/chain-fees';

const fetch = async (options: FetchOptions) => {
  const dailyFees = await fetchTransactionFees(options);
  return { dailyFees };
}
```

### Helper Function Reference

You can find the full source code for these helper functions in the DeFi Llama GitHub repository:

* [Token Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/token.ts) - Contains functions like addTokensReceived, getETHReceived, etc.
* [Uniswap Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/uniswap.ts) - Contains uniV2Exports, uniV3Exports
* [Compound Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/compound.ts) - Contains compoundV2Export
* [Graph Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/getUniSubgraphVolume.ts) - Contains getGraphDimensions2

**Exact day timestamp**

Some data sources are only able to return data given a 00:00:00 day timestamp. In that case you can use the parameter `startOfDay` to get the timestamp of the specific day. For example passing `1663718399 (2022-09-20T23:59:59.000Z)` timestamp will return `1663632000000 (2022-09-20T00:00:00.000Z)`
