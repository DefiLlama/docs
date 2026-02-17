# How to update a project

### Update TVL

If you'd like to update the code used to calculate the TVL of a DeFi project already listed on DefiLlama:

1. Fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page).
2. Make your changes to the fork (generally easiest by cloning your new fork into a desktop IDE).
3. Make a Pull Request from your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
4. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR as they are monitored regularly.

### Events

If you'd like to update or add an **Event** to a DeFi project listed on DefiLlama:

1. Same as steps 1 & 2 above, fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page) so that you can make your changes.
2. Add a "hallmarks" export to module.exports and add your hallmark as an array inside of an array (i.e. hallmarks: \[\["2025-05-29", "what happened"]]. You can add more by separating the inner arrays with a comma(","). View an example of hallmark entries [here](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/uniswap/index.js#L57). The dates must follow the YYYY-MM-DD standard.
3. Once you have added the Hallmarks make a Pull Request from your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
4. Hallmarks is not for adding any product development but instead its meant to explain changes in TVL. Only add events that had an impact on the TVL of a project.
5. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR.

### Update project metadata (name, description, logo...)

Open a ticket in [https://defillama.com/support](https://defillama.com/support) requesting a metadata update, along with the info to change and proof that the request is legitimate.

Proof should be a link to a source that's verified (domain or twitter that we currently track for the project) that explicitly lists the new values that should be changed.

For example, for a domain/URL update this proof should be the previous domain having a banner explaining the migration and listing the new domain, for other updates we accept a tweet from the official handle or a blogpost on the main domain explaining the rebrand.
