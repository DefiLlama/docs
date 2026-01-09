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

* Liquidity pools with a few providers and no trading activities.
* Assets deposited into lending pools with a few lenders and no borrowers.
* Assets deposited into yield/staking pools from a few depositors only to earn points or rewards.
* Wrapper assets that lack verified or provable backing.
* Assets without real user deposits or that can no longer be withdrawn by users.

Our intention is not to call out or discredit any individual protocol. Instead, we want to give all teams and chains the opportunity to align with a more accurate and honest standard. By excluding these unproductive assets, we ensure that TVL remains a reliable and trustworthy metric. This supports meaningful liquidity, real user engagement, and a more representative measurement of value across DeFi.

Our commitment is to provide the community with accurate data, improve transparency, and ensure fair representation for all participants in the ecosystem.

### Unproductive Assets-Protocol List

#### Momentum

We removed TVL from unproductive liquidity pools, these pools have BTC wrapper assets with no trading activities:

* YBTC.B/satYBTC.B - 0x584e68589d2ce655c47fa88d75090258c0d8b5b16c3d643e0ddc2b71e81e6546
* MBTC/brBTC - 0xb64d0313a3a542e3d8f8066df767e2459d356377ca15ff003ef5e16629569b4d
* BTCvc/eBTCvc - 0x1d182ec3743611393c06e810662dcf70efcfd2e16d855dfdb4dee6abc1bf9ce0

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### DeSyn Liquid Strategy

We removed TVL from unproductive vaults. These vaults have only a few depositors, no users deposits. We removed vaults on these blockchains:

* BSC
* Mode
* Ethereum
* Plume
* Core
* Hemi
* B2 Network
* GOAT Network
* AILayer
* Bitlayer

#### Saros DLMM

We removed unproductive pools on Saros DLMM that have only a few liquidity providers with no trading activities:

* UNIBTC/XBTC - 7hc6hXjDPcFnhGBPBGTKUtViFsQuyWw8ph4ePHF1aTYG
* BFBTC/CBBTC - 9BJ1xTWSuSTqSpgh6fuCmsArHNVA2Fmu5PtDmC6kTAF6
* BFBTC/UNIBTC - HtyaKeqMTd9o289XKZWc8CSJTdp8xABgXrRDWw1wp96W
* BFBTC/WBTC - DfohHvQNtXdN7ZgB6AQxNZiqNm2u528mAhY8MTeNEcAd
* BFBTC/ZBTC - 66AFL6NKwwKkWr5bbvigviEcMzWCMXWhypch1KsQhoCH
* UNIBTC/WBTC - J5ix1fmNfLfpyYr5MDVw4U9dqqCNG8oVvrSkGF3cTwtB

#### Stream Finance

We removed looping/leverage assets from Stream Finance vaults.

There are wallets deposit some initial USDC, WBTC, ETH on ethereum to mint xUSD, xBTC, xETH , they bridge xUSD, xBTC, xETH to other chains, they used xUSD, xBTC, xETH as collateral to borrow USDT, USDC, deUSD, they use swap all borrowed tokens to USDC, WBTC, ETH, deposit back to xUSD, xBTC, xETH

We removed all bad debts from Stream Finance after their financial loss report: [https://x.com/StreamDefi/status/1985556360507822093](https://x.com/StreamDefi/status/1985556360507822093)

#### Cygnus

We delisted Cygnus from our website because we detected they mint and stake unbacking assets BTC and ETH.

#### unagiswap

We removed unproductive liquitiy pools that have a single liquidity provider with no users trading activities.

* USDa/sUSDa pool with 70M - 0x332df42cf1c2c4874edb62f1f498424d2c2e928a

#### Avalon Finance

We removed BTC wrapper assets with no borrow from lending pools.

* Merlin - WBTC
* Kaia - USDa
* BoB - SolvBTC, SolvBTC\_BBN
* Taiko - SolvBTC
* BSC - USDX, sUSDX

#### Sumer.money

We removed unproductive assets with no borrow from lending pools.

* Meter - suUSD, suETH
* Base - suUSD, suETH
* Arbitrum - suUSD, suETH, suBTC
* Ethereum - suUSD, suETH, suBTC
* Core - suUSD, suBTC, solvBTC.m
* Berachain - suUSD, suBTC
* Hemi - suUSD, suETH, suBTC, brBTC
* Bitlayer - suBTC
* GOAT Network - suETH, suBTC, enzoBTC
* ZkLink - suBTC, solvBTC.m, M-BTC, solvBTC.b
* B2 Network - suUSD, suBTC
* Monad - suUSD

#### Segment Finance

We removed satUSD token from lending pool which has no borrow on BoB chain.

#### VaultCraft

We removed TVL from unproductive vaults. These vaults have only a few depositors, no users deposits.

Ethereum:

* 0xcF9273BA04b875F94E4A9D8914bbD6b3C1f08EDb
* 0x77e88cA17A6D384DCBB13747F6767F30e3753e63
* 0xdB06a9D79f5Ff660f611234c963c255E03Cb5554

Base:

* 0x023577b99e8A59ac18454161EecD840Bd648D782

Hemi:

* 0x748973D83d499019840880f61B32F1f83B46f1A5
* 0x0b8E088a35879f30a4d63F686B10adAD9cB3DBE1

#### iZiSwap

We removed unproductive liquitiy pools that have a single liquidity provider with no users trading activities.

Hemi pools:

* bfBTC/hemiBTC - 0x469a5066578e22a1222cc78b2ccaca602db6bb4a
* bfBTC/hemiBTC - 0xFE1c507Be86F977B61d12D1DA3c95D0dEeB1B86A
* brBTC/suBTC - 0x98a3a18583138474aedd2ceec034cba1fa783613
* mBTC/uniBTC - 0xe9635693b7606f1914c0cd698065ec84267a62a1

Taiko pools:

* mBTC/uniBTC - 0x5e1e8c9c77b0de88f1c4597a3c145b0c7abcf485

#### Bluefin AMM

We removed TVL from unproductive assets in liquidity pools that have no users trading activities

* BTCvc-vBTCvc - 0x90811fd5409c14d29ec40f59bf7158c52dffa20bc11eb33873670addbf149aa2

#### Pell Network

We removed TVL from unproductive assets in vaults that have a few deposit addresses

BOB - SolvBTC Restaking vault, there is only one depositor [https://explorer.gobob.xyz/tx/0xf624ccb21311a92c1690e56a22235e28eced190ea39ab6d6ce18a51c56aeb671](https://explorer.gobob.xyz/tx/0xf624ccb21311a92c1690e56a22235e28eced190ea39ab6d6ce18a51c56aeb671)

Core - uBTC Restaking vault, there are a few depositors [https://scan.coredao.org/tx/0xbec633767d7606ef539640e65e0fe8a9cdc8c8d6c7dc404b286ab38d32abbfa1](https://scan.coredao.org/tx/0xbec633767d7606ef539640e65e0fe8a9cdc8c8d6c7dc404b286ab38d32abbfa1) [https://scan.coredao.org/tx/0x45a5258730fd80d20d2f52ef787e11204f07da2e557e39cae948b0b756671e86](https://scan.coredao.org/tx/0x45a5258730fd80d20d2f52ef787e11204f07da2e557e39cae948b0b756671e86) [https://scan.coredao.org/tx/0x95c32c817c501c36cdeb2d02b618c9296b664d462614eb92624f833110188d78](https://scan.coredao.org/tx/0x95c32c817c501c36cdeb2d02b618c9296b664d462614eb92624f833110188d78)

Zetachain - pumpBTC Restaking vault, there is only one depositor [https://zetascan.com/tx/0x2f77bb7e06096c1296c5079445cae6ef9e29a0b91a15fe2558bc90693a9f7e86](https://zetascan.com/tx/0x2f77bb7e06096c1296c5079445cae6ef9e29a0b91a15fe2558bc90693a9f7e86)

Bitlayer - suBTC vault has only one depositor [https://www.btrscan.com/tx/0x59a1596e4d43f0e31258a5dc5f53c7d3dd353a1ce887841e933724bf290e3c98](https://www.btrscan.com/tx/0x59a1596e4d43f0e31258a5dc5f53c7d3dd353a1ce887841e933724bf290e3c98) [https://www.btrscan.com/tx/0xa0154c09665070d28cfc484fe7c6358bf732ac894f64e0003029935542df4af1](https://www.btrscan.com/tx/0xa0154c09665070d28cfc484fe7c6358bf732ac894f64e0003029935542df4af1) [https://www.btrscan.com/tx/0xaec872b3d29f192c24ccb41d72428be2a95f00398e7c28b1a4125334ed3b4449](https://www.btrscan.com/tx/0xaec872b3d29f192c24ccb41d72428be2a95f00398e7c28b1a4125334ed3b4449)

Plume - esBTC vault has only one depositor [https://explorer.plume.org/tx/0x639ceb9257cf5e14d738c5253b586f2c3d24dbc5595ae9de63c07d6eef768a8a](https://explorer.plume.org/tx/0x639ceb9257cf5e14d738c5253b586f2c3d24dbc5595ae9de63c07d6eef768a8a) [https://explorer.plume.org/tx/0xa29388e964f7f7eac7f7c771073fd67edf1cbf0736bcd0532e6c86550dabd169](https://explorer.plume.org/tx/0xa29388e964f7f7eac7f7c771073fd67edf1cbf0736bcd0532e6c86550dabd169) [https://explorer.plume.org/tx/0x98938144405da515ab7f93b1d89b85b73a6bc2fc2cf5836fc82c156431057eec](https://explorer.plume.org/tx/0x98938144405da515ab7f93b1d89b85b73a6bc2fc2cf5836fc82c156431057eec) [https://explorer.plume.org/tx/0xa4988e9b29f3f81b0fe1725adeba66038e7023f51272f9bf368a6bdbbc159ed5](https://explorer.plume.org/tx/0xa4988e9b29f3f81b0fe1725adeba66038e7023f51272f9bf368a6bdbbc159ed5) [https://explorer.plume.org/tx/0x1208586ac1142cff780ce6d7b137fbb67396c97f8bcdef9f7cfb523901447d60](https://explorer.plume.org/tx/0x1208586ac1142cff780ce6d7b137fbb67396c97f8bcdef9f7cfb523901447d60) [https://explorer.plume.org/tx/0xf52b0e861cd71071ace96328a2f64d2bca3a808fe84f8cf403cae7e68172de57](https://explorer.plume.org/tx/0xf52b0e861cd71071ace96328a2f64d2bca3a808fe84f8cf403cae7e68172de57)

Plume - YBTC.B vault has only one depositor [https://explorer.plume.org/tx/0xa0ffd15e22d676a3b9727682b821d08db5b1c6f231317d40cf554477854eafb7](https://explorer.plume.org/tx/0xa0ffd15e22d676a3b9727682b821d08db5b1c6f231317d40cf554477854eafb7) [https://explorer.plume.org/tx/0xf4fb7f263d24d8f66a551143fa9978ac3d401d1187fea26b689092be62adf5da](https://explorer.plume.org/tx/0xf4fb7f263d24d8f66a551143fa9978ac3d401d1187fea26b689092be62adf5da) [https://explorer.plume.org/tx/0xb4a70f25662697c2657e29781d265e186efbbf40151799100473c09d65d6a7fc](https://explorer.plume.org/tx/0xb4a70f25662697c2657e29781d265e186efbbf40151799100473c09d65d6a7fc)

Goat - nETH vault has only one depositor [https://explorer.goat.network/tx/0xe302ec93a029fc503b22d8e62cfa6e3ea8bed46bfb3881e6c733b2e0e90b68f8](https://explorer.goat.network/tx/0xe302ec93a029fc503b22d8e62cfa6e3ea8bed46bfb3881e6c733b2e0e90b68f8)

Goat - rnETH vault has only one depositor [https://explorer.goat.network/tx/0xbb25df627ab1c8853d0ba0ff725229a4a3372bb05638c95093f319947104d28e](https://explorer.goat.network/tx/0xbb25df627ab1c8853d0ba0ff725229a4a3372bb05638c95093f319947104d28e)

Rootstock - uniBTC vault has only two deposits [https://explorer.rootstock.io/tx/0x662f1c457321b9935f7e53173543fc00601d3fe8cc67eb221021c618ebbaf327](https://explorer.rootstock.io/tx/0x662f1c457321b9935f7e53173543fc00601d3fe8cc67eb221021c618ebbaf327) [https://explorer.rootstock.io/tx/0x111fd08c24fc70bc3024f74107815030ee7f10c664584002a22c1b08310a38f5](https://explorer.rootstock.io/tx/0x111fd08c24fc70bc3024f74107815030ee7f10c664584002a22c1b08310a38f5)

#### Kodiak V3

We removed TVL from unproductive liquidity pools that have no trading activities

* SolvBTC.BNB/SolvBTC - 0x24619368bad314d1635a54027c5231b9b83c4a7e
* brBTC-uniBTC = 0xe9703de93406cc31441a57ce5d08272ed545d32b

#### RollX

We removed TVL from unproductive assets in vaults that have a few deposit addresses

Base - bfBTC vault, there is only a few depositors [https://etherscan.io/tx/0xce658f57edc490798ddc55e3f67169e3190fe23afe50a4ce4ca411d0714edada](https://basescan.org/address/0x623F2774d9f27B59bc6b954544487532CE79d9DF)

