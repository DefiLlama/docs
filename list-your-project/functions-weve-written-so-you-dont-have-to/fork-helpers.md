# Fork helpers

### Uniswap V2

There are a few different helpers for Uni V2 forks but we recommend using `uniTvlExport`.

```javascript
const { uniTvlExport } = require('../helper/unknownTokens')
const chain = 'yourChain'
const factory = '0x...' // v2 factory address

module.exports = uniTvlExport(chain, factory)
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/kekswap/index.js" %}
Example Uni V2 Adapter
{% endembed %}

### Uniswap V3

There are a few different helpers for Uni V3 forks but we recommend using `uniTvlExport`.

```javascript
const { uniV3Export } = require('../helper/uniswapV3')

module.exports = uniV3Export({
  chainX: { factory: '0x...', fromBlock: 'block when factory contract was deployed' },
  chainY: { factory: '0x...', fromBlock: 'block when factory contract was deployed' },
})
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/beamswap-v3/index.js" %}
Example Uni V3 Adapter
{% endembed %}

### GMX

```javascript
const { gmxExports } = require('../helper/gmx')

module.exports = {
  bsc: {
    tvl: gmxExports({ vault: '0x...', })
  }
}
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/nex/index.js" %}
Example GMX Adapter
{% endembed %}

### Aave

```javascript
const { aaveExports } = require("../helper/aave")
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/klap/index.js" %}
Example Aave Fork Adapter
{% endembed %}

### Compound

```javascript
const { compoundExports } = require("../helper/compound");
const { compoundExports2 } = require("../helper/compound");
module.exports = {
  polygon: compoundExports(controller, chain, cETHAddress, '0x0000000000000000000000000000000000000000'),
  polygon: compoundExports2({ 
    comptroller: '0x...',
    cether: '0x...', // optional, needed if gas token is used
  }),
};
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/huckleberry/index.js" %}
Example Compound Fork Adapter
{% endembed %}

### MasterChefs

For MasterChef's we recommend using masterChefExports, but in some more complicated scenarios you may need to use addFundsInMasterChef.

```javascript

const { masterchefExports } = require("../helper/unknownTokens")
const { addFundsInMasterChef } = require('../helper/masterchef');
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/sudoinu/index.js" %}
Example masterChefExports adapter
{% endembed %}

### Olympus DAO

```javascript
const { ohmTvl } = require("../helper/ohm");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/snowcatdao/index.js" %}
Example Olympus DAO Adapter
{% endembed %}

### tomb

```javascript
const { unknownTombs } = require("../helper/unknownTokens")
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/gametheory/index.js" %}
Example tomb Adapter
{% endembed %}
