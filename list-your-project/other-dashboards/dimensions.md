{% hint style="info" %}
**Note (Updated: 2025-05-01):** This page might contain outdated information. For the most up-to-date documentation, please visit the main [Building Dimension Adapters](../how-to-write-dimension-adapter.md) guide.
{% endhint %}

# Dimensions

In the previous page we have seen how to create the structure of our adapter. In this section we will focus on explaining the different dimensions that our adapters can return.

We call dimension to the attributes returned by the `fetch` function of our adapters. Depending on where would you like to list your project, you should return one of the below dimensions.

All dimensions should be returned as balance object (`Object<string>`) where keys are the coins and their values are the amount of each coin. Our code will get the price of each coin and calculate the final result.

Examples:

```typescript
// volume dimension in tokens object
{
    dailyVolume: {
        "ethereum:0x0000000000000000000000000000000000000000": "3924300000000"
    }
}
```

> In order to be listed your adapter would need to provide a minimum of one **daily** dimension. Providing all of them is not required but recommended in order to have better insights.

{% hint style="info" %}
All total values like totalVolume, totalFees, totalRevenue... are OPTIONAL
{% endhint %}

**Dexs, dexs aggregators and derivatives dimensions:**

*   `dailyVolume`: (Required for these dashboards) Trading volume for the period.

**Options Dimensions:**

*   `dailyNotionalVolume`: Notional volume of options contracts traded/settled.
*   `dailyPremiumVolume`: Premium volume collected/paid.

**Fees Dimensions:**

*   `dailyFees`: (**Required**) All fees and value collected from *all* sources (users, LPs, yield generation, liquid staking rewards, etc.), excluding direct transaction/gas costs paid by users to the network. This represents the total value flow into the protocol's ecosystem due to its operation.
*   `dailyUserFees`: (Optional, but helpful) The portion of `dailyFees` directly paid by end-users (e.g., swap fees, borrow interest, liquidation penalties, marketplace commissions paid by buyers/sellers).
*   `dailyRevenue`: (**Required**) The portion of `dailyFees` kept by the protocol entity itself, distributed either to the treasury (`dailyProtocolRevenue`) or governance token holders (`dailyHoldersRevenue`).
    *   `dailyRevenue = dailyProtocolRevenue + dailyHoldersRevenue`
*   `dailyProtocolRevenue`: The portion of `dailyRevenue` allocated to the protocol's treasury or core team.
*   `dailyHoldersRevenue`: The portion of `dailyRevenue` distributed to governance token holders (e.g., buybacks, burns).
*   `dailySupplySideRevenue`: The portion of `dailyFees` distributed to liquidity providers, lenders, or other suppliers of capital/resources essential to the protocol's function.
*   `dailyBribeRevenue`: Governance token paid as bribe/incentive for token holder action.
*   `dailyTokenTax`: Fees generated from a tax applied to token transfers.

## Fee/Revenue Attribution Examples by Protocol Type

If you are unsure how to classify fees and revenues, refer to this table or ask on Discord:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>


| Attribute         | DEXs                                        | Lending                                    | Chains                                         | NFT Marketplace                        | Derivatives                      | CDP                        | Liquid Staking                  | Yield                              | Synthetics                       |
| ----------------- | ------------------------------------------- | ------------------------------------------ | ---------------------------------------------- | -------------------------------------- | -------------------------------- | -------------------------- | ------------------------------- | ---------------------------------- | -------------------------------- |
| UserFees          | Swap fees paid by users                     | Interest paid by borrowers                 | Gas fees paid by users                         | Fees paid by users                     | Fees paid by users               | Interest paid by borrowers | % of rewards paid to protocol   | Paid management + performance fees | Fees paid by users               |
| Fees              | =UserFees                                   | =UserFees                                  | =UserFees                                      | =UserFees                              | UserFees + burn/mint fees        | =UserFees                  | Staking rewards                 | Yield                              | =UserFees                        |
| Revenue           | % of swap fees going to protocol governance | % of interest going to protocol governance | Burned coins (fees-sequencerCosts for rollups) | Marketplace revenue + creator earnings | Protocol governance revenue      | =ProtocolRevenue           | =ProtocolRevenue                | =ProtocolRevenue                   | =ProtocolRevenue                 |
| ProtocolRevenue   | % of swap fees going to treasury            | % of interest going to protocol            | \*                                             | Marketplace revenue                    | Value going to treasury          | Interest going to treasury | =UserFees                       | =UserFees                          | % of fees going to treasury      |
| HoldersRevenue    | Money going to gov token holders            | \*                                         | \*                                             | \*                                     | Value going to gov token holders | \*                         | \*                              | \*                                 | % of fees going to token holders |
| SupplySideRevenue | LPs revenue                                 | Interest paid to lenders                   | \*                                             | \*                                     | LP revenue                       | \*                         | Revenue earned by stETH holders | Yield excluding protocol fees      | LPs revenue                      |

> Some notes:
>
> * Protocol governance includes treasury + gov token holders
> * Revenue = HoldersRevenue + ProtocolRevenue
