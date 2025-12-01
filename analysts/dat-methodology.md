# DAT Methodology

Every other data provider lists a single mNAV number per DAT, but we believe that this can lead to incorrect assumptions, so instead we provide 3 numbers that define a range.

### Why one mNAV number can mislead you &#x20;

Look at Greenlane Holdings on November 30th, 2025:

| Stat                                | Value                   |
| ----------------------------------- | ----------------------- |
| Total BERA holdings                 | \~54.23 million BERA    |
| Treasury value                      | \~54.34 million dollars |
| Share price                         | \~3.38$                 |
| Outstanding Share Count             | \~4.5 million shares    |
| mNAV (treasury value / share count) | 0.29                    |

Looking at this mNAV number you would conclude “Greenlane trades at 0.29 times its crypto treasury. This looks extremely cheap.”

However, the company has many active contracts (that filings indicate are effectively certain to dilute over time / that are very likely to result in future share issuance), and when you account for this future dilution, the total share count that will have a claim on the treasury will be \~32.6m shares, resulting in an mNAV of 2.1. So you might buy thinking you're getting a heavy discount to the crypto treasury and you're protected when in reality you're buying at 2x premium over the crypto treasury value, very expensive and exposing yourself to a 50% loss.

To help investors avoid falling into this pitfall, and to provide more accurate metrics on the market, we created 3 versions of mNAV, each using a different share count:

* **Realized mNAV**: The ratio of marketcap to crypto treasury value where marketcap = share price \* currently issued shares.
* **Realistic mNAV**: The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by the realistic share count, which is calculated by taking realized share count (outstanding shares) and adding dilution that is unavoidable under GAAP, such as diluted EPS share count, prefunded warrants, triggered convertibles, earnouts, and mandatory conversions.
* **Maximum mNAV**: The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by the the maximum share count, which simulates the max dilution if we assume that every single outstanding option, warrant and other contracts that can lead to share dilution gets executed. Covers the case of the maximum number of shares that could be minted based on the current active contracts.

### Definitions

#### Realized mNAV

The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by factual, legal share count today.

Includes all completed share events after the latest filing using the outstanding share count and including all new issuances, settled ATM sales, executed conversions, exercises, repurchases, cancellations, splits, and ADS ratio changes.

It answers:

“How is the market valuing the crypto treasury on the shares that exist right now?”

#### Realistic mNAV

The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by the economically most probable share count.

Builds on the fully diluted realized share count, also known as outstanding shares, and adds dilution that is unavoidable under GAAP, such as diluted EPS share count, prefunded warrants, triggered convertibles or earnouts and mandatory conversions.

Uses basic instead of diluted if the company reports a net loss.

It answers:

“What does valuation look like once unavoidable dilution is priced in?”

#### Maximum mNAV

The ratio of marketcap to crypto treasury value where the marketcap is calculated by multiplying the share price by the fixed count contract dilution ceiling.

Covers all fixed share contracts such as options, warrants, RSUs, PSUs, fixed convertibles, and fixed share earnouts.

Excludes dollar based programs such as ATMs, shelves and equity lines because these are not fixed share counts, are not guaranteed to be used and are highly price and path dependent.

It answers:

“What does valuation look like under the full fixed share contractual scenario?”

### Why this three line system is better

* It reveals dilution instead of hiding it inside one opaque denominator.
* It shows the factual baseline, the economic central case and the fixed contract ceiling.
* It maintains one clear numerator and one clear denominator.
* It provides consistent comparability across issuers.
* It prevents errors caused by stale share counts, ATM capacity, ADS ratio drift and vague “fully diluted” claims.
