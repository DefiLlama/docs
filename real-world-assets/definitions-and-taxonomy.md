# Definitions & Taxonomy

## Column Definitions

These are the canonical meanings for columns used on the RWA Dashboard.

- **Ticker**: Canonical ticker symbol assigned to the asset or platform.
- **Name**: Human-readable name of the asset or platform.
- **Website**: Official website for the issuer, asset, or platform.
- **Twitter**: Official social account for the issuer, asset, or platform.
- **Primary Chain**: Primary blockchain used for the asset.
- **Chain**: All supported chains for the asset, if applicable.
- **Contracts**: Contract addresses for the asset across supported chains.
- **Category**: High-level category grouping for the asset or platform.
- **Asset Class**: Specific asset class within the category.
- **Type**: Whether the row represents an Asset, Platform, or Wrapper.
- **RWA Classification**: High-level classification describing the economic nature of the entry.
- **Access Model**: Indicates how users interact with the asset.
- **Issuer**: Legal issuer entity or responsible party.
- **Issuer Source Link**: Primary source link supporting issuer and product claims.
- **Issuer Registry Info**: Registry details for the issuer, including addresses, and custodian notes when applicable.
- **ISIN**: International Securities Identification Number, if applicable.
- **Attestation Links**: Links to proof-of-reserves, attestations, audits, or reserve reports.
- **Attestations**: Whether the asset has attestations.
- **Redeemable**: Whether the asset can be redeemed for the underlying or a cash equivalent per documented terms.
- **CEX Listed**: Whether the asset is available to trade on a centralized exchange.
- **KYC for mint/redeem**: Whether issuer minting and/or redemption requires identity verification.
- **KYC/Allowlisted/Whitelisted to Transfer/Hold**: Whether holding or transferring requires allowlisting or whitelisting.
- **Transferable**: Whether the token can be transferred onchain.
- **Self Custody**: Whether users can hold the token in a self-custody wallet.
- **Description/Notes**: Summary of structure, backing, mechanics, and constraints.
- **Parent Platform**: Parent platform grouping when an asset is issued or managed under a broader platform.

## Type Taxonomy

Use these as the standardized **Type** values on the RWA Dashboard.

- **Asset**: Primary token representing real-world asset exposure being tracked, not the platform enabling issuance, and not a wrapper of another tracked asset.
- **Platform**: Protocol, issuer, or infrastructure entry that enables issuance, custody, trading, or management of RWAs, but is not the RWA exposure token itself.
- **Wrapper**: Token that wraps, pools, tranches, or strategies one or more underlying assets, creating RWA exposure via a derivative or structured product.

## Category Taxonomy

Use these as the standardized **Category** values on the RWA Dashboard.

- **Carbon Credits & Environmental Assets**: Tokenized carbon credits and other environmental assets, including avoidance and removal instruments.
- **Fiat-Backed Stablecoins**: Stablecoins designed to track a fiat currency, backed mainly by cash, deposits, and short-dated government securities.
- **Governance & Protocol Tokens**: Tokens used to govern RWA-related protocols. They do not represent direct claims on RWAs.
- **Non-RWA Stablecoins**: Stablecoins targeting a peg using onchain collateral or mechanisms, rather than direct backing by real-world assets.
- **Stablecoins backed by RWAs**: Fiat-pegged stablecoins whose primary backing is RWAs, commonly tokenized Treasuries, credit instruments, or other yield-bearing RWAs.
- **Tokenized Commodities**: Tokens representing commodity exposure, including precious metals, energy, industrial metals, and related baskets.
- **Tokenized Credit & Private Debt**: Tokens representing loans, receivables, invoices, and other credit instruments backed by off-chain cashflows.
- **Tokenized Equities & Public Securities**: Tokens representing exposure to publicly traded equities, ETFs, and other listed securities.
- **Tokenized Funds**: Tokenized shares or units of investment funds, including money market funds, Treasury funds, credit funds, and diversified RWA funds.
- **Tokenized Real Estate**: Tokens representing real estate interests, fractional ownership, or revenue participation linked to property assets.
- **Tokenized Treasury & Bonds**: Tokens representing government debt instruments or bond exposure, including Treasury bills and sovereign bonds.
- **Tokenized Alternative Assets**: Tokens representing alternative assets that do not fit other categories, such as collectibles and royalties.
- **RWA Marketplaces & Infrastructure**: Platforms and infrastructure providers enabling tokenization, issuance, compliance, settlement, and secondary markets for RWAs.

## Asset Class Taxonomy

Use these values to standardize **Asset Class** within each Category.

### Fiat-Backed Stablecoins

- **USD fiat stablecoin**: USD-pegged stablecoin backed mainly by cash, bank deposits, and short-dated government securities.
- **EUR fiat stablecoin**: EUR-pegged stablecoin backed mainly by cash, bank deposits, and short-dated government securities.
- **Other fiat stablecoin**: Fiat-pegged stablecoin (non-USD, non-EUR) backed mainly by cash, deposits, and short-dated government securities.
- **Bank deposit token**: Token representing a claim on bank deposits, or a tokenized bank deposit product, with bank-held backing.
- **Yield-bearing fiat stablecoin**: Fiat-pegged stablecoin designed to pass through yield from reserves to holders, directly or indirectly.

### Stablecoins backed by RWAs

- **Treasury-backed stablecoin**: Fiat-pegged stablecoin backed primarily by Treasury bills or other short-dated sovereign debt.
- **Credit-backed stablecoin**: Fiat-pegged stablecoin backed primarily by credit instruments such as private credit, loans, or structured credit.
- **Mixed RWA-backed stablecoin**: Fiat-pegged stablecoin backed by a diversified mix of RWAs, typically including both Treasuries and credit.

### Non-RWA Stablecoins

- **Crypto-backed stablecoin**: Stablecoin backed primarily by onchain crypto collateral, often overcollateralized.
- **Algorithmic stablecoin**: Stablecoin that targets a peg using algorithmic mechanisms rather than direct reserve backing.
- **Synthetic dollar**: Dollar-pegged exposure created through derivatives, hedging, or synthetic mechanisms, rather than direct reserves.

### Tokenized Treasury & Bonds

- **Tokenized Treasury bills**: Token representing exposure to short-dated government bills, typically Treasury bills.
- **Tokenized sovereign bonds**: Token representing exposure to sovereign bonds beyond bills, with longer duration risk.
- **Tokenized corporate bonds**: Token representing exposure to bonds issued by corporations.
- **Bond fund**: Tokenized fund share primarily holding bonds (sovereign, corporate, or mixed), rather than a single bond exposure.
- **Treasury fund**: Tokenized fund share primarily holding Treasury bills, Treasury notes, or similar sovereign debt instruments.

### Tokenized Funds

- **Money market fund**: Tokenized fund share targeting capital preservation and liquidity through cash-equivalents and short-dated instruments.
- **Treasury fund**: Tokenized fund share primarily holding Treasury bills, Treasury notes, or similar sovereign debt instruments.
- **Credit fund**: Tokenized fund share primarily holding credit instruments (private credit, structured credit, corporate credit, or similar).
- **Mixed RWA fund**: Tokenized fund share holding a diversified mix of RWAs across multiple asset types.
- **Tokenized crypto fund**: Tokenized fund share providing managed exposure to crypto assets through a fund structure.

### Tokenized Credit & Private Debt

- **Private credit token**: Token representing exposure to private credit arrangements, including loans and credit facilities.
- **Invoice or receivables token**: Token representing exposure to invoices, receivables, or similar short-dated cashflow claims.
- **Real estate debt token**: Token representing exposure to debt secured by real estate, including mortgages and property-backed loans.
- **Consumer debt token**: Token representing exposure to consumer credit, such as personal loans or similar consumer receivables.
- **SME or business debt token**: Token representing exposure to small and medium enterprise debt, or broader business lending.

### Tokenized Real Estate

- **Fractional real estate token**: Token representing a fractional interest in real estate ownership, or an equivalent ownership-linked structure.
- **Real estate revenue share**: Token representing participation in property-linked revenues (rent, cashflows, or profit participation) without direct title ownership.
- **Tokenized REIT exposure**: Token representing exposure to REIT-like structures or portfolios of real estate securities.

### Tokenized Equities & Public Securities

- **Tokenized equity**: Token representing economic exposure to a publicly traded equity.
- **Tokenized ETF**: Token representing economic exposure to an exchange-traded fund, or ETF-like basket.
- **Tokenized index exposure**: Token representing economic exposure to an index or index-tracking basket of public securities.

### Tokenized Commodities

- **Allocated gold token**: Gold exposure where specific allocated bars are held, with identified custody and documented allocation.
- **Unallocated gold token**: Gold exposure backed by unallocated gold claims, or pooled metal exposures without bar-level allocation.
- **Silver token**: Token representing economic exposure to silver.
- **Platinum token**: Token representing economic exposure to platinum.
- **Palladium token**: Token representing economic exposure to palladium.
- **Energy commodity token**: Token representing economic exposure to energy commodities (oil, gas, or similar).
- **Industrial metals token**: Token representing economic exposure to industrial metals (copper, aluminum, and similar).
- **Commodity basket token**: Token representing exposure to a basket of multiple commodities.

### Carbon Credits & Environmental Assets

- **Carbon credit token**: Token representing carbon credits, typically tied to a specific standard, registry, or project type.
- **Carbon pool token**: Token representing pooled carbon credit exposure across multiple credit types or projects.
- **Removal credit token**: Token representing credits tied specifically to carbon removal activities, rather than avoidance.

### Tokenized Alternative Assets

- **Collectibles token**: Token representing exposure to collectibles or similar non-traditional assets (art, trading cards, and similar).
- **Royalties token**: Token representing exposure to royalty streams or revenue participation rights.
- **Other alternative asset token**: Token representing alternative assets that do not fit other defined asset classes.

### Governance & Protocol Tokens

- **Governance token**: Token that primarily confers governance rights over a protocol or platform.
- **Utility token**: Token that primarily provides utility within a protocol or platform, rather than representing an RWA claim.

### RWA Marketplaces & Infrastructure

- **Tokenization platform**: Platform providing issuance and lifecycle management for tokenized RWAs.
- **RWA marketplace**: Platform focused on discovery, trading, and distribution of tokenized RWAs.
- **Compliance and identity rails**: Infrastructure supporting compliance, identity, allowlisting, transfer controls, and related requirements.
- **Custody and settlement rails**: Infrastructure providing custody, settlement, and asset servicing for tokenized RWAs.
