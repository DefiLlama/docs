# How to add a new Blockchain

## For EVM Chain

#### 1. **Add the Blockchain to `chains.json`**

You need to add the name of the blockchain in the `projects/helper/chains.json` file to recognize it as a new supported chain.

**Example Change**:

```json
"chains": [
  "ethereum",
  "binance-smart-chain",
  "polygon",
  "zklink",  // Add your new blockchain here. 
  "fraxtal",
  "zksync"
]
```

You should use the field  shortName from [https://chainlist.org/rpcs.json](https://chainlist.org/rpcs.json)

***

***

#### 2. **Update RPC URLs in `env.js (Optional)`**

You'll need to add the appropriate RPC endpoint for the new blockchain in the `projects/helper/env.js` file.

**Example Change**:

```js
const ENV_KEYS = [
  "OZONE_RPC": "https://node1.ozonechain.io",
  "ZETA_RPC": "https://zetachain-evm.blockpi.network/v1/rpc/public",
  "DEFIVERSE_RPC": "https://rpc.defi-verse.org/",
  "ZKLINK_RPC": "https://rpc.zklink.io",  // Add the RPC URL for your new blockchain
];
```

***

#### 3. **Add Token Mappings in `tokenMapping.js`**

Add the token mappings for the new blockchain to the `projects/helper/tokenMapping.js` file. This file maps token addresses to their respective identifiers for accurate tracking and handling.

**Example Change**:

```js
const fixBalancesTokens = {
  zklink: {
    [ADDRESSES.zklink.WETH]: { coingeckoId: "ethereum", decimals: 18 },
  },
  ozone: {
     '0x83048f0bf34feed8ced419455a4320a735a92e9d': { coingeckoId: "ozonechain", decimals: 18 }, 
  },
};
```

This ensures tokens on the new blockchain (`zklink`) are properly recognized, including their `coingeckoId` for price tracking and their decimals.

***

#### 4. Submit a Protocol **using your blockchain (e.g.,** projects/savmswap/index.j&#x73;**)**

Lastly, update the project’s configuration file to add your new blockchain as a valid supported chain. If we don´t track any protocol on your blockchain, we can not add it. So make sure to add the new chain under a current project or add a new adapter to track the project on your blockchain

**Example Change**:

```js
const { uniTvlExport } = require('../helper/unknownTokens')

module.exports = uniTvlExport('zklink', '0x1842c9bD09bCba88b58776c7995A9A9bD220A925') //blockchain, factory address


```

