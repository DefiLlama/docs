# What to include as TVL?

### TVL

Total value locked inside a platform / protocol's own contracts by users.

### TVL Filters&#x20;

We separate TVL into different types. This lets users decide what they do and do not want to include in the dashboard data. These types are:

* Staking - the platform's own tokens
* Pool2 - staked LP tokens where one side of the market is the platform's own governance token.
* Borrows - deposits borrowed from the platform&#x20;
* Vesting - Tokens that are not circulating or not issued yet. This mostly applies to vesting protocols where a token with 10M mcap and 1B FDV could have 500M locked, in these cases it makes no sense for TVL from that token to be 500M when mcap of it is only 10M
* Offers - funds that are approved for spending on a non-custodial platform, but not actually deposited into the platform contracts

### Not TVL

* Assets that aren't on the blockchain, such as bonds or fiat currency. We don't consider the dollars stored on Tether's bank account as TVL, for example.
* We also don't accept assets that your protocol generates and are locked into other protocols, as that's the later protocols TVL, not your project's. See [this](https://github.com/DefiLlama/DefiLlama-Adapters/pull/60#issuecomment-807045050) for rationale.
