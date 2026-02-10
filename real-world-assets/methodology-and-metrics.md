# Methodology & Metrics

## Evidence Flags

These fields are treated as factual signals and are recorded as **Yes** or **x**:

- Attestations
- Redeemable
- CEX Listed
- KYC for mint/redeem
- KYC/Allowlisted/Whitelisted to Transfer/Hold
- Transferable
- Self Custody

### How to read Yes and x

- **Yes** means the field is publicly verifiable as Yes.
- **x** means not publicly verifiable as Yes (this includes cases where the answer is No, and cases where it is Unknown due to missing disclosure).

Frontend display can show Yes explicitly, and can treat x as blank, or "Not publicly verifiable," depending on design.

### Evidence Flag Definitions

#### Attestations

- **Yes** if there is a public proof source from the issuer showing reserves or underlying assets (attestation report, audit report, proof-of-reserves report, or similar), with clear scope and a date or update cadence.
- **x** if no such public proof source is available.

#### Redeemable

- **Yes** if holders have a documented way to redeem the token for the underlying asset or for a cash equivalent, either directly with the issuer, or through a clearly documented redemption process.
- **x** if redemption is not offered, not documented, or purely discretionary.

#### Transferable

- **Yes** if the token can be transferred onchain between wallet addresses under normal use, including cases where transfers are restricted to allowlisted wallets.
- **x** if transfers are blocked, or not practically possible (for example, non-transferable tokens, internal ledger entries, or tokens locked to one wallet).

#### Self Custody

- **Yes** if a user can hold the token in a self-custody wallet (user-controlled address or user-controlled multisig), not only through a custodian or platform account.
- **x** if the token must be held through an authorized custodian, broker, or platform account, with no self-custody option.

#### KYC for mint/redeem

- **Yes** if minting or redeeming through the issuer's primary process requires onboarding with identity verification and anti-money-laundering checks.
- **x** if minting and redeeming are available without identity verification, or if no issuer mint or redeem process is documented.

#### KYC/Allowlisted/Whitelisted to Transfer/Hold

- **Yes** if holding or transferring requires being verified or allowlisted (for example, wallet-level whitelists, transfer restrictions, identity registries, or enforced verification for secondary transfers).
- **x** if any wallet can hold and transfer without being allowlisted.

#### CEX Listed

- **Yes** if the asset is available to trade on a centralized exchange.
- **x** if it is not available to trade on a centralized exchange.

**Note**: CEX Listed is informational and is not used for RWA Classification.

## Metrics

### Onchain Marketcap

**Definition**: Onchain Marketcap is the circulating onchain token supply multiplied by price, excluding burned tokens.

#### How it's computed

- **Supply**: total token supply minus burned tokens (and other provably non-circulating supply, if verifiable)
- **Price**: the reference market price used by DeFiLlama for that asset
- **Onchain Marketcap**: (circulating onchain supply) × (price)

**Implementation note**: This should be reproducible from onchain reads using an open adapter.

### Active Marketcap

**Definition**: Active Marketcap is the portion of Onchain Marketcap that reflects independent market ownership and economic risk, excluding issuer-controlled, custodian-controlled, or operational balances that do not represent market float.

#### What counts

- Tokens held by independent holders in self-custody wallets
- Tokens deployed in third-party DeFi contracts (lending, pools, vaults, and similar)
- Tokens held in exchange wallets tied to customer balances, where customers are exposed to price moves and can exit

#### What does not count

- Issuer, platform, and operational wallets (treasury, mint, redeem, distribution, and similar)
- Custody or omnibus wallets where balances are effectively book-entry and do not represent independent onchain float
- Clearly internal loops where tokens circulate among issuer-controlled or platform-controlled wallets without changing economic ownership

#### Simple economic impact test

**Question**: If this wallet were hacked, would it cause material economic impact because the attacker could meaningfully sell, deploy, or extract value?

- **Yes**: count it as Active
- **No**: treat it as Onchain, but not Active

### DeFi Active TVL

**Definition**: DeFi Active TVL is the value of an asset that is deposited, pooled, or otherwise used inside third-party DeFi protocols tracked by DeFiLlama.

#### What counts

- Balances held in third-party DeFi protocols (lending markets, AMMs, pools, vaults, and similar)
- Positions that are publicly readable onchain and supported by a DeFiLlama adapter

#### What does not count

- The issuing platform's own contracts used mainly for minting, redemption, custody, or internal operations
- Centralized exchange wallets
- Issuer-controlled wallets and contracts that do not represent third-party DeFi usage

**Note**: DeFi Active TVL is meant to reflect third-party DeFi usage, not total issuance, and not issuer-managed deployments.

## Access Model

Access Model describes how a user can hold and move the asset onchain.

### Allowed values

- Permissioned
- Permissionless
- Non-transferable
- Custodial Only
- Unknown

### Definitions

- **Permissioned**: Holding or transferring requires allowlisting, whitelisting, or enforced verification.
- **Permissionless**: Any wallet can hold and transfer, without enforced verification for holding or transfer.
- **Non-transferable**: Can be held, but cannot be transferred to other wallets.
- **Custodial Only**: Must be held through an authorized custodian or platform account, with no self-custody option.
- **Unknown**: Access requirements are not disclosed publicly.

### Derivation rules

Apply these rules in order, using KYC/Allowlisted/Whitelisted to Transfer/Hold, Transferable, and Self Custody:

1. If KYC/Allowlisted/Whitelisted to Transfer/Hold = Yes → **Permissioned**
2. Else if Transferable = Yes and Self Custody = Yes → **Permissionless**
3. Else if Transferable = x and Self Custody = Yes → **Non-transferable**
4. Else if Self Custody = x → **Custodial Only**
5. Else → **Unknown**

**Note**: KYC for mint/redeem affects primary issuance and redemption access. It does not determine whether secondary holding or transfer is permissioned.

## RWA Classification

RWA Classification describes what the entry economically represents.

### Allowed values

- RWA
- Programmable Finance
- Non-RWA (Platform)
- Non-RWA (Gov/Utility)

### Definitions

- **RWA**: A tokenized real-world asset that is transferable and self-custodial, with at least one of: redemption, or public reserve or asset verification.
- **Programmable Finance**: An onchain exposure product tied to RWAs, such as wrappers, vault shares, strategy or pool tokens, synthetics, or other structured exposures.
- **Non-RWA (Platform)**: A platform or infrastructure entry related to RWAs, rather than an asset or exposure instrument.
- **Non-RWA (Gov/Utility)**: A governance or utility token that is not an RWA, and not an RWA exposure instrument.

### Classification procedure

Entries already labeled **Non-RWA (Platform)** or **Non-RWA (Gov/Utility)** are not changed by the rules below.

1. If the token is a **Derived or Structured Token**, classify it as **Programmable Finance**.
2. Otherwise apply the **RWA rule**.

#### RWA rule

If Transferable = Yes and Self Custody = Yes and (Attestations = Yes or Redeemable = Yes) → **RWA**.
Else → **Programmable Finance**.

#### Derived or Structured Token test

Treat a token as Derived or Structured if it is any of the following:

- Wrapper or receipt token
- Vault share
- Strategy token
- Pool token (LP token, or tranche-like exposure)
- Synthetic exposure token
- Locked derivative (time-locked, escrowed, or staked variants of another token)

### RWA badge display

This is a display layer only. The underlying classification remains RWA.

- **Green RWA**: Attestations = Yes and Redeemable = Yes
- **Standard RWA**: Attestations = Yes or Redeemable = Yes
