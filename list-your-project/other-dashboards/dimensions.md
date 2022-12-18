# Dimensions

In the previous page we have seen how to create the structure of our adapter. In this section we will focus on explaining the different dimensions that our adapters can return.

We call dimension to the attributes returned by the `fetch` function of our adapters. Depending on where would you like to list your project, you should return one of the following dimensions:

All dimensions can be returned in USD value (as `string`) or as a tokens object (as `Object<string>`).

Examples

```typescript
// volume dimension in USD
{
    dailyVolume: "465424"
}
// volume dimension in tokens object
{
    dailyVolume: {
        "ethereum:0x0000000000000000000000000000000000000000": "392.43"
    }
}
```

**Dexs, aggregators and derivatives dimensions:**

* dailyVolume
* totalVolume

**Options dimensions**

* `dailyNotionalVolume`
* `dailyPremiumVolume`
* `totalNotionalVolume`
* `totalPremiumVolume`

**Fees dimensions**

* `dailyFees` (Fees paid by protocol users excluding gas fees)
* `dailyRevenue` (Fees accrued to the protocol)
* `dailySupplySideRevenue` (Fees earned by liquidity providers)
* `dailyHoldersRevenue` (Money going to gov token holders or burned coins)
* `totalFees` (Accomulative value of dailyFees)
* `totalRevenue` (Accomulative value of dailyRevenue)
* `totalSupplySideRevenue` (Accomulative value of dailySupplySideRevenue)
* `totalDailyHoldersRevenue` (Accomulative value of dailyHoldersRevenue)

If you are not sure how to fit the different fees and revenues generated in your protocol, in [this page](https://github.com/DefiLlama/dimension-adapters/blob/master/README.md) you will find a table with different categories of protocols and with the different types of fees and revenues that can generate.
