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
const { compoundExports2 } = require("../helper/compound");
module.exports = {
  polygon: compoundExports2({ 
    comptroller: '0x...',
    cether: '0x...', // optional, needed if gas token is used
  }),
};
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/basilisk/index.js" %}
Example Compound Fork Adapter
{% endembed %}



### Liquity

```javascript
const { getLiquityTvl } = require("../helper/liquity.js")

module.exports = {
  chainX: {
    tvl: getLiquityTvl('0x...'),
  }
}
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/liquidloans-io/index.js" %}
Example Liquity Fork Adapter
{% endembed %}


### Balancer V2

```javascript

const { onChainTvl } = require('../helper/balancer')

module.exports = {
  metis: {
    tvl: onChainTvl('0x...', <startBlock>),
  }
}
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/hummus-weighted/index.js" %}
Example Balancer V2 Fork Adapter
{% endembed %}

