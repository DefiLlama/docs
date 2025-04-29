---
hidden: true
---

# Listing your project

{% hint style="info" %}
**Note (Updated: 2025-05-01):** This page might contain outdated information. For the most up-to-date documentation, please visit the main [Building Dimension Adapters](../how-to-write-dimension-adapter.md) guide.
{% endhint %}

## Listing your project

Same as TVL adapters, the majority of adapters for the dashboards under this section are contributed and maintained by their respective communities, with all changes being coordinated through the [`DefiLlama/dimension-adapters` github repo](https://github.com/DefiLlama/dimension-adapters).

Here you will find information about how you can list your DeFi project to one of the following dashboards:

* **Dexs dashboard**. Tracks volume from different dexs (only spot/swaps).
* **Aggregators dashboard**. Tracks `volume` from different aggregators.
* **Fees dashboard**. Tracks `fees` and `revenue` from different protocols.
* **Derivatives dashboard**. Tracks `volume` from different protocols.
* **Options dashboard**. Tracks notional and premium `volume` from different options dexs.

The simple instructions to add your project are:

1. Fork the [`dimension-adapters`](https://github.com/DefiLlama/dimension-adapters) repository.
2. Add a new folder with the slug of the project under the respective adapters folder.
3. Add your adapter to an `index.ts` file and export it. It should look like this: `./[dashboard]/[slug]/index.ts`. You will find more information in the next sections.
4. Test that the adapter works correctly by running `yarn test [dashboard] [protocolSlug]`. You will find more information in the next sections.
5. Submit a PR! A llama will take a look at it and merge it. Once merged, it can take up to 24h to be available in the dashboard.

{% hint style="info" %}
Seeing issues getting logs or with calls at historical blocks?\
You can replace the RPC being used by creating a .env file and filling it with rows like this:\
ETHEREUM\_RPC="https://..."

BSC\_RPC="https://..."

POLYGON\_RPC="https://..."\
...
{% endhint %}
