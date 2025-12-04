# DAT Methodology

Every other data provider lists a single mNAV number per DAT, but we believe that this can lead to incorrect assumptions, so instead we provide 3 numbers that define a range.

### Why one mNAV number can mislead you

Look at Greenlane Holdings on November 30th, 2025:

| Stat                                     | Value                   |
| ---------------------------------------- | ----------------------- |
| Total BERA holdings                      | ~54.23 million BERA     |
| Treasury value                           | ~54.34 million dollars  |
| Share price                              | ~3.38$                  |
| Outstanding Share Count                  | ~4.8 million shares     |
| mNAV (marketcap / crypto treasury value) | 0.33                    |

Looking at this mNAV number you would conclude “Greenlane trades at 0.33 times its crypto treasury. This looks extremely cheap.”

However, the company has many active contracts (that filings indicate are effectively certain to dilute over time / that are very likely to result in future share issuance), and when you account for this future dilution, the total share count that will have a claim on the treasury will be ~35.4m shares, resulting in an mNAV of ~2.42. So you might buy thinking you're getting a heavy discount to the crypto treasury and you're protected when in reality you're buying at ~2.42x premium over the crypto treasury value, very expensive and exposing yourself to a ~60% loss.

To help investors avoid falling into this pitfall, and to provide more accurate metrics on the market, we created 3 versions of mNAV, each using a different share count:

* **Realized mNAV**: The ratio of marketcap to crypto treasury value where marketcap = share price × currently issued shares.
* **Realistic mNAV**: The ratio of marketcap to crypto treasury value, where marketcap uses the economically most probable share count multiplied by the share price.
* **Maximum mNAV**: The ratio of marketcap to crypto treasury value, where the marketcap is based on the fixed-count contractual dilution ceiling times the share price. Covers the case of the maximum number of shares that could be minted based on the current active contracts.

### Definitions

#### Realized mNAV

The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by factual, legal share count today.

Includes all completed share events after the latest filing using the outstanding share count and including all new issuances, settled ATM sales, executed conversions, exercises, repurchases, cancellations, splits, and ADS ratio changes.

It answers:

> “How is the market valuing the crypto treasury on the shares that exist right now?”

#### Realistic mNAV

The ratio of marketcap to crypto treasury value, where the marketcap uses the economically most probable share count multiplied by the share price.

Builds on the fully diluted realized share count, also known as outstanding shares, and adds dilution that is unavoidable under GAAP, such as diluted EPS share count, prefunded warrants, triggered convertibles or earnouts and mandatory conversions.

Uses basic instead of diluted if the company reports a net loss.

It answers:

> “What does valuation look like once unavoidable dilution is priced in?”

#### Maximum mNAV

The ratio of marketcap to crypto treasury value, where the marketcap is based on the fixed-count contractual dilution ceiling times the share price.

Covers all fixed share contracts such as options, warrants, RSUs, PSUs, fixed convertibles, and fixed share earnouts.

Excludes dollar-based programs such as ATMs, shelves and equity lines because these are not fixed share counts, are not guaranteed to be used and are highly price and path dependent.

It answers:

> “What does valuation look like under the full fixed share contractual scenario?”

### Why this three line system is better

* It reveals dilution instead of hiding it inside one opaque denominator.
* It shows the factual baseline, the economic central case and the fixed contract ceiling.
* It maintains one clear numerator and one clear denominator.
* It provides consistent comparability across issuers.
* It prevents errors caused by stale share counts, ATM capacity, ADS ratio drift and vague “fully diluted” claims.

---

### Exact methodology and sourcing to replicate DefiLlama's mNAV

The sections above explain the intuition and definitions behind the three mNAV lines. This section specifies the exact data inputs and calculations used so that DefiLlama’s DAT mNAV can be replicated.

#### 1. mNAV formula and notation

For each dilution bucket `B` (Realized, Realistic, Maximum):

`mNAV_B = (FD shares_B × Share price) ÷ Crypto Treasury Value`

Where:

- `FD shares_B` is the fully diluted share count for bucket `B`.
- `Share price` is the last trade on the primary exchange.
- `Crypto Treasury Value` is the total USD value of crypto tokens the company directly holds.

Numerator:

- `Marketcap_B = FD shares_B × Share price`

Denominator:

- `Crypto Treasury Value`

Interpretation:

- `mNAV_B > 1` → the company trades at a **premium** to its crypto treasury.
- `mNAV_B < 1` → the company trades at a **discount** to its crypto treasury.

#### 2. Crypto Treasury Value

**Crypto Treasury Value** is the total USD value of all crypto tokens the company directly holds on a given date.

Treasury includes:

- Tokens in company-controlled wallets.
- Tokens held with custodians.
- Wrapped assets representing the same economic exposure.
- Staked assets, valued at principal token units (not including future rewards).
- Stablecoins classified as treasury reserves.

**Multi-token treasuries**

If a company holds multiple tokens, each token is valued separately and then summed:

`Crypto Treasury Value = (BTC units × BTC price in USD) + (ETH units × ETH price in USD) + (WLD units × WLD price in USD) + …`

**Not included:**

- Customer assets.
- Cash, bonds, equities, or other non-crypto assets.
- Equity stakes in other publicly traded companies, including other DAT names.

Examples:

- BMNR holding ORBS stock is **not** counted.
- BMNR holding WLD tokens directly **is** counted.

**Token count sourcing**

DefiLlama uses authoritative, up-to-date sources in the following priority order:

1. SEC filings.
2. Company IR pages.
3. Treasury dashboards.
4. Custodian disclosures.
5. Verified wallets.
6. Official press releases via PR Newswire, GlobeNewswire, Business Wire, Accesswire.

**Handling vague language**

When disclosures are not precise:

- “At least 10,000 BTC” → recorded as **10,000 BTC**.
- “Around $1m of ETH” → converted into ETH units using DefiLlama pricing at the reference date.
- “Substantial amount” → ignored unless a quantifiable range can be derived.

If only USD values are disclosed, DefiLlama uses that USD figure directly as part of **Crypto Treasury Value**.

**Token pricing**

DefiLlama uses aggregated, volume-weighted exchange pricing that is consistent across all DefiLlama products.

#### 3. Fully diluted share buckets (FD)

For a given date, **Crypto Treasury Value** is the same across all buckets. The difference between the three mNAV lines comes from the **FD share count used in the numerator** through `Marketcap_B`.

All buckets:

- Adjust for stock splits.
- Adjust for reverse splits.
- Normalize ADS structures and ADS ratio changes.

##### FD realized (Shares Outstanding, used for Realized mNAV)

FD realized corresponds to the **currently existing shares** that have legal claim today.

The FD realized share count is composed of:

- Shares outstanding from the latest filing.
- Plus **completed** corporate actions after that filing:
  - New issuances (including ATM settlements).
  - Executed conversions.
  - Option and warrant exercises.
  - Repurchases and cancellations.
  - Splits and reverse splits.
  - ADS ratio changes.

This FD share count is used as the `FD shares` input when calculating the **Realized mNAV** line.

##### FD realistic (GAAP likely dilution, used for Realistic mNAV)

FD realistic incorporates dilution that is effectively unavoidable under GAAP, starting from FD realized.

The FD realistic share count is composed of:

- FD realized shares.
- Diluted weighted average shares from GAAP EPS.
- Prefunded warrants.
- Earnouts or conversions that filings indicate are certain or already triggered.
- For issuers with a **net loss**, diluted EPS share count is replaced by basic (GAAP rule).

This FD share count is used as the `FD shares` input when calculating the **Realistic mNAV** line.

##### FD maximum (fixed share contractual dilution, used for Maximum mNAV)

FD maximum captures the full fixed-share contractual ceiling, starting from FD realistic.

The FD maximum share count is composed of:

- FD realistic.
- All fixed-share instruments, whether in- or out-of-the-money:
  - Options.
  - Warrants.
  - RSUs.
  - PSUs.
  - Fixed convertibles.
  - Fixed-share earnouts.

Explicitly **excludes**:

- Dollar-denominated ATM capacity.
- Shelf registration capacity.
- Equity lines and similar dollar-based facilities.

These are excluded because they:

- Are not fixed share counts.
- Are not guaranteed to be used.
- Are highly price- and path-dependent.

This FD share count is used as the `FD shares` input when calculating the **Maximum mNAV** line.

#### 4. Equity prices

Share prices are sourced from Yahoo Finance:

- Last traded price.
- On the primary listing exchange.
- Matched as closely as possible to the treasury valuation date used for token pricing.

#### 5. How mNAV is calculated

For each bucket `B`:

1. **Compute Crypto Treasury Value.**  
   Aggregate all directly held tokens at their USD prices.

2. **Compute `Marketcap_B`.**  
   `Marketcap_B = FD shares_B × Share price`.

3. **Compute `mNAV_B`.**  
   `mNAV_B = Marketcap_B ÷ Crypto Treasury Value`.

DefiLlama then publishes three lines:

- `mNAV_realized`
- `mNAV_realistic`
- `mNAV_maximum`

#### 6. Why mNAV may differ from other sources

mNAV values shown on DefiLlama may differ from other dashboards or broker data because:

- DefiLlama **excludes** equity stakes in other DATs from Crypto Treasury Value.
- Token counts can vary across IR pages, filings, custodians and PRs; DefiLlama reconciles and prioritizes sources as described.
- Vague PR language is interpreted **conservatively**.
- Some platforms include ATM capacity in their “fully diluted” share counts; DefiLlama does **not**.
- Some platforms do not normalize for ADS structures or historical splits.
- Pricing sources and reference times can differ.
- Disclosure dates can differ across data vendors.
- The method of calculating “mNAV” or “NAV multiple” can vary by platform and by company.

#### 7. Acronyms

- **DAT**: Digital Asset Treasury  
- **Crypto Treasury Value**: USD value of directly held tokens  
- **FD**: Fully Diluted  
- **FD realized**: Completed events only (shares outstanding plus completed actions)  
- **FD realistic**: GAAP likely dilution (FD realized plus unavoidable GAAP dilution)  
- **FD maximum**: All fixed-count contractual dilution (FD realistic plus all fixed-share instruments)  
- **mNAV**: Market to Net Asset Value multiple  
- **ATM**: At The Market program  
- **OTM**: Out of the Money  
- **ITM**: In the Money  
- **EPS**: Earnings Per Share  
- **RSU**: Restricted Stock Unit  
- **PSU**: Performance Stock Unit  
- **ADS**: American Depositary Share
