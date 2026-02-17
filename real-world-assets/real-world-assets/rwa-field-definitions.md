---
description: >-
  This page explains the required fields for Real-World Asset (RWA) token
  submissions. Teams must provide accurate, official, and verifiable
  information.
---

# RWA Field Definitions

## Basic Information

### Logo

**Definition:**\
The official visual logo representing the token or project.

**What to provide:**

* Direct URL to the logo image file
* PNG or SVG format preferred
* Minimum resolution: 200×200 pixels
* Transparent background preferred
* Must be official project branding

**Purpose:**\
Used for display in dashboards, asset pages, and analytics interfaces.

**Example:**\
[https://icons.llamao.fi/icons/protocols/ondo-finance?w=48\&h=48](https://icons.llamao.fi/icons/protocols/ondo-finance?w=48\&h=48)

***

### Ticker

**Definition:**\
The unique trading symbol of the token.

**What to provide:**

* Short uppercase symbol
* Typically 3–8 characters
* Must match onchain token and exchange listings

**Example:**\
USYC

***

### Token Name

**Definition:**\
The full official name of the tokenized asset.

**What to provide:**

* Complete asset name as defined by issuer

**Example:**\
Ondo US Dollar Yield Token

***

### Website

**Definition:**\
The official website of the token or issuer.

**What to provide:**

* Direct homepage URL
* Must be issuer-controlled

**Example:**\
[https://ondo.finance](https://ondo.finance)

***

### Twitter / X Profile

**Definition:**\
Official Twitter/X account of the issuer or token project.

**What to provide:**

* Direct profile URL

**Example:**\
[https://x.com/ondofinance](https://x.com/ondofinance)

***

### Telegram or Discord

**Definition:**\
Official community or team communication channel.

**What to provide:**

* Telegram group/channel link OR Discord invite link
* Must be official and controlled by project team

**Purpose:**\
Used for verification, communication, and project legitimacy checks.

**Examples:**

Telegram:\
https://t.me/(projectname)

Discord:\
https://discord.gg/(projectname)

***

## Blockchain Information

### Primary Chain

**Definition:**\
The main blockchain where the canonical version of the token is issued.

**What to provide:**

* Single blockchain name

**Examples:**

* ethereum
* arbitrum
* solana
* polygon
* base

***

### Chain

**Definition:**\
All blockchains where the token exists.

**What to provide:**

* List of blockchains

**Example:**\
ethereum, arbitrum

***

### Contracts

**Definition:**\
The official smart contract address(es) of the token.

**What to provide:**

Chain-prefixed contract address format:

chain:contract\_address

**Example:**\
ethereum:0x1234567890abcdef1234567890abcdef12345678

***

### Internal Addresses

**Definition:**\
Addresses controlled by the issuer used for treasury, custody, minting, or asset management.

**What to provide:**

List of addresses with labels.

**Examples:**

ethereum:0xabc123 (Treasury)\
ethereum:0xdef456 (Mint Authority)\
ethereum:0xghi789 (Custodian)

**Purpose:**\
Used to distinguish issuer-controlled tokens from circulating supply.

***

## Asset Classification

### Category

**Definition:**\
The type of real-world asset being tokenized.

**What to provide:**\
Select the closest category representing the underlying asset.

**Examples:**

* US Treasuries
* Private Credit
* Corporate Bonds
* Real Estate
* Commodities
* Carbon Credits & Environmental Assets
* Equities
* Stablecoins (Fiat-backed)
* Structured Products
* Funds / ETFs

***

## Market Data

### Token Price&#x20;

**Definition:**\
The CoinGecko identifier used to fetch live token price and market data. If available

**What to provide:**

If listed on CoinGecko:\
Provide CoinGecko ID.

**Examples:**

usd-coin\
ondo-us-dollar-yield

If NOT listed:

Not listed

***

## Issuer Information

### Issuer

**Definition:**\
The legal entity responsible for issuing the tokenized asset.

**What to provide:**

Full legal entity name.

**Example:**\
SolBridge Energy Foundation

***

## Proof & Verification

### Attestation

**Definition:**\
Proof that the token is backed by real-world assets.

**What to provide:**

Public link to one of the following:

* Attestation report
* Audit report
* Custodian verification
* Proof of reserves

**Example:**\
Link to the attestation

If none:

None

***

## Exchange & Access

### CEX Listing

**Definition:**\
Indicates whether the token is listed on centralized exchanges.

**What to provide:**

TRUE or FALSE

If TRUE, also provide exchange names.

**Example:**\
TRUE – Listed on Coinbase, Kraken

***

### KYC to Mint

**Definition:**\
Indicates whether KYC verification is required to mint or redeem tokens.

**What to provide:**

TRUE or FALSE

***

### KYC Whitelist Required to Hold/Transfer

**Definition:**\
Indicates whether wallets must be approved to hold or transfer tokens.

**What to provide:**

TRUE or FALSE

***

## Transfer & Custody

### Transferable

**Definition:**\
Indicates whether tokens can be freely transferred between wallets.

**What to provide:**

TRUE or FALSE

***

### Self Custody

**Definition:**\
Indicates whether users can hold tokens in their own wallets.

If FALSE, tokens must remain with a custodian.

**What to provide:**

TRUE or FALSE
