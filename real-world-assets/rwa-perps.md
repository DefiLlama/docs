# RWA Perps

The [RWA Perps dashboard](https://defillama.com/rwa/perps) tracks perpetual futures markets tied to real-world reference assets. Unlike the main RWA dashboard, which tracks tokenized assets with onchain supply and market capitalization, the perps dashboard tracks derivative markets that provide margined, cash-settled price exposure to assets such as public equities, commodities, bonds, ETFs, and equity indices. These markets do not confer ownership of, or redemption rights in, the underlying real-world asset.

All entries on the RWA Perps dashboard belong to the **RWA Perps** category.

---

## Dashboard Guide

### Overview Tab

The Overview tab is the default view and provides a market-level picture of all tracked RWA perpetual contracts.

#### Top-Level Metrics

Four summary cards appear at the top of the page:

| Card | Description |
| --- | --- |
| Total Open Interest | Aggregate USD notional of all outstanding perpetual contracts across all tracked venues. Shown with a 24-hour percentage change. |
| Total 24h Volume | Aggregate USD notional volume traded across all tracked markets in the last 24 hours. Shown with a 24-hour percentage change. |
| Total Markets | Count of individual perpetual markets being tracked. |
| Est. Protocol Fees 24h | Estimated fees earned by all tracked protocols from trading activity in the last 24 hours. |

#### Visualization

Below the metrics cards is an interactive visualization that can be toggled between three views using the tabs:

- **Open Interest** — a treemap or chart showing the distribution of open interest across markets, grouped by Asset Group or Base Asset.
- **Volume** — the same visualization using 24-hour trading volume instead of open interest.
- **Markets** — a count-based view showing how many markets exist per group.

The visualization supports multiple chart types (Treemap Chart is the default), can be nested by Base Asset or Asset Group, and is exportable as CSV or PNG. Hovering over a segment shows the parent group, child asset, open interest or volume value, share of parent, and share of total.

#### Markets Rankings Table

A searchable, sortable table lists every tracked perpetual contract. The table has a configurable column selector and can be exported as CSV. Default columns are:

| Column | Description |
| --- | --- |
| Contract | Ticker symbol of the perpetual futures contract as listed on the venue. |
| Venue | Protocol or exchange where the perpetual contract is listed and traded. |
| Base Asset | Underlying real-world asset the perpetual contract tracks (e.g. Crude Oil (WTI), Tesla, Gold). |
| Asset Group | Broader grouping of the base asset used for dashboard aggregation (e.g. Oil, Public Equities, Precious Metals). |
| Asset Class | High-level economic category of the underlying asset. See the Asset Class Taxonomy below. |
| Open Interest | Total USD value of outstanding perpetual contracts that have not been settled or closed. |
| 24h Volume | Total USD notional volume traded on this contract in the last 24 hours. |
| Price | Latest traded price of the perpetual contract in USD. |
| 24h Price Change | Percentage change in contract price over the last 24 hours. |
| Latest Funding Rate | Most recent periodic funding rate paid between long and short position holders. Positive means longs pay shorts. |
| Premium | Percentage difference between the perpetual contract price and the underlying asset's spot or index price. |
| Max Leverage | Maximum leverage multiplier available for this contract on the venue. |

Additional columns available via the column selector include:

| Column | Description |
| --- | --- |
| Issuer | Entity responsible for issuing or managing the underlying real-world asset. |
| Access Model | Whether trading the contract requires KYC, permissioning, or is permissionlessly accessible. |
| Volume 30d | Total USD notional volume traded on this contract over the last 30 days. |
| Est. Protocol Fees 24h | Estimated fees earned by the protocol from trading activity on this contract in the last 24 hours. |

### Venues Tab

The Venues tab provides a venue-level view, aggregating market data across all contracts listed on each protocol or exchange.

#### Chain Filters

A row of venue pills allows filtering by specific venue (e.g. km, vntl, xyz). Selecting "All" shows aggregated data across all venues.

#### Venue Chart

An interactive stacked area chart shows historical open interest, volume, or market counts per venue over time. The chart can be filtered by Base Asset and is exportable as CSV or PNG.

#### Venues Table

A table below the chart lists each venue with the following columns:

| Column | Description |
| --- | --- |
| Venue | Name of the protocol or exchange. |
| Open Interest | Total USD open interest across all markets on the venue. |
| % of Total OI | Venue's share of total open interest across all tracked venues. |
| 24h Volume | Total USD volume across all markets on the venue in the last 24 hours. |
| % of Total 24h Volume | Venue's share of total 24-hour volume. |
| Markets | Count of individual perpetual markets listed on the venue. |

---

## Column Definitions

The tables below document every column on the RWA Perps dashboard. For columns shared with the main RWA dashboard (such as Access Model and Issuer), see [Definitions & Taxonomy](definitions-and-taxonomy.md).

### Market Identification

| Column | Definition |
| --- | --- |
| Contract | Ticker symbol of the perpetual futures contract as listed on the venue. |
| Venue | Protocol or exchange where the perpetual contract is listed and traded. |
| Base Asset | Underlying real-world asset the perpetual contract tracks (e.g. Crude Oil (WTI), Tesla, Gold). |
| Asset Group | Grouping of the base asset by product family, used for dashboard aggregation. |
| Asset Class | High-level economic category of the underlying asset, such as commodities, equities, or fixed income. See the Asset Class Taxonomy below. |
| Canonical Market ID | Venue-qualified market identifier used when the entry is a market rather than a token contract, such as `xyz:GOLD` or `cash:GOLD-USDT`. This is the canonical internal market slug, not the frontend trading pair. |
| Pair | Displayed trading pair or quote convention for perpetual markets, such as GOLD-USDC or TSLA-USDT. Use `x` when the row is not a market or when no pair is applicable. |
| Margin / Settlement Asset | Frontend-readable market quote suffix or settlement unit, such as USDC, USDT, or USDH. This shows the margin and cash-settlement asset used by the market rather than the tracked underlying. |
| Reference Asset | Normalized exact asset, benchmark, ETF, company, or commodity tracked by the market. Used for exact cross-venue alignment and exact-asset dashboard pages. Examples: Gold, Silver, S&P 500, Tesla, Crude Oil (WTI). |
| Reference Asset Group | Broader user-facing grouping used to pull related markets into a single dashboard view across venues. Examples: Oil, Precious Metals, Public Equities, Equity Indices. See the Asset Group Taxonomy in [Definitions & Taxonomy](definitions-and-taxonomy.md). |

### Market Metrics

| Column | Definition |
| --- | --- |
| Open Interest | Total USD value of outstanding perpetual contracts that have not been settled or closed. This replaces Onchain Marketcap as the primary sizing metric for perps. |
| 24h Volume | Total USD notional volume traded on the contract in the last 24 hours. |
| Price | Latest traded price of the perpetual contract in USD. |
| 24h Price Change | Percentage change in contract price over the last 24 hours. |
| Latest Funding Rate | Most recent periodic funding rate paid between long and short position holders. A positive rate means longs pay shorts; negative means shorts pay longs. |
| Premium | Percentage difference between the perpetual contract price and the underlying asset's spot or index price. A positive premium means the perp trades above spot. |
| Max Leverage | Maximum leverage multiplier available for the contract on the venue. |
| Volume 30d | Total USD notional volume traded on the contract over the last 30 days. |
| Est. Protocol Fees 24h | Estimated fees earned by the protocol from trading activity on this contract in the last 24 hours. |

---

## Asset Class Taxonomy

All entries on the RWA Perps dashboard fall under the **RWA Perps** category. Within that category, each market is assigned one of the following asset classes.

| Asset Class | Definition |
| --- | --- |
| Stock Perp | Perpetual markets that track the price of an individual public company's equity. They provide margined, cash-settled equity exposure and do not confer ownership of the underlying shares. |
| ETF Perp | Perpetual markets that track the price of an exchange-traded fund. They provide margined, cash-settled fund exposure and do not represent ownership of fund shares or redemption rights. |
| Equity Perp | Perpetual markets that track a public-equity benchmark or basket. They provide margined, cash-settled index exposure rather than a directly backed token or ownership claim on constituent shares. |
| Commodity Perp | Perpetual markets that track a commodity or commodity benchmark, such as gold or crude oil. They provide margined, cash-settled commodity exposure and do not represent title to physical inventory. |
| Bond Perp | Perpetual markets that track a bond or bond benchmark, such as government bonds, bond ETFs, or fixed-income indices. They provide margined, cash-settled fixed-income exposure and do not represent ownership of the underlying securities. |
| Perp Venue | Venues that list and operate perpetual markets tied to real-world reference assets such as public equities, ETFs, equity indices, bonds, and commodities. |

---

## Key Differences from the Main RWA Dashboard

The RWA Perps dashboard differs from the main RWA dashboard in several important ways:

**Primary sizing metric.** The main dashboard uses Onchain Marketcap and Active Marketcap to measure asset size. The perps dashboard uses Open Interest (outstanding notional exposure) instead, since perpetual contracts do not have a circulating token supply in the traditional sense.

**No redemption or attestation flags.** Perpetual contracts are cash-settled derivatives and do not represent direct claims on underlying assets. The evidence flags (Attestations, Redeemable) that are central to the main dashboard's classification framework do not apply to perps.

**Venue-centric structure.** Each market on the perps dashboard is tied to a specific venue. The same underlying asset (e.g. Tesla) may have separate entries for each venue that lists a perpetual contract for it. The Venues tab provides aggregated venue-level metrics.

**Funding rate and premium.** These metrics are unique to perpetual markets and reflect the cost of maintaining leveraged positions and the divergence between perp and spot prices.

**Reference Asset and Reference Asset Group.** These fields enable cross-venue comparison by normalizing the underlying asset name across different venues that may use different contract tickers for the same exposure. The Reference Asset Group taxonomy is shared with the main RWA dashboard and is documented in [Definitions & Taxonomy](definitions-and-taxonomy.md).
