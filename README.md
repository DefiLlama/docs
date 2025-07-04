# DefiLlama and our methodology

We pride ourselves in producing inclusive, non-biased, and community driven statistics for the decentralised finance industry. We do our best to treat all projects equally with regards to what is and isn't included in TVL, how long it takes to list or update a project's TVL, and everything else.

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
