---
hidden: true
---

# Important considerations

{% hint style="info" %}
**Note (Updated: 2025-05-01):** This page might contain outdated information. For the most up-to-date documentation, please visit the main [Building Dimension Adapters](./) guide.
{% endhint %}

## Important considerations

**Multiple adapters, same protocol**

* If you want to list your protocol in different dashboards you can return in the same adapters dimensions of different dashboards. See the following example:

```typescript
const adapter = {
  adapter: {
    ...,
    fetch: async ({ endTimestamp }) => {
        const { dailyVolume } = await querySubgraph(volumeQuery, endTimestamp)
        const { dailyFees } = await querySubgraph(feesQuery, endTimestamp)
        return {
            dailyVolume, // dimension for dexs dashboard
            dailyFees, // dimension for fees dashboard
        }
    }
    ...,
  }
}
```

**Methodology**

* For fees and revenue adapters please don't forget to include how have you calculated the values in the methodology attribute.

```typescript
const adapter = {
  adapter: {
    ...
    fetch: async () => ({
      dailyFees: "32498",
      dailyRevenue: "0",
      dailySupplySideRevenue: "32498"
    }),
    meta: {
      methodology: {
        Fees: "User pays 2% of each swap",
        Revenue: "Protocol takes no revenue",
        SupplySideRevenue: "LPs revenue is 2% of user fees",
      }
    }
  }
}
```

**Incomplete data**

* If the adapter is not able to provide data for all dimensions, let's say for example you can only provide data for `dailyVolume` but not for `totalVolume`, please keep the dimension as `undefined`, don't assign an arbitrary value or set to it `"0"`.

```typescript
// wrong
{
    dailyFees: "2144",
    totalFees: "0" // <- wrong!!
}

// correct
{
    dailyFees: "2144"
}
```

* If the adapter can't be used to backfill data and only allows to get data for today's timestamp, please **set the flag** `runAtCurrTime` to `true`. See more about this flag in the previous sections.

```typescript
const adapter = {
  adapter: {
    fetch: fetchFunction,
    runAtCurrTime: true
  }
}
```

**Precision**

Please, consider using the _BigNumber_ library when doing mathematical operations that might result or include a value with a magnitude outside the range of safe values to use in JavaScript.

```typescript
const feesInGas = new BigNumber(graphRes["fees"])
const ethGasPrice = await getGasPrice(timestamp)
return {
    dailyRevenue = feesInGas.multiplyBy(ethGasPrice)
}
```
