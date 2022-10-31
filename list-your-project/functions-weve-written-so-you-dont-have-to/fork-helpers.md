# Fork helpers

### Uniswap V2

There are a few different helpers for Uni V2 forks but we recommend using `uniTvlExport`.

```
const { uniTvlExport } = require('../helper/unknownTokens')
const chain = 'yourChain'
const factory = '0x...' // v2 factory address

module.exports = uniTvlExport(chain, factory)
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/kekswap/index.js" %}
Example Uni V2 Adapter
{% endembed %}
### GMX


```
const { gmxExports } = require('../helper/gmx')
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/nex/index.js" %}
Example GMX Adapter
{% endembed %}

### Aave

```
const { aaveExports } = require("../helper/aave")
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/klap/index.js" %}
Example Aave Fork Adapter
{% endembed %}

### Compound

```
const { usdCompoundExports } = require("../helper/compound");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/huckleberry/index.js" %}
Example Compound Fork Adapter
{% endembed %}

### MasterChefs

For MasterChef's we recommend using masterChefExports, but in some more complicated scenarios you may need to use addFundsInMasterChef.

```const { masterchefExports } = require("../helper/unknownTokens")
const { addFundsInMasterChef } = require('../helper/masterchef');
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/sudoinu/index.js" %}
Example masterChefExports adapter
{% endembed %}

### Olympus DAO

```
const { ohmTvl } = require("../helper/ohm");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/snowcatdao/index.js" %}
Example Olympus DAO Adapter
{% endembed %}


### tomb

```
const { unknownTombs } = require("../helper/unknownTokens")
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/gametheory/index.js" %}
Example tomb Adapter
{% endembed %}
