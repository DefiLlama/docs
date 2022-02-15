# Staking and Pool2

The stakings and pool2 functions make it really simple to add any native token TVL.

```
const { stakings } = require("../helper/staking");
const { pool2s } = require("../helper/pool2");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/bitpif/index.js" %}
Example Stakings Adapter
{% endembed %}

If your protocol token isn't on GitHub, you can estimate the USD value of the token using stakingPriceLP. This function will use uniV2 pool weights we determine the value of the staked tokens. (NB: there must be a Uni V2 pool with your coin in, which has significant liquidity. Otherwise the price oracle will be unreliable and vulnerable to manipulation.)

```
const { stakingPricedLP } = require("../helper/staking");
```

{% embed url="https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/corgiswap.js" %}
Example Price From LP Adapter
{% endembed %}

