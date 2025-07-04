# Data Definitions

### Total Value Locked (TVL)

For protocols: Value of all coins held in smart contracts of the protocol

For chains: Sum of TVL of the protocols in that chain

In traditional finance terms, TVL is similar to Assets Under Management (AUM). It represents the total value of assets that users deposit into a protocol. This is similar to how a fund or bank might report the total value of client deposits or managed assets.

### Fees

Total fees paid by users when using the protocol.

Fees are equivalent to what would traditionally be considered Revenue in most off-chain businesses. They reflect the total amount paid by users for using the service, regardless of where those fees end up. This is the top-line number that shows how much the protocol is facilitating in economic activity.

### Revenue

Subset of fees that the protocol collects for itself, usually going to the protocol treasury, the team or distributed among token holders. This doesn't include any fees distributed to Liquidity Providers.

Revenue, in the way we define it here, is closer to Gross Income in traditional accounting. It’s the portion of fees that the protocol keeps for itself after paying out costs to actors like liquidity providers. This is what goes to the team, the treasury, or tokenholders.

### Token Holder Revenue

Subset of revenue that is distributed to tokenholders by means of buyback and burn, burning fees or direct distribution to stakers.

Token Holder Revenue is similar to Dividends in traditional finance terms. It’s the part of protocol revenue that is returned to tokenholders through staking rewards, fee burns, or direct payouts. Like dividends, this shows how much value the protocol is distributing back to its investors.

### USD Inflows

A protocol's TVL might go down even if more assets are deposited in the scenario where the prices of assets comprising TVL go down, so just looking at the TVL chart is not the best way to see if a protocol is receiving deposits or money is exiting, as that info gets mixed with price movements.

USD Inflows is a metric that fixes that by representing the net asset inflows into a protocol's TVL.

It's calculated by taking the balance difference for each asset between two consecutive days, multiplying that difference by asset price and then summing that for all assets.

If a protocol has all of its TVL in ETH and one day ETH price drops 20% while there are no new deposits or withdrawals, TVL will drop by 20% while USD inflows will be 0$.

### Staked

Value of governance coins that are staked in the protocol's staking system.

### Annual Operational Expenses

Operational costs for salaries, audits... throughout the year. We collect this data mainly from annual protocol reports on their forums, so it's always referencing old data and likely to be outdated up till 1 year.

### Total Raised

Sum of all money raised by the protocol, including VC funding rounds, public sales and ICOs.

### Active Addresses (24h)

Number of unique addresses that have interacted with the protocol directly in the last 24 hours. Interactions are counted as transactions sent directly against the protocol, thus transactions that go through an aggregator or some other middleman contract are not counted here.

The reasoning for this is that this is meant to help measure stickiness/loyalty of users, and users that are interacting with the protocol through another product aren't likely to be sticky.

### Treasury

Value of coins held in ownership by the protocol. By default this excludes coins created by the protocol itself.

In traditional finance, terms, the Treasury is the protocol’s Total Assets. This is what the protocol controls directly, and is often used to fund operations, growth initiatives, or serve as a reserve.

### Token Volume

Sum of value in all swaps to or from that token across all Centralized and Decentralized exchanges tracked by coingecko. Example: for SBR this was all the volume done on SBR/ANY pairs on FTX, Serum...

Data for this metric is imported directly from CoinGecko.

### Token liquidity

Sum of value locked in DEX pools that include that token across all DEXs for which DefiLlama tracks pool data. Example: There's a SOL/SBR pool on Orca (Solana DEX) with 25k of TVL on that pool, so this 25k counts towards the liquidity of SBR.

### Fully Diluted Valuation (FDV)

Fully Diluted Valuation, this is calculated by taking the expected maximum supply of the token and multiplying it by the price. It's mainly used to calculate the hypothetical marketcap of the token if all the tokens were unlocked and circulating.

Data for this metric is imported directly from coingecko.

### Volume

Volume traded on the DEX, this metric is only applicable to protocols that are DEXs (in this case only Saber), and it's just the sum of value of all trades that went through the DEX on a given day.

### Events

This is a manually curated list of events that impacted the protocol. The reason why we display this is just to help the user understand the reason behind changes in the protocol metrics.

Example: When there's a hack that causes a sudden drop in TVL we add an event at that date explaining that a hack was the reason why TVL dropped.

### Median APY

Median APY across DeFi pools tracked by DefiLlama that include a given asset. This is calculated by finding all pools for an asset, then sorting them by APY and finding the median weighted by TVL.

### Contributors

Number of unique github accounts that made at least one commit to a repository in the github organization of a given project.

### Developers

Number of unique github accounts that made at least one commit on three distinct months to a repository in the github organization of a given project.

### Assets (for CEXs)

This includes all the assets that the CEX has under their custody, excluding any assets that are under a different custodian but are credited inside the CEX for use as collateral.
