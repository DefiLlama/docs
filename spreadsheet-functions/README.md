# Getting Started

## Google Sheets

### Installation
Open the extension page on [Google Workspace Marketplace](https://workspace.google.com/marketplace/app/defillama_sheets/571407189628) to install the add-on.


### Setup
1. **Extensions** → **DefiLlama** → **Open Sidebar**
2. Click **Sign In**
3. Authorize with your defillama account

### Test
```excel
=DEFILLAMA("price", "Bitcoin")
```
---

## Excel

### Installation
1. Open Excel
2. **Insert** → **Add-ins** -> **More Add-ins**
3. Search "DefiLlama"
4. Click **Add**

### Setup
1. Click **DefiLlama Sheets**
2. Click **Sign In**
3. Authenticate with your account

### Test
```excel
=DEFILLAMA("tvl", "Ethereum")
```

### Troubleshooting
- **#NAME? error**: Restart Excel, ensure add-in is enabled
- **Auth issues**: Sign out and sign in again via task pane

### In case of any issues, questions or feedback, please contact us at [support@defillama.com](mailto:support@defillama.com)
---

## Quick Examples

```excel
// Current data
=DEFILLAMA("tvl", "Ethereum")
=DEFILLAMA("fees", "Uniswap", "24h")
=DEFILLAMA("price", "Bitcoin")

// Historical
=DEFILLAMA_HISTORICAL("tvl", "Aave", "2024-01-01")

// Yields
=DEFILLAMA_YIELD("Arbitrum", "USDC", "apy", 10)
=DEFILLAMA_YIELD_TOP_POOLS(20)

// Stablecoins
=DEFILLAMA_STABLECOIN_MCAP()  // Total market cap
=DEFILLAMA_STABLECOIN_MCAP("USDC", "Ethereum")
```

## Next Steps

- [Function Reference](./function-reference.md) - Complete parameter documentation
- [Templates](./templates.md) - Templates for common use cases
- [Support](mailto:support@defillama.com) - In case of any issues, questions or feedback