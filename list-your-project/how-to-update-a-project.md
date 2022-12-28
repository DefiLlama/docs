# How to update a project

If you'd like to update the code used to calculate the TVL of a DeFi project already listed on DefiLlama:

1. Fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page).
2. Make your changes to the fork (generally easiest by cloning your new fork into a desktop IDE).
3. Make a Pull Request from your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
4. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR as there a monitored regularly.

If you'd like to add a **Hallmark** to a DeFi project listed on DefiLlama:

1. Same as steps 1 & 2 above, fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page) so that you can make your changes.
2. Add a "hallmarks" export to module.exports and add your hallmark as an array inside of an array(i.e. hallmarks: \[\[unix timestamp, "your note as a string"]]. You can add more by separating  the inner arrays with a comma(","). View an example of hallmark entries [here](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/uniswap/index.js#L57). An easy way to convert a regular date into a unix timestamp is using the website [here](https://www.epochconverter.com/). Timestamp needs to be in seconds.
3. Once you have added the Hallmarks make a Pull Request from your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
4. Hallmarks is not for adding any product development but instead its meant to explain changes in TVL. Only add events that had an impact on the TVL of a project.&#x20;
5. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR&#x20;

If you'd like to update the metadata (name, logo, description, adding an audit info etc) of a project already listed on DefiLlama:

1. This information can be updated from the [data.ts](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/protocols/data.ts) and [data2.ts](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/protocols/data2.ts) files.
2. Fork the [defillama-server repo](https://github.com/DefiLlama/defillama-server)(button towards the top right of the repo page).
3. Find your protocol and make your changes to the fork (generally easiest by cloning your new fork into a desktop IDE). Icons should be saved as a 240px x 240px .jpg or .png file. Please save the file under the same name as the protocol.
4. Make a Pull Request from your fork, to the main defiLlama-server repo, with a brief explanation of what you changed.
5. Wait for someone to either comment on or merge your Pull Request.&#x20;

Updating the logo requires one additional step:

1. Icons need to be saved in both the defillama-server repo and in the defillama-app repo.
2. Fork the [icons repo](https://github.com/DefiLlama/icons) (button towards the top right of the repo page).
3. Add your icon to the public [icons](https://github.com/DefiLlama/defillama-app/tree/main/public/icons) file as a 240px x 240px .jpg \*\*Please save the file under the very same name as the protocol.
4. Make a Pull Request from your fork
5. Wait for someone to either comment on or merge your Pull Request.&#x20;

