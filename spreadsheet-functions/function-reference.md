# Function Reference

## Available Metrics

| Metric            | Description                          |
| ----------------- | ------------------------------------ |
| `tvl`             | Total Value Locked                   |
| `borrows`         | Total borrowed amounts               |
| `fees`            | Protocol fees                        |
| `revenue`         | Protocol revenue                     |
| `holders-revenue` | Revenue distributed to token holders |
| `volume`          | DEX trading volume                   |
| `perps`           | Perpetuals trading volume            |
| `mcap`            | Market capitalization                |
| `stablecoins`     | Stablecoin market cap                |
| `fdv`             | Fully diluted valuation              |
| `ofdv`            | Outstanding FDV                      |
| `price`           | Token price                          |

## Core Functions

### DEFILLAMA

Get current metrics for any protocol or chain.

```
DEFILLAMA(metric, name, [timeframe])
```

**Parameters:**

- `metric`: See [Available Metrics](#available-metrics)
- `name`: Protocol or chain name
- `timeframe`: `24h` (default), `7d`, `30d`, `all` (optional) - used for metrics like fees/volume/revenue

**Returns:** Numeric value

**Examples:**

```excel
=DEFILLAMA("tvl", "Ethereum")
=DEFILLAMA("fees", "Uniswap-v2", "24h")
=DEFILLAMA("volume", "GMX", "7d")
```

---

### DEFILLAMA_HISTORICAL

Get historical data for any metric.

```
DEFILLAMA_HISTORICAL(metric, name, startDate, [endDate])
```

**Parameters:**

- `metric`: See [Available Metrics](#available-metrics)
- `name`: Protocol or chain name
- `startDate`: Date format `yyyy-mm-dd` or numeric date
- `endDate`: Optional, returns array if provided

**Returns:** Single numeric value or 2-column array (dates, values)

**Examples:**

```excel
=DEFILLAMA_HISTORICAL("tvl", "Ethereum", "2024-01-01")
=DEFILLAMA_HISTORICAL("fees", "Uniswap", "2024-01-01", "2024-01-31")
```

---

### DEFILLAMA_INFO

Get all available metrics for a protocol or chain.

```
DEFILLAMA_INFO(entity)
```

**Parameters:**

- `entity`: Protocol or chain name

**Returns:** 2-column array with metric names and values

**Example:**

```excel
=DEFILLAMA_INFO("Aave")
```

---

## Utility Functions

### DEFILLAMA_METRICS

List all available metrics.

```
DEFILLAMA_METRICS()
```

## **Returns:** 2-column array with metric names and descriptions

### DEFILLAMA_CHAINS

List all supported blockchains.

```
DEFILLAMA_CHAINS()
```

**Returns:** 3-column array with chain name, ticker symbol, and TVL

---

### DEFILLAMA_PROTOCOLS

List all tracked protocols.

```
DEFILLAMA_PROTOCOLS()
```

**Returns:** 5-column array with protocol name, category, symbol, TVL, and market cap

## Yield Functions

### DEFILLAMA_YIELD

Filter and find yield pools.

```
DEFILLAMA_YIELD([chain], [token], [sortBy], [limit])
```

**Parameters (all optional):**

- `chain`: Blockchain name (e.g., "Ethereum", "Arbitrum")
- `token`: Token symbol (e.g., "USDC", "ETH")
- `sortBy`: `apy` (default), `tvl`, `apyBase`, `apyReward`
- `limit`: Max results (default: 50)

**Returns:** Array with pool name, chain, project, TVL, APY, tokens

**Examples:**

```excel
=DEFILLAMA_YIELD()
=DEFILLAMA_YIELD("Ethereum", "USDC")
=DEFILLAMA_YIELD("", "ETH", "tvl", 10)
```

---

### DEFILLAMA_YIELD_POOL

Get detailed information for a specific pool.

```
DEFILLAMA_YIELD_POOL(poolId)
```

**Parameters:**

- `poolId`: Unique pool identifier

**Returns:** Array with pool details (name, chain, TVL, APY breakdown, tokens, risk metrics)

**Example:**

```excel
=DEFILLAMA_YIELD_POOL("747c1d2a-c668-4682-b9f9-296708a3dd90")
```

---

### DEFILLAMA_YIELD_TOP_POOLS

Get top performing yield pools.

```
DEFILLAMA_YIELD_TOP_POOLS([limit])
```

**Parameters:**

- `limit`: Number of pools (default: 20)

**Returns:** Array with top pools sorted by APY

**Example:**

```excel
=DEFILLAMA_YIELD_TOP_POOLS(50)
```

---

### DEFILLAMA_YIELD_CHAINS

List available chains for yield filtering.

```
DEFILLAMA_YIELD_CHAINS()
```

**Returns:** Array of chain names available in yield pools

**Example:**

```excel
=DEFILLAMA_YIELD_CHAINS()
```

---

### DEFILLAMA_YIELD_PROJECTS

List available projects for yield filtering.

```
DEFILLAMA_YIELD_PROJECTS()
```

**Returns:** Array of project names available in yield pools

---

## Stablecoin Functions

### DEFILLAMA_STABLECOINS

List all tracked stablecoins.

```
DEFILLAMA_STABLECOINS()
```

**Returns:** Array with stablecoin name, symbol, market cap, price, dominant chain

---

### DEFILLAMA_STABLECOIN_MCAP

Get stablecoin market cap.

```
DEFILLAMA_STABLECOIN_MCAP([name], [chain])
```

**Parameters (all optional):**

- `name`: Stablecoin name or symbol (default: "all" for total)
- `chain`: Blockchain filter (default: "all" for aggregate)

**Returns:** Numeric value (market cap in USD)

**Examples:**

```excel
=DEFILLAMA_STABLECOIN_MCAP()
=DEFILLAMA_STABLECOIN_MCAP("USDC")
=DEFILLAMA_STABLECOIN_MCAP("USDT", "Ethereum")
```

---

### DEFILLAMA_STABLECOIN_HISTORY

Get historical stablecoin data.

```
DEFILLAMA_STABLECOIN_HISTORY(name, startDate, [endDate], [chain])
```

**Parameters:**

- `name`: Stablecoin name, symbol, or "all"
- `startDate`: Date format `yyyy-mm-dd` or numeric date
- `endDate`: Optional end date (returns array if provided)
- `chain`: Optional blockchain filter (default: "all")

**Returns:** Single value or 2-column array (dates, market caps)

**Examples:**

```excel
=DEFILLAMA_STABLECOIN_HISTORY("USDC", "2024-01-01")
=DEFILLAMA_STABLECOIN_HISTORY("USDT", "2024-01-01", "2024-12-31")
=DEFILLAMA_STABLECOIN_HISTORY("all", "2024-01-01", "2024-12-31", "Ethereum")
```

---
