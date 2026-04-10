# Methodology & Metrics

This page describes how DeFiLlama evaluates and measures the tokenized real-world assets tracked on the [RWA dashboard](https://defillama.com/rwa). It covers evidence flags, metric definitions, access model classification, and the RWA classification framework.

---

## Evidence Flags

Each asset is tagged with seven factual signals. A value of **Yes** means the claim has been publicly verified. A value of **x** covers both negative answers and cases that have not yet been verified.

| Flag | What it records |
| --- | --- |
| Attestations | Public proof-of-reserves or asset-verification reports from the issuer. |
| Redeemable | A documented redemption pathway allowing holders to redeem for underlying assets. |
| CEX Listed | Availability on at least one centralized exchange. |
| KYC for mint/redeem | Identity verification (KYC/KYB/onboarding) required to mint or redeem through the issuer. |
| KYC/Allowlisted/Whitelisted to Transfer/Hold | Prior verification or allowlisting required to hold or transfer the token, enforced at the token, contract, or venue level. |
| Transferable | The token can be transferred freely between onchain addresses. |
| Self Custody | The token can be held in a user-controlled wallet. |

---

## Metrics

### Onchain Marketcap

Total USD value of the asset that exists onchain across all tracked chains. Calculated by multiplying circulating supply (excluding burned tokens) by the reference price sourced via DeFiLlama.

### Active Marketcap

The portion of Onchain Marketcap that is taking real market risk in user or protocol hands. Active Marketcap isolates independent market ownership by including self-custody holdings and third-party DeFi positions, while excluding issuer wallets, custody omnibus accounts, and internal operational flows. The economic test is whether compromise of a given wallet would enable meaningful value extraction by a third party.

### DeFi Active TVL

The subset of Active Marketcap that is deployed in third-party DeFi protocols tracked by DeFiLlama. Issuer-managed contracts and exchange wallets are excluded.

### Utilization

The ratio of DeFi Active TVL to Onchain Marketcap, expressed as a percentage. This shows how much of the total onchain supply is actively being used in DeFi protocols.

---

## Access Model

The Access Model describes how users can hold and transfer the asset. Five categories are assigned based on a deterministic hierarchy that evaluates allowlist requirements first, then transferability and self-custody in sequence.

| Access Model | Meaning |
| --- | --- |
| Permissioned | Requires KYC/AML verification to hold or transfer the asset. |
| Permissionless | Open to any wallet address without identity verification. |
| Non-transferable | Holdable but locked to the original wallet; cannot be transferred to other addresses. |
| Custodial Only | Must be held through an authorized custodian; no self-custody option. |
| Unknown | Access requirements have not been disclosed. |

---

## RWA Classification

Every entry receives one of five classification labels that describe its economic nature. These labels are derived from the evidence flags and structural properties of the asset.

| Classification | Meaning |
| --- | --- |
| True RWA | Tokenized real-world asset that is transferable and self-custodial, with both documented redemption and reserve/asset verification. |
| RWA | Tokenized real-world asset that is transferable and self-custodial, with either documented redemption or reserve/asset verification. |
| Programmable Finance | RWA-linked onchain products such as wrappers, strategy/pool tokens, synthetics, or restricted fund tokens that are not transferable or self-custodial. |
| Non-RWA (Platform) | Platform or infrastructure item related to RWAs, not an asset token or RWA exposure instrument. |
| Non-RWA (Gov/Utility) | Governance or utility token that is not a tokenized real-world asset or RWA exposure instrument. |

A two-tier badge system is used on the dashboard. Green badges are awarded to assets that meet both attestation and redemption requirements. Standard badges are shown for assets that meet either condition.

---

## Dashboard Guide

The [RWA dashboard](https://defillama.com/rwa) is the primary interface for exploring tokenized real-world assets tracked by DeFiLlama. It is organized into the following sections.

### Top-Level Metrics

Four summary cards are displayed at the top of the page:

| Card | Description |
| --- | --- |
| Total RWA Active Mcap | Aggregate Active Marketcap across all tracked RWA assets. |
| Total RWA Onchain Mcap | Aggregate Onchain Marketcap across all tracked RWA assets. |
| DeFi Active TVL | Total value of RWA assets deployed in third-party DeFi protocols. |
| Total Asset Issuers | Count of distinct issuers across all tracked assets. |

### Navigation Tabs

The dashboard has five tabs: **Overview**, **Chains**, **Platforms**, **Asset Groups**, and **Categories**. The Overview tab shows the full asset list across all views. Chains, Platforms, Asset Groups, and Categories provide filtered and aggregated views by each dimension.

### Chain Filters

A row of chain pills below the tabs allows filtering all data by blockchain. Selecting a specific chain (e.g. Ethereum, Arbitrum, Base) restricts the metrics, chart, and table to assets deployed on that chain.

### Time Series Chart

An interactive chart sits below the metrics cards and can display Active Mcap, Onchain Mcap, or DeFi Active TVL over time. The chart type can be toggled (e.g. Time Series Chart) and data can be grouped by Asset Group. The chart is exportable as CSV or PNG.

### Assets Rankings Table

A searchable, sortable table lists every tracked asset. The default columns are: Name, Asset Group, Active Marketcap, Onchain Marketcap, DeFi Active TVL, Utilization, Category, Asset Class, Access Model, and Type. A column selector allows users to show or hide additional fields. The table can be exported as CSV.
