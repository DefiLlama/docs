# What to include as TVL?

### TVL

Total value locked inside a platform / protocol's own contracts by users.

### TVL Filters

We separate TVL into different types. This lets users decide what they do and do not want to include in the dashboard data. These types are:

* Staking - the platform's own tokens
* Pool2 - staked LP tokens where one side of the market is the platform's own governance token.
* Borrows - deposits borrowed from the platform
* Vesting - Tokens that are not circulating or not issued yet. This mostly applies to vesting protocols where a token with 10M mcap and 1B FDV could have 500M locked, in these cases it makes no sense for TVL from that token to be 500M when mcap of it is only 10M
* Offers - funds that are approved for spending on a non-custodial platform, but not actually deposited into the platform contracts

### Not TVL

* Assets that aren't on the blockchain, such as bonds or fiat currency. We don't consider the dollars stored on Tether's bank account as TVL, for example.
* We also don't accept assets that your protocol generates and are locked into other protocols, as that's the later protocols TVL, not your project's. See [this](https://github.com/DefiLlama/DefiLlama-Adapters/pull/60#issuecomment-807045050) for rationale.
* We don't count native token staking. For example, ATOM staking to secure the Cosmos hub isn't counted.

### Edge Cases

* **Real-World Assets (RWA)**:\
  At present, we only track the tokenization of real-world assets (RWAs), such as bonds, treasuries, and real estate. Only tokenized representations of these assets, issued and traded on the blockchain, are counted towards TVL.

### Unproductive Assets

We are improving the Total Value Locked (TVL) metric by removing unproductive or artificial liquidity. Some assets are deposited only to earn rewards or boost metrics without taking real market risk or providing value to users. These positions often come from a small number of large wallets and do not support actual liquidity for the ecosystem. In some cases, TVL may also be inflated through circular or non-backed assets, making the data misleading. Our goal is to ensure TVL reflects real economic activity and remains transparent for all users.

While we are not placing blame on any team or chain, there may be instances where whales or large liquidity providers attempt to farm incentives without taking meaningful risks. We have also observed cases where unproductive assets are included in TVL through undisclosed arrangements or recycled liquidity. We understand that some projects may have adopted these practices to remain competitive or support growth during early stages. However, these positions can distort the perception of real usage and user participation.

There are common cases to be considered:

Liquidity pools with a few providers and no trading activities.

* Liquidity pools with a few providers and no trading activities.
* Assets deposited into lending pools with a few lenders and no borrowers.
* Assets deposited into yield/staking pools from a few depositors only to earn points or rewards.
* Wrapper assets that lack verified or provable backing.
* Assets without real user deposits or that can no longer be withdrawn by users.

Our intention is not to call out or discredit any individual protocol. Instead, we want to give all teams and chains the opportunity to align with a more accurate and honest standard. By excluding these unproductive assets, we ensure that TVL remains a reliable and trustworthy metric. This supports meaningful liquidity, real user engagement, and a more representative measurement of value across DeFi.

Our commitment is to provide the community with accurate data, improve transparency, and ensure fair representation for all participants in the ecosystem.
