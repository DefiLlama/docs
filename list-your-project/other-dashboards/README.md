# How to write dimensions adapters

This guide will help you create adapters for DefiLlama's various dashboards, including [fees](https://defillama.com/fees), [volumes](https://defillama.com/dexs), [aggregators](https://defillama.com/aggregators), [derivatives](https://defillama.com/derivatives), [Bridge Aggregators](https://defillama.com/bridge-aggregators), [Options](https://defillama.com/options), and others.

## What is an Adapter?

An adapter is some code that:

1. Collects data on a protocol by calling some endpoints or making blockchain calls
2. Computes a response and returns it

It's a TypeScript file that exports an async function which takes a FetchOptions object containing:

* startTimestamp: Unix timestamp for start of period
* endTimestamp: Unix timestamp for end of period
* startBlock: Block number corresponding to start timestamp
* endBlock: Block number corresponding to end timestamp
* createBalances: Helper function to track token balances
* api: Helper for making contract calls
* getLogs: Helper for fetching event logs

The function returns an object with metrics (like fees, volume, etc.) for that time range.

## Introduction to Dimension Adapters

DefiLlama's dashboards track various metrics (dimensions) for DeFi protocols. Each dashboard focuses on specific dimensions:

* **Dexs dashboard**: Tracks trading volume from DEXs (spot/swaps)
* **Fees dashboard**: Tracks fees and revenue from all types of protocols
* **Aggregators dashboard**: Tracks volume from DEX aggregators
* **Derivatives dashboard**: Tracks volume from derivatives protocols
* **Aggregator-Derivatives dashboard**: Tracks volume from aggregator-derivatives protocols
* **Bridge Aggregators dashboard**: Tracks volume from bridge aggregators
* **Options dashboard**: Tracks notional and premium volume from options DEXs

## How to List Your Project

The majority of adapters for DefiLlama dashboards are contributed and maintained by their respective communities, with all changes being coordinated through the [`DefiLlama/dimension-adapters` GitHub repo](https://github.com/DefiLlama/dimension-adapters).

To add your protocol to any dashboard, follow these steps:

1. Fork the [`dimension-adapters`](https://github.com/DefiLlama/dimension-adapters) repository
2. Create a new file at `[dashboard]/yourProtocolName/index.ts` or `[dashboard]/yourProtocolName.ts` (where `[dashboard]` is the relevant folder like `fees`, `dexs`, `aggregators`, `aggregator-derivatives`, `bridge-aggregators`, `options`, etc.)
3. Implement your adapter following the guidelines in this document
4. Test your adapter using `npm test [dashboard] yourProtocolName`
5. Submit a PR! A llama will review it and merge it. Once merged, it can take up to 24h to be available in the dashboard

{% hint style="info" %}
Seeing issues getting logs or with calls at historical blocks? You can replace the RPC being used by creating a .env file and filling it with rows like this: ETHEREUM\_RPC="https://..." BSC\_RPC="https://..." POLYGON\_RPC="https://..." ...
{% endhint %}

## Basic Example

Let's start with a simple, complete example of a fees adapter:

```typescript
import { FetchOptions, SimpleAdapter } from "../../adapters/types";
import { CHAIN } from "../../helpers/chains";

const FeeCollectedEvent = "event FeesCollected(address indexed _token, address indexed _integrator, uint256 _integratorFee, uint256 _lifiFee)"

const LIFIFeeCollector = '0xbD6C7B0d2f68c2b7805d88388319cfB6EcB50eA9';

const fetch = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  const data: any[] = await options.getLogs({
    target: LIFIFeeCollector,
    eventAbi: FeeCollectedEvent,
  });
  data.forEach((log: any) => {
    dailyFees.add(log._token, log._integratorFee);
  });
  return { dailyFees, dailyRevenue: dailyFees, dailyProtocolRevenue: dailyFees };
};

const methodology = {
  Fees: 'All fees paid by users for swap and bridge tokens via LI.FI.',
  Revenue: 'All fees are kept by LI.FI as protocol revenue.',
  ProtocolRevenue: 'All fees are distributed to LI.FI treasury.',
}

const adapter: SimpleAdapter = {
  version: 2,
  fetch,
  chains: [CHAIN.ETHEREUM],
  start: '2023-07-27',
  methodology
}

export default adapter;
```

### Adapter Structure

The object exported by your adapter file defines its behavior. The main configuration object holds a `version` key and supports two different structures:

**Recommended Structure**: For protocols with the same fetch logic across all chains, you can use the simplified structure with `fetch`, `chains`, `start`, and `methodology` at the root level.

### SimpleAdapter Properties

* **fetch**: The core async function that returns different dimensions of a protocol. The dimensions returned depend on which dashboard you're targeting (e.g., `dailyVolume` for the dexs dashboard, `dailyFees` for the fees dashboard). See "Core Dimensions" below.
* **chains**: Array of chain constants (e.g., `[CHAIN.ETHEREUM, CHAIN.POLYGON]`) indicating which chains this adapter supports.
* **start**: The earliest timestamp (as YYYY-MM-DD or unix timestamp) we can pass to the fetch function. This tells our servers how far back we can get historical data.
* **methodology**: (Optional) Object describing how different dimensions are calculated. See "Metadata and Methodology" below.
* **runAtCurrTime**: (Optional, defaults to `false`) Boolean flag. Set to `true` if the adapter can only return the latest data (e.g., last 24h) and cannot reliably use the `startTimestamp` and `endTimestamp` passed to `fetch`.

#### Example of Multi-chain Root-level Structure

```typescript
import { CHAIN } from "../../helpers/chains";

const methodology = {
  Fees: 'All fees paid by users for protocol operations.',
  Revenue: 'Portion of fees kept by the protocol after paying suppliers.',
  SupplySideRevenue: 'Portion of fees distributed to liquidity providers.',
  ProtocolRevenue: 'Portion of gross profit allocated to protocol treasury.',
}

const adapter: SimpleAdapter = {
  version: 2,
  fetch,
  chains: [CHAIN.ETHEREUM, CHAIN.POLYGON, CHAIN.ARBITRUM],
  start: '2023-01-01',
  methodology
}
```

## Testing Your Adapter

Test your adapter locally before submitting a PR:

```
> npm test [dashboard] [protocolSlug]
> npm test [dashboard] [protocolSlug] [timestamp]
```

```
npm test fees katana
```

To test at specific day (unix format or yyyy-mm-dd):

```
npm test fees katana 1662110960
npm test dexs katana 2025-04-10
```

This checks if your adapter correctly returns data for the requested time period.

### Adapter Version

The top-level `version` key specifies the adapter version:

* **Version 2 (Recommended)**: `version: 2`. These adapters accept arbitrary start and end timestamps as input to `fetch`, allowing for flexible time ranges.
* **Version 1**: `version: 1`. Use this only if your `fetch` function can run for fixed time periods only (00:00 to 23:59 of a given day), typically because the underlying data source only provides daily data.

### Core Dimensions

Your `fetch` function should return an object containing properties corresponding to the metrics (dimensions) relevant to the dashboard you are targeting. All dimensions should be returned as balance objects (`Object<string>`) where keys are the token identifiers (e.g., `ethereum:0x...`) and values are the raw amounts (no decimal adjustments).

> **Minimum Requirements:** To be listed, your adapter **must** provide accurate `dailyFees` and `dailyRevenue` dimensions. `dailySupplySideRevenue` is strongly encouraged whenever the protocol has supply-side costs. `dailyHoldersRevenue` should be included for protocols that distribute value to tokenholders. Always include breakdown labels and `breakdownMethodology`. Cumulative `total*` dimensions are deprecated and should not be used.

Here are the standard dimensions grouped by dashboard type:

**Dexs and Dex Aggregators Dimensions:**

* `dailyVolume`: (**Required**) Trading volume for the period.

**Derivatives and Aggregators-Derivatives Dimensions:**

* `dailyVolume`: (**Required**) Perpetual trading volume for the period.
* `openInterestAtEnd`: (Optional) Open interest at the end of the period.
* `longOpenInterestAtEnd`: (Optional) Long open interest at the end of the period.
* `shortOpenInterestAtEnd`: (Optional) Short open interest at the end of the period.

**Bridge Aggregators Dimensions:**

* `dailyBridgeVolume`: (**Required**) Bridge volume for the period.

**Options Dimensions:**

* `dailyNotionalVolume`: (**Required**) Notional volume of options contracts traded/settled.
* `dailyPremiumVolume`: (**Required**) Premium volume collected/paid.
* `openInterestAtEnd`: (Optional) Open interest at the end of the period.
* `longOpenInterestAtEnd`: (Optional) Long open interest at the end of the period.
* `shortOpenInterestAtEnd`: (Optional) Short open interest at the end of the period.

**Fees Dimensions:**

Our fees dimensions follow an income statement model inspired by GAAP accounting standards. See the "Breakdown Labels & Income Statement" section below for the full methodology and rationale.

* `dailyFees`: (**Required**) All fees and value collected from _all_ sources (users, LPs, yield generation, liquid staking rewards, etc.), representing the total value flow into the protocol's ecosystem. This maps to **Gross Protocol Revenue** on the income statement — everything the protocol could theoretically keep if it took 100%. For a DEX this is total swap fees, for lending this is all borrow interest, for liquid staking this is all staking rewards from staked ETH. Block rewards are **not** fees — they are incentives. For chain adapters, track only transaction fees paid by users (not perp DEX fees built on top).
* `dailyUserFees`: (Optional, but helpful) The portion of `dailyFees` directly paid by end-users (e.g., swap fees, borrow interest, liquidation penalties, marketplace commissions paid by buyers/sellers).
* `dailySupplySideRevenue`: (**Required when applicable**) The portion of `dailyFees` distributed to liquidity providers, lenders, stakers, or other suppliers of capital/resources. This maps to **Cost of Revenue** on the income statement. Examples: LP fees on a DEX, interest paid to lenders, staking rewards passed through to stETH holders, blob fees to mainnet for rollups, validator commissions, trading rebates.
* `dailyRevenue`: (**Required**) The portion of `dailyFees` kept by the protocol entity itself. This maps to **Gross Profit** on the income statement. `dailyRevenue = dailyFees - dailySupplySideRevenue`.
* `dailyProtocolRevenue`: (Optional, clarifies revenue split) The portion of `dailyRevenue` allocated to the protocol's treasury or core team.
* `dailyHoldersRevenue`: (Optional, but important for protocols distributing to holders) All value flowing to governance token holders. This maps to **Tokenholder Income** on the income statement. Includes buybacks, token burns, direct distributions, AND income from external sources (airdrops from other protocols, bribes from other protocols, etc.).

> **Deprecated:** `dailyBribeRevenue` and `dailyTokenTax` are deprecated as separate dimensions. Instead, include these as labeled sub-sections within `dailyHoldersRevenue` (e.g., `dailyHoldersRevenue.add(token, amount, 'Bribes from Protocol X')`).

**Fee/Revenue Attribution Examples by Protocol Type:**

If you are unsure how to classify fees and revenues, refer to this table or contact us at support@defillama.com or ask on Discord:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

| Attribute         | DEXs                                        | Lending                                    | Chains                                         | NFT Marketplace                        | Derivatives                      | CDP                        | Liquid Staking                  | Yield                              | Synthetics                       |
| ----------------- | ------------------------------------------- | ------------------------------------------ | ---------------------------------------------- | -------------------------------------- | -------------------------------- | -------------------------- | ------------------------------- | ---------------------------------- | -------------------------------- |
| UserFees          | Swap fees paid by users                     | Interest paid by borrowers                 | Gas fees paid by users                         | Fees paid by users                     | Fees paid by users               | Interest paid by borrowers | % of rewards paid to protocol   | Paid management + performance fees | Fees paid by users               |
| Fees              | =UserFees                                   | =UserFees                                  | =UserFees                                      | =UserFees                              | UserFees + burn/mint fees        | =UserFees                  | Staking rewards                 | Yield                              | =UserFees                        |
| SupplySideRevenue | LPs revenue                                 | Interest paid to lenders                   | Sequencer costs, blob fees                     | Creator earnings                       | LP revenue, trading rebates      | \*                         | Revenue earned by stETH holders | Yield excluding protocol fees      | LPs revenue                      |
| Revenue           | % of swap fees going to protocol governance | % of interest going to protocol governance | Burned coins (fees-sequencerCosts for rollups) | Marketplace revenue + creator earnings | Protocol governance revenue      | =ProtocolRevenue           | =ProtocolRevenue                | =ProtocolRevenue                   | =ProtocolRevenue                 |
| ProtocolRevenue   | % of swap fees going to treasury            | % of interest going to protocol            | \*                                             | Marketplace revenue                    | Value going to treasury          | Interest going to treasury | =UserFees                       | =UserFees                          | % of fees going to treasury      |
| HoldersRevenue    | Money going to gov token holders            | \*                                         | \*                                             | \*                                     | Value going to gov token holders | \*                         | \*                              | \*                                 | % of fees going to token holders |

> **Notes:**
>
> * Protocol governance includes treasury + gov token holders.
> * `Revenue = HoldersRevenue + ProtocolRevenue`.
> * `Revenue = Fees - SupplySideRevenue`.
> * Asterisk (\*) indicates typically not applicable or zero for that category.
> * For chains: only track transaction fees paid by users. Perp DEX fees on Hyperliquid L1 are tracked under the perp adapter, not the chain adapter.

## Implementation Steps

Building the `fetch` function is the core task. Here's a breakdown:

1. **Identify Supported Chains**: Determine which blockchains your protocol runs on by referencing the [chains.ts](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/chains.ts) file. For the recommended root-level structure, add these to the `chains` array. For chain-specific configurations, you'll need a `BaseAdapter` entry for each chain.
2. **Define Start Dates**: Find your protocol's deployment date to set the `start` property (root-level for consistent dates, or per-chain for different deployment dates). This enables proper data backfilling.
3. **Choose Data Source(s)**: Select the appropriate method(s) to retrieve the necessary data for calculating dimensions. Common approaches are detailed below.

## Data Source Examples

Choose the appropriate data source based on your protocol's architecture. The `fetch` function receives an `options` object containing helper utilities like `createBalances`, `getLogs`, `api` (for contract calls), `queryDuneSql`, etc.

### On-Chain Event Logs

Ideal for tracking specific events that generate fees or volume:

```typescript
const fetch = async ({ getLogs, createBalances }) => {
  const dailyFees = createBalances();
  const dailyRevenue = createBalances();
  
  const logs = await getLogs({
    target: "0xcf205808ed36593aa40a44f10c7f7c2f67d4a4d4",
    eventAbi: 'event Trade(address trader, address subject, bool isBuy, uint256 shareAmount, uint256 ethAmount, uint256 protocolEthAmount, uint256 subjectEthAmount, uint256 supply)'
  });
  
  logs.forEach(log => {
    dailyFees.addGasToken(log.protocolEthAmount * 2);  // Example: Total fees
    dailyRevenue.addGasToken(log.protocolEthAmount);   // Example: Protocol's share
  });
  
  return { dailyFees, dailyRevenue };
};
```

Example: [Ostium](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/ostium/index.ts)

### Token Transfer Tracking

Track tokens received by protocol treasury/fee addresses:

```typescript
import { addTokensReceived } from '../../helpers/token';

const fetch = async (options: FetchOptions) => {
  // Track ERC20 token transfers to treasury
  const dailyFees = await addTokensReceived({
    options,
    tokens: ["0x4200000000000000000000000000000000000006"], // WETH on Base
    targets: ["0xbcb4a982d3c2786e69a0fdc0f0c4f2db1a04e875"] // Treasury
  });

  // Example: Assuming all received tokens are fees and revenue
  return { dailyFees, dailyRevenue: dailyFees }
}
```

Example: [Synthetix](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/synthetix.ts)

### Subgraphs

Fast queries for protocols with well-maintained subgraphs:

```typescript
import { request } from "graphql-request";

const fetch = async (options: FetchOptions) => {
  const dailyVolume = options.createBalances();
  
  const query = `{
    volumeStats(where: {timestamp_gte: ${options.startTimestamp}, timestamp_lt: ${options.endTimestamp}}) {
      volumeUSD
      token
    }
  }`;
  
  const { volumeStats } = await request("https://api.thegraph.com/subgraphs/name/protocol/subgraph", query);
  
  volumeStats.forEach(stat => {
    // Assuming volumeUSD needs conversion if not directly usable
    dailyVolume.add(stat.token, stat.volumeUSD); 
  });
  
  return { dailyVolume };
};
```

Examples:

* [Curve](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/curve.ts)
* [LlamaLend](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/llamalend.ts)
* [TheGraph](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/thegraph.ts)
* [Dackieswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/dackieswap.ts)

### Query Engines (Dune, Flipside, Allium)

For complex queries or when direct blockchain access is too expensive:

```typescript
const fetch = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  
  const results = await options.queryDuneSql(`
    SELECT 
      SUM(amount) as fees,
      token_address
    FROM ethereum.transactions
    WHERE to_address = '0x123...abc' -- Example fee address
      AND block_time >= FROM_UNIXTIME(${options.startTimestamp})
      AND block_time < FROM_UNIXTIME(${options.endTimestamp})
    GROUP BY token_address
  `);
  
  if (results && results.length > 0) {
    results.forEach(row => {
      dailyFees.add(row.token_address, row.fees);
    });
  }
  
  return { dailyFees };
};
```

Example: [Pumpswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/pump-swap/index.ts)

### Contract Calls

For protocols where data is accessible through view functions or requires multiple contract interactions:

```typescript
const fetch = async (options: FetchOptions) => {
  const dailyFees = options.createBalances();
  
  // Example: Get plugin data through contract calls
  const plugins = await options.api.call({
    target: "0xd7ea36ECA1cA3E73bC262A6D05DB01E60AE4AD47", // Contract address
    abi: "address[]:getPlugins",
  });
  
  // Use multiCall for efficiency when making multiple similar calls
  const bribes = await options.api.multiCall({
    abi: "function getBribe() returns (address)",
    calls: plugins
  });
  
  // Example: Collect fee data from events emitted by bribe contracts
  for (const bribe of bribes) {
    const logs = await options.getLogs({
      target: bribe,
      eventAbi: "event Bribe__RewardNotified(address indexed rewardToken, uint256 reward)",
    });
    
    logs.forEach((log) => {
      dailyFees.add(log.rewardToken, log.reward);
    });
  }
  
  return { dailyFees };
};
```

Example: [Beradrome](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/beradrome/index.ts)

## Metadata and Methodology

Always include a `methodology` object to explain how your metrics are calculated. This is crucial for transparency.

```typescript
const methodology = {
  Fees: "All swap fees paid by traders (0.3% per trade).",
  Revenue: "Protocol keeps 0.05% of each swap after paying LPs.",
  SupplySideRevenue: "LPs receive 0.25% of each swap as liquidity incentive.",
  HoldersRevenue: "Protocol buybacks funded from treasury revenue.",
  ProtocolRevenue: "0.05% of swap fees allocated to protocol treasury.",
}

const adapter: SimpleAdapter = {
  version: 2,
  fetch,
  chains: [CHAIN.ETHEREUM],
  start: '2023-01-01',
  methodology
}
```

The `methodology` object provides a one-line summary per dimension. For detailed per-label explanations, use `breakdownMethodology` (see below).

## Breakdown Labels & Income Statement

### Why We Use an Income Statement Model

Our previous system only displayed aggregated numbers with no breakdown. Users didn't know what we were counting (e.g., does Ethereum fees include blob fees?), there was no way to tell if a new revenue stream was being tracked, and we only captured tokenholder income from burns/distributions while missing airdrops, bribes, etc.

We moved to a system inspired by **GAAP** accounting standards. The goal is:
* Break down each component as much as possible
* Name and description of each component must be easy to understand — if you were a user and saw this breakdown, would you understand what each thing is?
* When a user wonders "does this include X revenue stream/cost?" it should be trivial to answer by looking at the breakdown
* Include every way that tokenholders make money, even if it's coming from another protocol

### Income Statement Template

This template shows the most common revenue streams and costs, and where each belongs:

**Gross Protocol Revenue** (`dailyFees`):
* \+ Swap Fees
* \+ Liquidation Fees
* \+ Interest Income (borrow interest)
* \+ Staking Rewards
* \+ MEV Captured
* \+ Gas Fees (transaction fees on chains and rollups)

**Cost of Funds** (`dailySupplySideRevenue`):
* \- LP Payments
* \- Interest Expenses (paid to lenders/depositors)
* \- Staking Rewards, less fees (passed through to stakers)
* \- MEV paid to stakers, less fees
* \- Blob fees to mainnet (for rollups)
* \- Validator Commissions
* \- Trading Rebates (including those funded by token emissions)

**Gross Profit** (`dailyRevenue`) = Gross Protocol Revenue - Cost of Funds

**Operating Expenses:**
* \- Offchain expenses such as salaries (difficult to track, ignore if not possible)
* \- Token Emissions (liquidity mining, staking rewards)
* \- Airdrops (user acquisition / retroactive rewards, ideally amortize over length of points program)
* \- DAO Grants

**Operating Income** = Gross Profit - Operating Expenses

**Other Income:**
* \+ Treasury interest income (excluding from own token)
* \+ Treasury investment income
* \+ Protocol owned liquidity income
* \+ Grant revenue
* \+ Third party liquidity incentives

**Net Income** = Operating Income + Other Income

---

**Tokenholder Income** (`dailyHoldersRevenue`) — OFF STATEMENT:

*Capital Allocations:*
* \+ Treasury Buybacks
* \+ Tokenholder Distributions

*Other Tokenholder Flows:*
* \+ Airdrops Received by Tokenholders from Other Protocols (e.g., Binance Earn where BNB stakers receive airdrops from tokens that launch on Binance)
* \+ Bribes Received by Tokenholders from Other Protocols
* \+ Other Off-Protocol Tokenholder Income

**Tokenholder Income** = Capital Allocations + Other Tokenholder Flows

*Other Incentives (OFF STATEMENT):*
* \+ Third party liquidity incentives
* \+ Points programs

### When to Use Breakdown Labels

**Always provide labels, even when there is only one source/destination of fees.** This prevents having to update and backfill data later when the adapter is listed under a parent protocol.

For example, when writing a Fluid DEX adapter, add a `'Swap Fees'` label even though it has only one source of fees. Later, when Fluid Lending is also listed and both are grouped under the Fluid parent protocol, the DEX adapter already has proper breakdown labels and doesn't need updating or data backfilling.

### How Labels Change Per Dimension

Labels should vary by dimension to provide the most useful information:

* **`dailyFees`**: Use **source-of-fees** labels that describe where money comes from. Simple labels like: `'Swap Fees'`, `'Borrow Interest'`, `'Flashloan Fees'`, `'Liquidation Fees'`, `'Staking Rewards'`, `'MEV Rewards'`
* **`dailyRevenue`**, **`dailyProtocolRevenue`**, **`dailySupplySideRevenue`**, **`dailyHoldersRevenue`**: Use **more detailed labels** that describe both the source and destination: `'Swap Fees To LPs'`, `'Borrow Interest To Treasury'`, `'Borrow Interest To Lenders'`, `'Staking Rewards To Protocol'`

This distinction matters because the same source of fees often splits across multiple destinations:

```typescript
// dailyFees: simple source labels
dailyFees.add(token, totalBorrowInterest, 'Borrow Interest')

// dailySupplySideRevenue: detailed destination labels
dailySupplySideRevenue.add(token, lenderShare, 'Borrow Interest To Lenders')

// dailyRevenue: detailed destination labels
dailyRevenue.add(token, protocolShare, 'Borrow Interest To Treasury')
```

### Adding Breakdown Labels in Code

The third parameter in `.add()` specifies the category label. All balance methods support this:

```typescript
// .add() with label
dailyFees.add(tokenAddress, amount, 'Borrow Interest')

// .addGasToken() with label
dailyFees.addGasToken(amount, 'Staking Rewards')

// .addUSDValue() with label
dailySupplySideRevenue.addUSDValue(usdAmount, 'Borrow Interest To Lenders')

// .add() with Balances object and label
dailyRevenue.add(balancesObj, 'Spot Fees')
```

### Label Naming Best Practices

Labels must be **clear, descriptive, and immediately understandable to a user who sees the breakdown**:

**Good labels:**
* `'Borrow Interest'` — clear what borrowers are paying
* `'GHO Borrow Interest'` — specific to the GHO market
* `'Liquidation Fees'` — describes the fee source
* `'Staking Rewards'` — clear revenue source
* `'MEV Rewards'` — specific MEV-related revenue
* `'Spot Fees'` — trading fees on spot markets
* `'Borrow Interest To Treasury'` — clear destination
* `'Borrow Interest To Lenders'` — clear who receives it
* `'Spot fees on Unit markets'` — specific enough to distinguish from other spot fees

**Bad labels:**
* `'Protocol Fees'` — too vague, doesn't explain what kind of fees
* `'Fees'` — not descriptive at all
* `'Revenue'` — doesn't explain the source
* `'Other'` — not informative
* `'Misc'` — meaningless to users

**Key principles:**
1. **Be specific**: Users should immediately understand what each label means
2. **Break down as much as possible**: More granular breakdowns are always better
3. **Use constants when they fit**: Check `helpers/metrics.ts` for shared labels, but **prioritize clarity over reuse**
4. **Vary labels across dimensions**: Use different labels in dailyFees vs dailySupplySideRevenue when it helps understanding
5. **Think like income statements**: Would an investor reading this breakdown understand exactly where the money comes from and goes?
6. **Answer "does this include X?"**: When a user wonders if a specific revenue stream or cost is included, the answer should be obvious from the breakdown

### Using Metric Constants

Standard labels are defined in [`helpers/metrics.ts`](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/metrics.ts):

```typescript
import { METRIC } from '../../helpers/metrics';

METRIC.BORROW_INTEREST       // 'Borrow Interest'
METRIC.LIQUIDATION_FEES      // 'Liquidation Fees'
METRIC.FLASHLOAN_FEES        // 'Flashloan Fees'
METRIC.SWAP_FEES             // 'Token Swap Fees'
METRIC.LP_FEES               // 'LP Fees'
METRIC.STAKING_REWARDS       // 'Staking Rewards'
METRIC.MEV_REWARDS           // 'MEV Rewards'
METRIC.TOKEN_BUY_BACK        // 'Token Buy Back'
METRIC.CREATOR_FEES          // 'Creator Fees'
METRIC.MARGIN_FEES           // 'Margin Fees'
METRIC.OPEN_CLOSE_FEES       // 'Open/Close Fees'
METRIC.PERFORMANCE_FEES      // 'Performance Fees'
METRIC.MANAGEMENT_FEES       // 'Management Fees'
METRIC.CURATORS_FEES         // 'Curators Fees'
METRIC.OPERATORS_FEES        // 'Operators Fees'
METRIC.TRADING_FEES          // 'Trading Fees'
METRIC.TRANSACTION_GAS_FEES  // 'Transaction Gas Fees'
METRIC.TRANSACTION_BASE_FEES // 'Transaction Base Fees'
METRIC.TRANSACTION_PRIORITY_FEES // 'Transaction Priority Fees'
METRIC.MINT_REDEEM_FEES      // 'Mint/Redeem Fees'
METRIC.DEPOSIT_WITHDRAW_FEES // 'Deposit/Withdraw Fees'
METRIC.SERVICE_FEES          // 'Service Fees'
METRIC.ASSETS_YIELDS         // 'Assets Yields'
METRIC.PROTOCOL_FEES         // 'Protocol Fees'
```

**When to use constants vs custom labels:**
* **Use constants** when they clearly describe your category
* **Write custom labels** when constants would be unclear or confusing — user understanding is more important than code consistency
* Before creating a new constant, check if an existing one fits. Before using a constant, check if it's clear enough for your use case.

### breakdownMethodology Object

**Every label used in `.add()` calls MUST appear in `breakdownMethodology`**, and every label in `breakdownMethodology` must have corresponding data in code. This object documents each sub-section.

#### Structure

```typescript
const breakdownMethodology = {
  Fees: {
    'Label A': 'Description of this revenue source',
    'Label B': 'Description of this revenue source',
  },
  Revenue: {
    'Label C': 'Description of what protocol keeps from this source',
  },
  SupplySideRevenue: {
    'Label D': 'Description of what suppliers receive',
  },
  ProtocolRevenue: {
    'Label E': 'Description of what goes to treasury',
  },
  HoldersRevenue: {
    'Label F': 'Description of tokenholder income source',
  },
}
```

#### Complete Example: Aave (Lending)

```typescript
const breakdownMethodology = {
  Fees: {
    'Borrow Interest': 'All interest paid by borrowers from all markets (excluding GHO).',
    'Borrow Interest GHO': 'All interest paid by borrowers from GHO only.',
    'Liquidation Fees': 'Fees from liquidation penalty and bonuses.',
    'Flashloan Fees': 'Flashloan fees paid by flashloan borrowers and executors.',
  },
  Revenue: {
    'Borrow Interest': 'A portion of interest paid by borrowers from all markets (excluding GHO).',
    'Borrow Interest GHO': 'All 100% interest paid by GHO borrowers.',
    'Liquidation Fees': 'A portion of fees from liquidation penalty and bonuses.',
    'Flashloan Fees': 'A portion of fees paid by flashloan borrowers and executors.',
  },
  SupplySideRevenue: {
    'Borrow Interest': 'Amount of interest distributed to lenders from all markets (excluding GHO).',
    'Borrow Interest GHO': 'No supply side revenue for lenders on GHO market.',
    'Liquidation Fees': 'Fees from liquidation penalty and bonuses are distributed to lenders.',
    'Flashloan Fees': 'Flashloan fees paid by flashloan borrowers and executors are distributed to lenders.',
  },
  ProtocolRevenue: {
    'Borrow Interest': 'Interest from all markets (excluding GHO) collected by Aave treasury.',
    'Borrow Interest GHO': 'All interest paid on GHO market collected by Aave treasury.',
    'Liquidation Fees': 'A portion of liquidation fees collected by Aave treasury.',
    'Flashloan Fees': 'A portion of flashloan fees collected by Aave treasury.',
  },
}
```

#### Example: Fluid (Lending with Buybacks)

```typescript
const FLUID_METRICS = {
  BorrowInterest: METRIC.BORROW_INTEREST,
  TokenBuyBack: METRIC.TOKEN_BUY_BACK,
  BorrowInterestToTreasury: 'Borrow Interest To Treasury',
  BorrowInterestToLenders: 'Borrow Interest To Lenders',
}

const breakdownMethodology = {
  Fees: {
    [FLUID_METRICS.BorrowInterest]: "All interest paid by borrowers.",
  },
  Revenue: {
    [FLUID_METRICS.BorrowInterestToTreasury]: "Percentage of interest going to treasury.",
  },
  ProtocolRevenue: {
    [FLUID_METRICS.BorrowInterestToTreasury]: "Percentage of interest going to treasury.",
  },
  SupplySideRevenue: {
    [FLUID_METRICS.BorrowInterestToLenders]: "Amount of interest distributed to lenders.",
  },
  HoldersRevenue: {
    [FLUID_METRICS.TokenBuyBack]: "FLUID token buyback from the treasury.",
  },
}
```

#### Example: Hyperliquid (DEX with Buybacks)

```typescript
const breakdownMethodology = {
  Fees: {
    'Spot Fees': 'Fees collected on all spot trades, excluding trades on markets with Unit assets (eg bridged BTC).',
    'Spot fees on Unit markets': 'Fees from spot trades on markets that include an asset deployed by Unit, in these spot markets all fees go to Unit.',
  },
  Revenue: {
    'Spot Fees': '99% of spot trade fees, excluding perp fees and unit protocol fees.',
  },
  SupplySideRevenue: {
    'Unit Revenue': 'All fees earned on Unit spot markets go to Unit.',
    'HLP': '1% of the spot fees go to HLP vault (used to be 3% before 30 Aug 2025).',
  },
  HoldersRevenue: {
    [METRIC.TOKEN_BUY_BACK]: "99% of spot trade fees (excluding perp fees and unit protocol fees) for buy back HYPE tokens.",
  },
}
```

#### Example: Liquity (CDP with No Protocol Revenue)

```typescript
const breakdownMethodology = {
  Fees: {
    'Borrow Fees': 'One-time borrow fees paid by borrowers.',
    'Redemption Fees': 'Redemption fees paid by borrowers.',
    'Gas Compensation': 'Gas compensations paid to liquidators when triggering liquidations.',
    'Liquidation Profit': 'Profit from ETH collaterals distributed to stability pool stakers on liquidations.',
  },
  Revenue: {
    'Borrow Fees': 'One-time borrow fees paid by borrowers.',
    'Redemption Fees': 'Redemption fees paid by borrowers.',
  },
  HoldersRevenue: {
    'Borrow Fees': 'Borrow fees distributed to LUSD stability pool and LQTY stakers.',
    'Redemption Fees': 'Redemption fees distributed to LUSD stability pool and LQTY stakers.',
  },
  SupplySideRevenue: {
    'Gas Compensation': 'Gas compensations paid to liquidators when triggering liquidations.',
    'Liquidation Profit': 'Profit from ETH collaterals distributed to stability pool stakers.',
  }
}
```

### Adding to Your Adapter

```typescript
const adapter: SimpleAdapter = {
  version: 2,
  fetch,
  chains: [CHAIN.ETHEREUM],
  start: '2023-01-01',
  methodology,
  breakdownMethodology,
}
```

### Requirements Checklist

**Must have:**
1. Every label used in `.add()` calls has a corresponding entry in `breakdownMethodology`
2. Every label in `breakdownMethodology` has corresponding data assigned in code
3. Labels are descriptive and immediately understandable to users
4. Descriptions clearly explain what each category represents
5. Labels are provided even when there's only one source of fees (for parent protocol compatibility)
6. `dailySupplySideRevenue` (Cost of Revenue) is tracked whenever the protocol pays out to suppliers

**Common mistakes to avoid:**
1. Using vague labels like "Protocol Fees" or "Other" — be specific
2. Missing `breakdownMethodology` entries for labels used in code
3. Having `breakdownMethodology` entries with no corresponding data
4. Not breaking down enough — more detail is always better
5. Using the same labels across all dimensions when different labels would be clearer
6. Forgetting to label single-source adapters (breaks when listed under parent protocol)
7. Tracking block rewards as fees — they are incentives, not fees
8. Including perp DEX fees in chain adapters — those belong in the perp adapter

### Real-World Examples

Browse these adapters for complete implementations:
* [Aave](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/aave-v3.ts) — Multi-market lending with GHO breakdown
* [Fluid](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/fluid/index.ts) — Lending with treasury/lender split and buybacks
* [Liquity](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/liquity.ts) — CDP with borrow, redemption, and liquidation fees
* [Hyperliquid](https://github.com/DefiLlama/dimension-adapters/blob/master/dexs/hyperliquid-spot/index.ts) — DEX with unit markets and HYPE buybacks

For full income statement examples showing how major protocols in each category break down their financials, see the [examples folder](./examples/).

## Code Structure Guidelines

Follow these rules when writing adapters:

* **Prefer on-chain data**: Use on-chain event logs and contract calls where possible. We are stricter about this for chains where we maintain our own indexer, or where there is significant volume/fees, or where you suspect wash trading.
* **Use `pullHourly: true`**: This avoids recomputing data for the same time period repeatedly, and provides more granular hourly data.
* **Never swallow errors**: It's better to fail than to return incorrect data. If a small chain with $10K volume fails, it shouldn't break an adapter that tracks $100M daily on other chains — return 0 for the failing chain.
* **Use/add helper code**: When multiple adapters use similar logic, extract it into shared helpers.
* **No npm dependencies**: Do not add npm packages. This leads to bloat.
* **Use `api.multiCall`**: Prefer `api.multiCall` over `Promise.all` for batching EVM calls. Use `PromisePool` for non-EVM calls.
* **Return token breakdowns**: Always return amounts with token addresses (not pre-converted USD). Always include `methodology` and `breakdownMethodology` (where appropriate).
* **Watch for wash trading**: Be vigilant about wash trading, especially on low-fee chains.

## Important Considerations

### Precision

Use the `BigNumber` library (available via `options.createBalances()` or direct import) for mathematical operations involving token amounts, especially when dealing with different decimals or potentially large/small numbers, to avoid JavaScript precision issues.

```typescript
import BigNumber from "bignumber.js";

// ... inside fetch function
const feesInGas = new BigNumber(graphRes["fees"]);
const ethGasPrice = await getGasPrice(timestamp); // Assuming getGasPrice helper exists
const dailyFees = options.createBalances(); // Create a Balances object

dailyFees.addGasToken(feesInGas.multipliedBy(ethGasPrice).toString()); 

return {
  dailyFees,
  dailyRevenue: dailyFees
}; 
```

## Helper Functions Reference

DeFiLlama provides numerous helper functions to simplify common tasks in adapter development. These are available either via direct import or through the `options` object passed to your `fetch` function.

### Protocol-Specific Helpers (Common Abstractions)

These helpers provide high-level abstractions for common DeFi protocol archetypes.

#### Uniswap V2/V3-like Protocols

*   **`uniV2Exports` / `getUniV2LogAdapter`**: Generates adapter configurations for Uniswap V2-style DEXes across multiple chains.

    ```typescript
    import { uniV2Exports } from '../helpers/uniswap';

    // Example for a Uniswap V2 fork on BSC
    export default uniV2Exports({
      [CHAIN.BSC]: {
        factories: ['0x123...abc'], // Factory address
        fees: {
          type: 'fixed', // Or 'variable' or 'stable'
          feesPercentage: 0.3 // Swap fee percentage
        }
      }
      // Other chains...
    });
    ```

    Examples:

    * [Nile Exchange V1](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/nile-exchange/index.ts) (V2-style)
    * [Hydrometer](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/hydrometer/index.ts)
    * [ABCDEFX](https://github.com/DefiLlama/dimension-adapters/blob/master/dexs/abcdefx/index.ts)
*   **`uniV3Exports`**: Creates adapters for Uniswap V3-style DEXes, supporting variable fees and multiple pools.

    ```typescript
    import { uniV3Exports } from '../helpers/uniswap';

    // Example for a Uniswap V3 fork on Scroll
    export default uniV3Exports({
      [CHAIN.SCROLL]: { 
        factory: '0xAAA32926fcE6bE95ea2c51cB4Fcb60836D320C42',
        // Optional custom fee handling or additional configurations
      }
      // Other chains...
    })
    ```

    Example:

    * [2thick](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/2thick.ts) (V3-style)

#### Compound V2-like Protocols

*   **`compoundV2Export`**: Creates an adapter for Compound V2-like protocols, taking config parameters and returning an object that tracks fees, revenue, and distribution among holders and suppliers.

    ```typescript
    import { compoundV2Export } from '../helpers/compound';

    // Example for a Compound V2 fork on Ethereum
    export default compoundV2Export({
      reserveFactor: 0.1, // Example: 10% of interest goes to protocol
      markets: {
        [CHAIN.ETHEREUM]: {
          comptroller: '0x123...abc', // Comptroller address
          // ... other market parameters like specific cToken addresses if needed
        }
        // Other chains...
      }
    });
    ```

    Example:

    * [Strike](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/strike/index.ts)

### Token Tracking Helpers

Functions for tracking native and ERC20 token movements.

*   **`addTokensReceived`**: Tracks ERC20 token transfers received by specified addresses. Supports filtering by sender/receiver and custom token transformations. Uses indexer first, then logs.

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

    Example:

    * [Synthetix](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/synthetix.ts)
*   **`addGasTokensReceived`**: Tracks native token transfers (like ETH) received by specified multisig addresses.

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
*   **`getETHReceived`**: Tracks native token transfers on EVM chains via Allium DB queries.

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
*   **`getSolanaReceived`**: Fetches token transfers to specified Solana addresses, allows blacklisting senders/signers.

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

    Example:

    * [Axiom](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/axiom.ts)

### EVM Data Helpers

Functions for querying EVM logs and indexers.

*   **`getLogs`** (Available via `options.getLogs`): Retrieves event logs based on filters (target, signature, topics).

    ```typescript
    const fetch = async (options: FetchOptions) => { // options includes getLogs
      const dailyFees = options.createBalances();
      const logs = await options.getLogs({
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
*   **`queryIndexer`**: Executes queries against DefiLlama's indexers (transfers, events, etc.).

    ```typescript
    import { queryIndexer } from '../../helpers/indexer';

    const fetch = async (options: FetchOptions) => {
      const transfers = await queryIndexer({
        chain: options.chain,
        fromTimestamp: options.startTimestamp, 
        toTimestamp: options.endTimestamp,
        type: 'Transfer', // Example: query token transfers
        filter: { to: "0x123...abc" } 
      });
      
      const dailyFees = options.createBalances();
      transfers.forEach(t => dailyFees.add(t.token, t.value));
      
      return { dailyFees };
    }
    ```

    [Example Implementation - Sudoswap V2](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/sudoswap-v2.ts)

### Query Engine Helpers

Functions for querying external data platforms.

*   **`queryDuneSql`** (Available via `options.queryDuneSql`): Executes SQL queries against Dune Analytics.

    ```typescript
    const fetch = async (options: FetchOptions) => { // options includes queryDuneSql
      const dailyFees = options.createBalances();
      const results = await options.queryDuneSql(`
        SELECT SUM(fee_amount) as fees
        FROM ethereum.transactions
        WHERE to = '0x123...abc'
        AND block_time BETWEEN to_timestamp(${options.startTimestamp}) AND to_timestamp(${options.endTimestamp})`
      );
      
      if (results && results.length > 0) {
        dailyFees.addGasToken(results[0].fees);
      }
      
      return { dailyFees };
    }
    ```

    [Example Implementation - Pumpswap](https://github.com/DefiLlama/dimension-adapters/blob/master/fees/pump-swap/index.ts)
*   **`queryAllium`** (Available via `options.queryAllium`): Queries the Allium database.

    ```typescript
    const fetch = async (options: FetchOptions) => { // options includes queryAllium
      const dailyFees = options.createBalances();
      const result = await options.queryAllium(`
        SELECT SUM(value) as revenue
        FROM ethereum.transactions
        WHERE to_address = '0x123...abc'
        AND block_timestamp BETWEEN TO_TIMESTAMP_NTZ(${options.startTimestamp}) AND TO_TIMESTAMP_NTZ(${options.endTimestamp})
      `);
      
      if (result && result.length > 0) {
        dailyFees.addGasToken(result[0].revenue);
      }
      
      return { dailyFees };
    }
    ```

### Chain-Specific Helpers

Helpers tailored for specific chains or L2s.

*   **`fetchTransactionFees`** (Available via `options.fetchTransactionFees`): Retrieves total native token transaction fees burned/collected by the network.

    ```typescript
    const fetch = async (options: FetchOptions) => { // options includes fetchTransactionFees
      const dailyFees = await options.fetchTransactionFees(); 
      // Assumes network fees are protocol revenue
      return { dailyFees, dailyRevenue: dailyFees }; 
    }
    ```

### General Helpers

Utility functions for common adapter patterns.

*   **`startOfDay`** (Available via `options.startOfDay`): Converts `options.endTimestamp` to 00:00:00 UTC for data sources requiring exact day timestamps.

    ```typescript
    const fetch = async (options: FetchOptions) => {
      const startOfDayTimestamp = options.startOfDay; 
      // Use startOfDayTimestamp in API calls requiring a 00:00:00 timestamp
      // e.g., const data = await fetchAPI(`...?date=${startOfDayTimestamp}`);
      // ...
    }
    ```

### Helper Source Code Reference

You can find the full source code for these helper functions in the DefiLlama GitHub repository:

* [Token Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/token.ts) - Contains functions like addTokensReceived, getETHReceived, getSolanaReceived, etc.
* [Uniswap Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/uniswap.ts) - Contains uniV2Exports, uniV3Exports
* [Compound Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/compoundV2.ts) - Contains compoundV2Export
* [Aave Helpers](https://github.com/DefiLlama/dimension-adapters/blob/master/helpers/aave/index.ts) - Contains aaveExports

## Frequently Asked Questions

### How does DeFiLlama ensure data quality and accuracy?

**Code Review Process**: Each protocol adapter undergoes peer review by llamas through GitHub pull requests. This ensures code quality, data accuracy, and that a consistent methodology is applied to all protocols for the same metrics before any adapter goes live.

**Methodology Consistency**: We maintain a uniform methodology across all protocol adapters and chains. Whenever the methodology evolves, our team propagates the update to every relevant adapter to ensure figures remain fully comparable across protocols.

**Monitoring Systems**: We maintain internal alert systems that detect unusual data spikes, broken adapters, and anomalies across both TVL and dimension adapters (fees/revenue/volume). This allows the team to quickly identify and fix issues.

**Historical Data Integrity**: When protocols add new components (like treasury wallets, new contracts, etc.), we backfill historical data to maintain completeness and accuracy. This ensures users have access to accurate historical insights.

### Data Classification Rules

* **Fees**: Only fees paid by users for a transaction/storage/etc should be tracked as fees. Block rewards are a cost and must be tracked as incentives, not fees.
* **Revenue**: Only the part of fees that gets burnt (or similar) can be tracked as revenue. The part that goes to stakers does not benefit the chain/holders — make sure to tag these correctly in the breakdown.
* **Holder revenue**: Usually the same as revenue unless a portion is set aside for the protocol (like in the case of Zcash).
* **Chain fees**: Track only the transaction fees paid by users. Do not include perp DEX fees for protocols like Hyperliquid L1 — those are counted under the perp listing.

### How we handle data integrity and keep data organic?

**Wash Trading Detection**: We actively identify and remove wash trading volumes to prevent them from undermining legitimate trading data.

**TVL Percentage Rules**: For pools with very low fee percentages (like 0.01%) that enable wash trading, we apply minimum TVL percentage rules. Only volume from pools meeting these thresholds is counted, effectively filtering out wash trading while preserving legitimate activity.

**Chain-Specific Considerations**:

* **Solana**: Due to lower transaction fees that make wash trading more viable, we apply TVL percentage filters to major Solana DEXs while maintaining legitimate volumes
* **BSC**: During farming campaigns that create wash trading incentives for low-liquidity pairs, we remove affected pairs to maintain data integrity

### How can I report data issues or provide feedback?

You can report issues or provide feedback by sending an email to support@defillama.com

Llamas regularly review feedback and implement necessary fixes to maintain the highest data quality standards across all adapters.
