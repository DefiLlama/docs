# Testing

After adding your adapter to the correct folder, you will be able to test it by checking the output of the following command:

```
> yarn test [dashboard] [protocolSlug]
```

or

```
> yarn test [dashboard] [protocolSlug] [timestamp]
```

Being `[dashboard]` one of the following values: `dexs`, `fees`, `incentives`, `aggregators`, `options`, `derivatives`. `[protocolSlug]` the name of your protocol and `[timestamp]` as optional parameter the timestamp of the day passed to the adapter to test if the adapter is able to return a response based on a given time.

Examples:

```
> yarn test fees bitcoin
```

```
> yarn test dexs uniswap 1662110960
```
