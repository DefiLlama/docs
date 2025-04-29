---
hidden: true
---

# Testing

{% hint style="warning" %}
**Note (Updated: 2025-05-01):** This page might contain outdated information. For the most up-to-date documentation, please visit the main [Building Dimension Adapters](./) guide.
{% endhint %}

## Testing

After adding your adapter to the correct folder, you will be able to test it by checking the output of the following command:

```
> npm test [dashboard] [protocolSlug]
```

or

```
> npm test [dashboard] [protocolSlug] [timestamp]
```

Being `[dashboard]` one of the following values: `dexs`, `fees`, `incentives`, `aggregators`, `options`, `derivatives`. `[protocolSlug]` the name of your protocol and `[timestamp]` as optional parameter the timestamp of the day passed to the adapter to test if the adapter is able to return a response based on a given time.

Examples:

```
> npm test fees bitcoin
```

```
> npm test dexs uniswap 1662110960
```
