# Listing your project

Same as TVL adapters, the majority of adapters for the dashboards under this section are contributed and maintained by their respective communities, with all changes being coordinated through the [`DefiLlama/adapters` github repo](https://github.com/DefiLlama/adapters).

Here you will find information about how you can list your DeFi project to one of the following dashboards:
- **Dexs dashboard**. Tracks daily and total `volume` from different dexs.
- **Aggregators dashboard**. Tracks daily and total `volume` from different aggregators.
- **Fees dashboard**. Tracks daily and total `fees` and `revenue` from different protocols.
- **Incentives dashboard**. Tracks daily `incentives` distributed from different protocols.
- **Derivatives dashboard**. Tracks daily and total `volume` from different protocols.
- **Options dashboard**. Tracks daily and total notonial and premium `volume` from different options dexs.

The simple instructions to add your project are:
1. Fork the [`Adapters`](https://github.com/DefiLlama/adapters) repository.
2. Add a new folder with the slug of the project under the respective adapteres folder (under `/fees` for fees dashboard, under `/incentives` for incentives dashboard, etc.)
3. Add your adapter to an `index.ts` file and export it. You can find more information [here](how-to-build-an-adapter.md).
4. Test that the adapter works correctly by running `yarn test [dashboard] [protocolSlug]`. More information on how to test your adapter [here](how-to-test-your-adapter.md).
5. Submit a PR! A llama will take a look at it and merge it. Once merged, it can take up to 24h to be available in the dashboard.