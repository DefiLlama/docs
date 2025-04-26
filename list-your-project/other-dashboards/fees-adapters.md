# Building Fees Adapters

This guide will help you create adapters for the [fees dashboard](https://defillama.com/fees) that track fees and revenue metrics for DeFi protocols.

## Adapter Versions

Before creating a fees adapter, you need to decide which version to use:

- **V1 Adapters**: Run for fixed time periods (00:00 to 23:59 of a given day)
- **V2 Adapters**: Accept arbitrary start and end timestamps as input

The key differences:

- V1 adapters are suitable for APIs that only return daily data with no way to specify custom time ranges
- V2 adapters allow for more flexible time ranges and real-time data (e.g., pulling data from 2 PM yesterday to 2 PM today)
- Use the `runAtCurrTime` flag for adapters that can only return the most recent 24-hour data with no historical support

## Implementation Steps

### 1. Identify Supported Chains

Identify all chains your protocol operates on by referencing the [chains.ts](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/chains.ts) file.

### 2. Check for Existing Adapters

Before implementation, search DeFiLlama and the dimensions repo to ensure an adapter doesn't already exist for your project.

### 3. Define Start Dates

For each chain, determine the deployment date of your protocol in YYYY-MM-DD format or as a unix timestamp. This helps with data backfilling.

### 4. Implement Fetch Function

The core of your adapter is the `fetch` function that collects fees data. You'll need to implement logic to return some or all of these dimensions:

- **dailyFees**: All fees collected from all sources (including staking rewards, yields, etc.)
- **dailyUserFees**: Fees paid directly by protocol users (excluding gas fees)
- **dailyRevenue**: Total revenue kept by the protocol (treasury + token holders)
- **dailyProtocolRevenue**: Revenue directed to the treasury
- **dailyHoldersRevenue**: Revenue going to governance token holders
- **dailySupplySideRevenue**: Revenue earned by liquidity providers

> Note: Your protocol doesn't need to implement all dimensions - only those relevant to your business model.

## Data Sources

There are several ways to gather fee data for your protocol. Choose the approach that best suits your project's architecture and data availability:

### 1. Chain Event Logs

Best for small/mid-size projects with manageable event volume. Provides the most accurate data directly from the source.

```typescript
const logs = await getLogs({
  target: "0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4",
  eventAbi: 'event Trade(address trader, address subject, bool isBuy, uint256 shareAmount, uint256 ethAmount, uint256 protocolEthAmount, uint256 subjectEthAmount, uint256 supply)'
})
```

**Example adapters using event logs**:
- [Ostium](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/ostium/index.ts) - Tracks various fee types from protocol events

### 2. Token Transfer Tracking

For protocols that collect fees in specific tokens, DeFi Llama provides several helper functions to track token movements:

```typescript
import { addTokensReceived } from '../helpers/token';

const fetch = async (options: FetchOptions) => {
  // Track ERC20 token transfers to treasury
  const dailyFees = await addTokensReceived({
    options,
    tokens: ["0x4200000000000000000000000000000000000006"], // WETH on Base
    targets: ["0xbcb4a982d3c2786e69a0fdc0f0c4f2db1a04e875"] // Treasury
  });

  // Track native token transfers
  const dailyRevenue = options.createBalances();
  await getETHReceived({
    options,
    balances: dailyRevenue,
    target: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045" // Treasury
  });

  return { dailyFees, dailyRevenue };
}
```

**Example adapters using token tracking**:
- [Synthetix](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/synthetix.ts) - Uses addTokensReceived to track sUSD fees
- [DexTools](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/dextools.ts) - Combines getETHReceived and getSolanaReceived for multi-chain support

### 3. Subgraphs

Fast and efficient but can become out of sync with the blockchain. Useful for protocols with existing well-maintained subgraphs.

```typescript
import { request } from "graphql-request";

const fetch = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  const endBlock = await options.getEndBlock();
  
  const query = `
    query getFeeData($block: Int!) {
      feeStats(block: { number: $block }) {
        feeAmount
        tokenAddress
      }
    }
  `;
  
  const { feeStats } = await request(subgraphEndpoint, query, { block: endBlock });
  
  feeStats.forEach(stat => {
    dailyFees.add(stat.tokenAddress, stat.feeAmount);
  });
  
  return { dailyFees };
}
```

**Example adapters using subgraphs**:
- [Curve](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/curve.ts) - Fetches fee data from multiple subgraphs
- [TheGraph](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/thegraph.ts) - Tracks fees across multiple chains with complex fee distribution
- [LlamaLend](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/llamalend.ts) - Calculates interest-based fees from subgraph data
- [Dackieswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/dackieswap.ts) - Handles variable fee tiers with subgraph data

### 4. Query Engines

Options include DeFiLlama indexers, Dune Analytics, Flipside Crypto, and Allium. These are especially useful for complex queries or historical analysis.

```typescript
// Using Dune Analytics
const value = await queryDune("3521814", {
  start: options.startTimestamp,
  end: options.endTimestamp,
  receiver: '9yMwSPk9mrXSN7yDHUuZurAh1sjbJsfpUqjZ7SvVtdco'
});

// Using Allium
const result = await queryAllium(`
  SELECT SUM(value) as revenue
  FROM ethereum.transactions
  WHERE to_address = '0x123...abc'
  AND block_timestamp BETWEEN TO_TIMESTAMP_NTZ(${options.startTimestamp}) AND TO_TIMESTAMP_NTZ(${options.endTimestamp})
`);
```

**Example adapters using query engines**:
- [Pumpswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/pump-swap/index.ts) - Uses Dune Analytics queries
- [Sudoswap V2](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/sudoswap-v2.ts) - Uses DeFiLlama's queryIndexer

### 5. Contract Calls

For protocols where fee data is accessible through view functions or requires multiple contract interactions.

```typescript
import { Adapter, FetchOptions } from "../../adapters/types";

// Contract addresses
const VOTER = "0xd7ea36ECA1cA3E73bC262A6D05DB01E60AE4AD47";
const BERO = "0x7838CEc5B11298Ff6a9513Fa385621B765C74174";
const HONEY = "0xFCBD14DC51f0A4d49d5E53C2E0950e0bC26d0Dce";

// Fee constants 
const SWAP_FEE = 30n;
const BORROW_FEE = 250n;
const DIVISOR = 10000n;

async function fetch(options: FetchOptions) {
  const dailyFees = options.createBalances();
  
  // Get plugin data through contract calls
  const plugins = await options.api.call({
    target: VOTER,
    abi: "address[]:getPlugins",
  });
  
  // Use multiCall for efficiency when making multiple similar calls
  const bribes = await options.api.multiCall({
    abi: "function getBribe() returns (address)",
    calls: plugins.map((plugin) => ({ target: plugin })),
  });
  
  // Collect fee data from events
  for (const bribe of bribes) {
    const logs = await options.getLogs({
      target: bribe,
      fromBlock: await options.getFromBlock(),
      toBlock: await options.getToBlock(),
      eventAbi: "event Bribe__RewardNotified(address indexed rewardToken, uint256 reward)",
    });
    
    logs.forEach((log) => {
      dailyFees.add(log.rewardToken, log.reward);
    });
  }
  
  return { dailyFees };
}
```

**Example adapters using contract calls**:
- [Beradrome](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/beradrome/index.ts) - Combines contract calls with event logs to track multiple fee sources


### 6. Protocol API Endpoints

Many protocols offer dedicated API endpoints that provide fee and revenue metrics. These should be used with proper validation to ensure accurate reporting.

```typescript
import fetchURL from "../utils/fetchURL";

const thalaDappURL = "https://app.thala.fi";
const feesQueryURL = `${thalaDappURL}/api/defillama/trading-fee-chart?timeframe=`;

const fetch = async (timestamp: number) => {
  const dayFeesQuery = (await fetchURL(feesQueryURL + "1D&endTimestamp=" + timestamp))?.data;
  const dailyFees = dayFeesQuery.reduce(
    (sum: number, a: any) => sum + a.value,
    0
  );
  
  return {
    dailyFees,
    dailyRevenue: dailyFees * 0.2, // If protocol keeps 20% of fees
    timestamp,
  };
};
```

**Example adapter using API endpoints**:
- [Thalaswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/thalaswap.ts) - Fetches metrics from protocol API

### 7. Helper Wrappers for DEXs and Lending Protocols

For common protocol types, DeFiLlama provides specialized helpers that simplify adapter creation:

**Example adapters using helper wrappers**:
- [Katana](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/katana.ts) - Uses getDexChainFees
- [Quickswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/quickswap.ts) - Uses getGraphDimensions2
- [2thick](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/2thick.ts) - Uses uniV3Exports for Uniswap V3-style DEXs
- [Nile Exchange V1](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/nile-exchange-v1/index.ts) - Uses uniV2Exports for Uniswap V2-style DEXs
- [Hydrometer](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/hydrometer/index.ts) - Uses uniV2Exports 
- [Strike](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/strike/index.ts) - Uses compoundV2Export for Compound-style lending protocol

## Helper Functions

To simplify adapter creation, DefiLlama provides numerous [helper functions](./helper-functions.md) for common tasks:

### Transaction Tracking Helpers

- `getETHReceived`: Track native tokens received by addresses on EVM chains
- `addTokensReceived`: Track ERC20 token transfers to specific addresses
- `getSolanaReceived`: Track token transfers on Solana
- `getLogs`: Retrieve event logs from blockchains

### Protocol-Specific Helpers

- `compoundV2Export`: Create adapters for Compound V2-like protocols
- `getUniV2LogAdapter`: Create adapters for Uniswap V2-style DEXs
- `uniV2Exports`: Generate adapters for Uniswap V2 across multiple chains
- `uniV3Exports`: Generate adapters for Uniswap V3-style DEXs
- `getDexChainFees`: Calculate fees based on volume data

### Chain-Specific Helpers

- `blockscoutFeeAdapter2`: Create adapters using Blockscout explorer data
- `L2FeesFetcher`: Track fees for Layer 2 networks
- `fetchTransactionFees`: Get transaction fees for a blockchain network

Visit the [helper functions documentation](./helper-functions.md) for detailed examples and usage instructions.


## Testing Your Adapter

Run this command to test your adapter:

```
npm test fees [protocolSlug]
```

Or test with a specific timestamp:

```
npm test fees [protocolSlug] [timestamp]
```

The test will validate your adapter's output format and ensure it returns the expected dimensions. 

## Methodology Documentation

When implementing your adapter, always include a clear methodology to explain how fees and revenue are calculated:

```typescript
meta: {
  methodology: {
    Fees: "User pays 0.3% on each swap",
    Revenue: "Protocol keeps 0.05% of each swap",
    SupplySideRevenue: "LPs receive 0.25% of each swap",
    UserFees: "Total fees paid by users (0.3% per swap)"
  }
}
```

Proper methodology documentation ensures transparency and helps users understand your protocol's fee structure. 