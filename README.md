# DefiLlama and our methodology

We pride ourselves in producing inclusive, non-biased, and community driven statistics for the decentralised finance industry. We do our best to treat all projects equally with regards to what is and isn't included in TVL, how long it takes to list or update a project's TVL, and everything else.

## All Metrics We Track

DefiLlama tracks a wide range of metrics across DeFi. Each metric has its own open-source adapter repository where anyone can contribute. If you want to list your project, find the relevant metric below and submit a PR to the corresponding repo.

| Metric | Description | Repository | Guide |
| --- | --- | --- | --- |
| **TVL** | Total Value Locked in protocol smart contracts | [DefiLlama-Adapters](https://github.com/DefiLlama/DefiLlama-Adapters) | [Guide](list-your-project/how-to-write-an-sdk-adapter.md) |
| **Fees & Revenue** | Protocol fees, revenue, and income statement breakdown | [dimension-adapters](https://github.com/DefiLlama/dimension-adapters) (`fees/`) | [Guide](list-your-project/other-dashboards/README.md) |
| **DEX/Perp Volume** | Spot/swap and perpetual trading volume on decentralized exchanges | [dimension-adapters](https://github.com/DefiLlama/dimension-adapters) (`dexs/`) | [Guide](list-your-project/other-dashboards/README.md) |
| **Options Volume** | Notional and premium volume from options DEXs | [dimension-adapters](https://github.com/DefiLlama/dimension-adapters) (`options/`) | [Guide](list-your-project/other-dashboards/README.md) |
| **Aggregator Volume** | Trading volume routed through DEX aggregators | [dimension-adapters](https://github.com/DefiLlama/dimension-adapters) (`aggregators/`) | [Guide](list-your-project/other-dashboards/README.md) |
| **Bridge Aggregator Volume** | Volume routed through bridge aggregators | [dimension-adapters](https://github.com/DefiLlama/dimension-adapters) (`bridge-aggregators/`) | [Guide](list-your-project/other-dashboards/README.md) |
| **Yields** | APY and yield data across DeFi pools | [yield-server](https://github.com/DefiLlama/yield-server) | [Guide](https://github.com/DefiLlama/yield-server/blob/master/README.md) |
| **Stablecoins** | Circulating supply and peg data for stablecoins | [peggedassets-server](https://github.com/DefiLlama/peggedassets-server) | [Guide](https://github.com/DefiLlama/peggedassets-server/blob/master/README.md) |
| **Emissions** | Token emission and unlock schedules | [emissions-adapters](https://github.com/DefiLlama/emissions-adapters) | [Guide](list-your-project/emissions-dashboard/README.md) |
| **Oracles TVS** | Total Value Secured by oracle providers | [defillama-server](https://github.com/DefiLlama/defillama-server) | [Guide](list-your-project/oracles-tvs.md) |
| **Coin Prices** | Token price adapters for unlisted coins | [coins](https://github.com/DefiLlama/defillama-server/tree/master/coins) | [Guide](coin-prices-api.md) |
| **Bridges** | Bridge TVL and cross-chain transfer tracking | [bridges-server](https://github.com/DefiLlama/bridges-server) | [Guide](https://github.com/DefiLlama/bridges-server/blob/master/README.md) |
| **Token Rights** | Governance, economic, and ownership rights | [Submit form](https://token-rights-teams.llama.fi/) | [Guide](list-your-project/token-rights.md) |

---

### Our Methodology

At DefiLlama we consider the value of any tokens locked in the contracts of a protocol / platform as TVL. Below are some notes about how our calculations work.

Valuing different tokens:

* Almost all tokens are priced using CoinGecko's API. Where this can't be done, we can accommodate using on-chain methods to quantify the value of a token. This is most commonly done by comparing the pool weights of a very liquid Uniswap V2 market.&#x20;
* We don't count any tokens that are not circulating or are yet to be issued. For example if a team locks a share of their tokens in a vesting contract we won't count these as TVL, since these tokens haven't been issued yet.
* We don't double-count within the same protocol. If users can deposit a token, get a receipt token and deposit that in another part of your protocol we'll only count it once. For example Cream has an ETH2 liquid staking token, which can be lent on their money market, we only count it once.

Node validators and other chain-native token staking:

* We don't count native token staking. For example, ATOM staking to secure the Cosmos hub isn't counted. Liquid staking protocols are tracked but not counted towards chain TVL by default.

Bridges:

* There are arguments both for including bridge TVL on the origin chain and the destination chain. Therefore we count the TVL of bridge projects but do not contribute them towards the TVL of any chain.&#x20;

Smart Wallets:

* We don't count funds in smart contract wallets, such as Argent or Gnosis Safe.

In the case of protocols that move money to different chains, TVL is counted on the chain where the users deposited the money and interacted with the protocol.
