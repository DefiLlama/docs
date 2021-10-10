# What to include as TVL?

Generally, any asset that is held in one of the protocol's contracts can be considered as part of TVL, with two notable exceptions:

* Assets on pool2, that is, money that is providing liquidity to an AMM pool where one of the tokens is from the protocol (except on some cases where those assets are performing an active function such as being used as collateral).
* Non-crypto assets which are external to the blockchain, such as bonds or fiat currency. We don't consider the dollars stored on Tether's bank account as TVL, for example.

We also don't accept assets that your protocol generates and are locked into other protocols, as that's the later protocols TVL, not your project's. See [this](https://github.com/DefiLlama/DefiLlama-Adapters/pull/60#issuecomment-807045050) for rationale.
