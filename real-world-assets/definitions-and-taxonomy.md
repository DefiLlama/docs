# Definitions & Taxonomy

This page defines every column on the [RWA dashboard](https://defillama.com/rwa) and documents the full taxonomy used to classify assets by type, category, and asset class.

---

## Column Definitions

### Identifier Fields

| Column | Definition |
| --- | --- |
| Ticker | Canonical ticker symbol assigned to the asset or platform. |
| Name | Human-readable name of the asset or platform. |
| Website | Official website for the issuer, asset, or platform. |
| Twitter | Official Twitter/X profile for the project or issuer. |

### Blockchain Fields

| Column | Definition |
| --- | --- |
| Primary Chain | Single chain slug representing the chain where the asset or platform is primarily issued or anchored. |
| Chain | List of chain slugs separated by semicolons representing all chains where the asset exists or is deployed. |
| Contracts | List of canonical onchain contracts formatted as `chainslug:address`. Use `x` when the asset has no single canonical contract. |

### Classification Fields

| Column | Definition |
| --- | --- |
| Category | High-level economic bucket describing the type of RWA or financial product represented. See the Category Taxonomy below. |
| Asset Class | Granular economic type within a Category describing how the exposure is structured. See the Asset Class Taxonomy below. |
| Type | Structural designation for the entry: Asset, Platform, or Wrapper. See the Type Taxonomy below. |
| RWA Classification | High-level classification describing the economic nature of the entry. See [Methodology & Metrics](methodology-and-metrics.md). |
| Access Model | Indicates how users interact with the asset. See [Methodology & Metrics](methodology-and-metrics.md). |

### Issuer & Verification Fields

| Column | Definition |
| --- | --- |
| Issuer | Legal entity or organization responsible for issuing or controlling the asset. |
| Issuer Source Link | Primary public reference confirming issuer information. |
| Issuer Registry Info | Identifiers or registry data for the issuer from corporate or regulatory databases. |
| ISIN | International Securities Identification Number, where applicable. |
| Attestation Links | URLs to public proof-of-reserves or asset-verification reports. |
| Attestations | Whether the issuer provides periodic attestations or assurance reports regarding reserves or backing. |

### Evidence Flags

| Column | Definition |
| --- | --- |
| Redeemable | Whether the token can be redeemed for underlying assets. |
| CEX Listed | Whether the token is listed on a centralized exchange. |
| KYC for mint/redeem | Whether minting or redeeming through the issuer requires KYC/AML verification. |
| KYC/Allowlisted/Whitelisted to Transfer/Hold | Whether holding or transferring the token requires prior verification or allowlisting, enforced at the token, contract, or venue level. |
| Transferable | Whether the token can be transferred freely between onchain addresses. |
| Self Custody | Whether the token can be held in a user-controlled wallet. |

### Metric Fields

| Column | Definition |
| --- | --- |
| Onchain Marketcap | Total USD value of the asset that exists onchain across tracked chains. |
| Active Marketcap | Portion of Onchain Marketcap taking real market risk in user or protocol hands. |
| DeFi Active TVL | Subset of Active Marketcap deployed in third-party DeFi protocols tracked by DeFiLlama. |
| Utilization | Ratio of DeFi Active TVL to Onchain Marketcap, expressed as a percentage. |

### Other Fields

| Column | Definition |
| --- | --- |
| Parent Platform | The issuer or platform entry that this asset belongs to, when applicable. |
| Description/Notes | Free-text field with structural details or modeling context. |
| Yield Bearing | Whether the token passes through yield to holders. |
| Oracle Provider | The oracle service providing price or reserve data for the asset. |
| Oracle Proof Link | URL to the oracle feed or proof used for the asset. |
| Claimed TVL | Issuer-reported AUM or TVL in USD. |
| Derivatives | Notes describing derivative or wrapper instruments built on top of the asset. |

---

## Type Taxonomy

Every entry is assigned one of three structural types.

| Type | Definition |
| --- | --- |
| Asset | Primary token representing real-world asset exposure being tracked, not the platform enabling issuance, and not a wrapper of another tracked asset. |
| Platform | Protocol, issuer, or infrastructure entry that enables issuance, custody, trading, or management of RWAs, but is not the RWA exposure token itself. |
| Wrapper | Token that wraps, pools, tranches, or strategies one or more underlying assets, creating RWA exposure via a derivative or structured product. |

---

## Category Taxonomy

Each asset is assigned to one of the following high-level categories. Assets on the RWA Perps dashboard use the **RWA Perps** category; all other categories apply to the main RWA dashboard.

| Category | Definition |
| --- | --- |
| Bond & MMF Funds | Tokens representing shares in fund-like products invested in cash and fixed income, such as Treasury bills, bond funds, and money market funds. Holding the token is meant to mirror holding a slice of the underlying fund. |
| Carbon & Environment | Tokens representing carbon credits and other standardized environmental units, such as verified emissions reductions, emissions allowances, and similar environmental certificates. |
| Crypto Funds | Tokens representing shares in managed funds whose holdings are cryptoassets, such as index funds or crypto hedge funds. Fund-style exposure rather than direct ownership of a single coin or token. |
| ETFs | Tokenized ETFs providing onchain exposure to exchange-traded fund shares, either fully backed by custodied shares or via synthetic replication. |
| Fiat Stablecoins | Stablecoins designed to track a fiat currency such as USD or EUR. Typically issued and redeemed by an issuer and backed by off-chain reserves like cash, bank deposits, and short-term government bills. |
| Gold & Commodities | Tokens tied to commodities, most often vaulted precious metals like gold. Typically backed by physical inventory or a clear issuer claim, and may be redeemable (directly or via approved partners) for the underlying commodity. |
| Governance Tokens | Tokens used to govern an RWA-related protocol. They usually give voting rights and sometimes a share of fees, but are not a direct claim on a specific off-chain asset like a Treasury bill, loan, or commodity. |
| Non-RWA Stablecoins | Stablecoins that aim to hold a peg using onchain mechanisms, such as crypto collateral, algorithms, or hedging, rather than cash and real-world assets held off-chain. |
| Other RWAs | Tokens tied to less common real-world assets such as art, collectibles, royalties, or intellectual property, plus platforms that issue or enable trading of them. |
| Private Credit | Protocols that finance real-world borrowing and receivables, for example private credit, trade finance, real estate credit, structured credit, or reinsurance-linked credit. Includes pools, originators, and markets that package or distribute credit onchain. |
| Real Estate | Tokens giving exposure to real estate, either a single property or a pool. Structures include fractional ownership, revenue sharing, or real-estate-backed debt. |
| RWA Perps | Perpetual markets tied to real-world reference assets such as public equities, ETFs, equity indices, bonds, and commodities. These markets provide margined, cash-settled price exposure rather than direct ownership or redemption rights in the underlying asset. Includes both individual perp markets and the venues that list them. |
| RWA Stablecoins | Fiat-pegged stablecoins where the main backing is real-world assets, for example Treasury bills, bonds, credit, or commodities. Redemptions depend on those assets' value and liquidity, and some versions pass through yield. |
| RWA Wrappers | Products that wrap RWA tokens (or RWA-backed stablecoins) into a simpler onchain position. They often issue a receipt token and may bundle assets, standardize yields, or split and repackage returns. |
| Stocks & Equities | Tokens that provide exposure to stocks or equity baskets. Exposure may be backed 1:1 by shares held by a custodian, or track prices synthetically. This category also includes platforms that issue and trade them. |

---

## Asset Class Taxonomy

Each asset is further classified into a granular asset class within its category. The tables below are organized by category.

### Fiat Stablecoins

| Asset Class | Definition |
| --- | --- |
| USD Stable | Stablecoins pegged to USD and backed mainly by cash, bank deposits, and short-term government paper held by a centralized issuer or custodian. |
| EUR Stable | Stablecoins pegged to EUR and backed mainly by euro cash, bank deposits, and short-term government paper. |
| Other Fiat Stable | Stablecoins pegged to non-USD, non-EUR fiat currencies such as GBP, CHF, SGD, or baskets of fiat. |
| Bank Deposit Token | Tokens that represent a direct, redeemable claim on a bank or central bank deposit account at par value. |
| Yield Fiat Stable | Fiat-pegged stablecoins that pass through some or all of the yield on reserve assets directly to token holders. |

### RWA Stablecoins

| Asset Class | Definition |
| --- | --- |
| RWA Fiat Stable | Fiat-pegged stablecoins whose reserves are primarily real-world assets such as Treasury bills, bonds, or commodity holdings (e.g. gold), but where yield is retained by the issuer rather than paid directly to token holders. |
| Yield RWA Stable | Fiat-pegged stablecoins whose reserves are real-world asset portfolios and where yield is paid directly to token holders via an increasing balance or rebase. |
| Multi-Asset RWA Stable | Stablecoins backed by a mix of real-world assets and onchain collateral, where neither side is clearly dominant. |

### Non-RWA Stablecoins

| Asset Class | Definition |
| --- | --- |
| Crypto Stable | Stablecoins backed primarily by onchain crypto collateral (e.g. ETH, stETH, LP tokens) with no material allocation to real-world assets. |
| Algo Stable | Stablecoins that rely mainly on algorithmic mechanisms or undercollateralized backing rather than fully reserved collateral. |
| Synthetic Stable | Stablecoins backed by synthetic crypto or derivatives strategies (such as delta-neutral basis trades or perp hedging) rather than direct fiat or real-world asset reserves. |

### Bond & MMF Funds

| Asset Class | Definition |
| --- | --- |
| T-Bills | Tokens that give direct or near-direct exposure to short-term government Treasury bills or ETFs that hold them. |
| Sovereign Bond Fund | Tokenized vehicles that hold government notes and bonds with longer duration than T-bills. |
| Corp Bond Fund | Tokenized funds that hold corporate bonds, securitized credit, or similar corporate fixed-income instruments. |
| MMF | Tokenized shares in a money market fund that holds short-duration, high-quality money market instruments. |
| Mixed RWA Fund | Tokenized funds that hold a planned mix of sovereign debt, credit, and cash-like assets where no single type dominates. |

### Gold & Commodities

| Asset Class | Definition |
| --- | --- |
| Gold | Tokens backed by physical gold, whether allocated to specific bars or held in pooled, unallocated form by a custodian. |
| Silver | Tokens backed primarily by physical silver held in secure custody. |
| Platinum | Tokens backed primarily by physical platinum. |
| Palladium | Tokens backed primarily by physical palladium. |
| Uranium | Tokens backed primarily by uranium or uranium inventory. |
| Other Commodity | Tokens backed by a single physical commodity that is not a precious metal or uranium, such as crude oil, natural gas, agricultural products, or metals outside the precious metals category. |
| Commodity Basket | Tokens backed by a basket of commodities rather than a single underlying asset. |

### Real Estate

| Asset Class | Definition |
| --- | --- |
| Single Property | Tokens that give equity-style exposure to a single property or real estate project. |
| REIT | Tokens that mirror units in a pooled real estate fund, REIT structure, or diversified basket/index of real estate exposures. |
| RE Revenue Share | Tokens whose primary claim is to a share of rental income or other property revenue streams. |
| RE Credit | Tokens backed primarily by loans, mortgages, or credit notes secured against real estate or property. |

### Stocks & Equities

| Asset Class | Definition |
| --- | --- |
| Stock Synthetic | Tokens that track a single listed equity using synthetic replication rather than full custody of shares. |
| Stock Backed | Tokens backed by held underlying shares in custody that map 1:1 to a single listed equity. |
| Equity Basket | Tokens that track a basket or index of equities or equity ETFs. |
| PE / Venture | Tokens that represent interests in private company equity, venture capital funds, or similar vehicles. |

### Private Credit

| Asset Class | Definition |
| --- | --- |
| Credit Pool | Pools or funds backed by diversified private loans such as SME, corporate, or consumer credit, including managed private credit funds and feeder vehicles that allocate across lending strategies. |
| Trade Finance | Pools backed by invoices, receivables, or other trade-finance assets. |
| Structured Credit | Credit backed by niche or structured assets such as revenue-based finance, litigation finance, project finance, or asset-backed fleet financing. |
| Other Credit | Private credit pools that are clearly real-world lending exposures but do not fit the more specific labels. |
| Reinsurance Pool | Pools where the primary risk and return come from reinsurance or insurance underwriting exposure. |
| Lending Venue | Protocols or markets that primarily facilitate issuance, trading, or lending against RWA collateral or private credit pools, rather than representing a single pool token. |

### RWA Wrappers

| Asset Class | Definition |
| --- | --- |
| Stable Yield Wrapper | Wrappers whose underlying is one or more stablecoins and that issue a yield-bearing receipt token. |
| Single Asset Wrapper | Wrappers that mainly hold one underlying RWA token or fund and issue a single receipt token. |
| RWA Basket Vault | Wrappers that hold a basket of RWA tokens or funds and present them as one composite position or index-like exposure. |
| RWA LP Wrapper | Tokens that represent LP positions where the majority of exposure is to RWA tokens or RWA stablecoins. |
| Leveraged RWA Vault | Wrappers that apply leverage or active rebalancing on top of RWA collateral to modify yield or duration. |
| Structured Note | Tokens that embed structured payouts or note-like features on top of underlying RWA collateral. |
| Yield Pool | Tokens that directly represent a yield-generating RWA pool that is not positioned as a stablecoin. |
| Active Vault | Vault tokens representing deposits into a discretionary, manager-directed strategy that may allocate across RWAs and other onchain yield sources, and that issue a receipt token. |

### Carbon & Environment

| Asset Class | Definition |
| --- | --- |
| Carbon Credit | Tokens representing carbon credits, including project-based voluntary offsets and allowances from regulated cap-and-trade or compliance carbon markets. |
| Carbon Reserve | Tokens that function as reserve or treasury assets and are backed by baskets of carbon credits or similar environmental units, rather than representing individual retireable offset claims. |
| Other Environmental | Tokens linked to other environmental or impact units such as renewable energy certificates or biodiversity credits. |

### Governance Tokens

| Asset Class | Definition |
| --- | --- |
| Governance Token | Tokens that confer governance rights, voting power, or protocol control over an RWA platform or product suite, without being a direct 1:1 claim on the underlying assets. |
| Revenue Share | Tokens that entitle holders to a share of protocol revenues or fees from an RWA platform while not being a direct claim on underlying asset pools. |

### Other RWAs

| Asset Class | Definition |
| --- | --- |
| Collectibles | Tokens backed by art, collectibles, or other non-financial real assets such as luxury goods. |
| IP / Royalty | Tokens whose primary claim is to intellectual property or revenue/royalties from IP such as music, patents, or media catalogs. |
| Other Exotic | Tokens backed by niche real-world assets that do not fit other categories, such as AI hardware, fleet assets, or other specialized equipment. |
| Domain Venue | Marketplaces/venues that fractionalize off-chain DNS domains into onchain tokens and facilitate primary issuance and secondary trading. |
| Collectibles Venue | Platforms/marketplaces that enable tokenization, trading, custody/vaulting, and redemption of physical collectibles via onchain tokens/NFTs. |

### Crypto Funds

| Asset Class | Definition |
| --- | --- |
| Crypto Index Fund | Tokenized fund shares providing beneficial exposure to a diversified basket of cryptocurrencies via a disclosed index methodology or systematic allocation strategy. |
| Crypto Hedge Fund | Tokenized fund shares providing beneficial exposure to an actively managed digital-asset fund pursuing absolute-return strategies across spot and/or derivatives markets. |

### ETFs

ETFs on the main RWA dashboard use the asset classes **ETF Backed** (tokens fully backed by custodied ETF shares) or share asset classes with Stocks & Equities where applicable.

### RWA Perps

Perpetual market asset classes are documented separately. See the [RWA Perps](rwa-perps.md) page for the full perps asset class taxonomy, column definitions, and dashboard guide.

| Asset Class | Definition |
| --- | --- |
| Stock Perp | Perpetual markets that track the price of an individual public company's equity. |
| ETF Perp | Perpetual markets that track the price of an exchange-traded fund. |
| Equity Perp | Perpetual markets that track a public-equity benchmark or basket. |
| Commodity Perp | Perpetual markets that track a commodity or commodity benchmark. |
| Bond Perp | Perpetual markets that track a bond or bond benchmark. |
| Perp Venue | Venues that list and operate perpetual markets tied to real-world reference assets. |

---

## Asset Group Taxonomy

Asset Groups are broader user-facing groupings used to pull related assets and markets into a single dashboard view. They appear in the Asset Group column on both the main RWA dashboard and the RWA Perps dashboard, and are used for chart grouping and the Asset Groups tab.

| Asset Group | Definition |
| --- | --- |
| Agricultural Commodities | Assets tied mainly to crops, farmland yield rights, livestock-related commodity flows, or other agricultural commodity exposure. |
| AI & Compute Infrastructure | Assets tied mainly to physical AI and compute infrastructure, such as GPUs, data centers, robotics equipment, tokenized compute capacity, or financing backed by those assets. |
| Bonds | Assets tied mainly to government or corporate debt instruments, money market funds, or similar fixed-income exposures where returns come mainly from interest and principal payments. |
| Carbon Credits | Assets tied mainly to carbon credits, emissions allowances, or other standardized environmental units used to represent avoided, reduced, or removed emissions. |
| Collectibles & Luxury Goods | Assets tied mainly to art, watches, wine, jewelry, rare cars, luxury goods, or similar collectible markets. |
| Commodity Baskets | Assets spanning multiple commodity types without one clearly dominant commodity. |
| Digital Assets | Assets tied mainly to cryptocurrencies, blockchain network activity, tokenized mining or hashrate, domain names, or investment funds whose reference assets are digital assets. |
| Energy | Assets linked mainly to power generation, electricity, fuel infrastructure, or other energy-sector cash flows that are not best described as oil or natural gas alone. |
| Equity ETFs | Assets whose primary reference is one or more exchange-traded funds that hold public equities. |
| Equity Indices | Assets tracking broad or thematic equity indexes rather than a single company or a specific ETF. |
| Fiat Currencies | Assets whose value is designed mainly to track a fiat currency or closely related cash unit, whether fully reserve-backed or maintained synthetically. |
| Industrial Metals | Assets tied mainly to industrial metals such as copper, aluminum, nickel, or similar materials used in manufacturing and infrastructure. |
| Intellectual Property & Royalties | Assets tied mainly to copyrights, music royalties, film rights, patents, trademarks, or other contractual cash flows from intellectual property. |
| Mixed Public Markets | Assets spanning multiple public-market reference types, such as equities, ETFs, indices, bonds, or commodities, without one clearly dominant sleeve. |
| Multi-Asset RWAs | Assets or platforms with meaningful exposure to multiple real-world asset sectors, such as bonds, credit, equities, real estate, or commodities, without one dominant reference asset. |
| Natural Gas | Assets tied mainly to natural gas benchmarks or gas-linked commodity exposures. |
| Oil | Assets tied mainly to crude oil benchmarks or related petroleum exposures. |
| Precious Metals | Assets tied mainly to gold, silver, platinum, palladium, or similar precious-metals exposure. |
| Private Credit | Assets tied mainly to privately originated loans, receivables, trade finance, structured credit, or similar non-public lending exposures. |
| Private Equity & Venture | Assets tied mainly to ownership stakes in private companies, venture investments, or platform equity that behaves more like private-company exposure than public-market shares. |
| Public Equities | Assets tied mainly to publicly listed companies or diversified baskets of listed shares. |
| Real Estate | Assets linked mainly to property values, rental income, home equity, or portfolios of real estate assets. |
| Reinsurance | Assets tied mainly to insurance-linked or reinsurance risk, where returns depend on underwriting performance, premium flows, and loss outcomes. |
| RWA Infrastructure | Tokens or platforms tied mainly to the issuance, trading, settlement, compliance, or governance rails used to bring real-world assets onchain, rather than a single underlying asset class. |
