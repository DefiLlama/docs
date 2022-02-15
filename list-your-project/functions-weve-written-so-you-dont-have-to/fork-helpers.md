# Fork helpers

### Olympus DAO

```
const { ohmTvl } = require("../helper/ohm");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/snowcatdao/index.js" %}
Example Olympus DAO Adapter
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

```
const { masterChefExports } = require("../helper/masterchef");
const { addFundsInMasterChef } = require('../helper/masterchef');
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/boujefinance/index.js" %}
Example masterChefExports adapter
{% endembed %}

### Uniswap V2

There are a few different helpers for Uni V2 forks but we recommend using calculateUsdUniTvl.

```
const { calculateUsdUniTvl } = require("../helper/getUsdUniTvl");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/solarflare.js" %}
Example Uni V2 Adapter
{% endembed %}

