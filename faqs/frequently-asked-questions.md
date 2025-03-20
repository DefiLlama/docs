---
description: Answers to the most Frequently Asked Questions
---

# Frequently Asked Questions

1. **What is DefiLlama?**
   * DefiLlama is the largest TVL aggregator for DeFi (Decentralized Finance). Our data is fully open-source and maintained by a team of passionate individuals and contributors from hundreds of protocols. Our focus is on accurate data and transparency.
2. **What is TVL?**
   * Total Value Locked, or TVL, is the sum of the value of crypto assets that have been deposited by users to a protocol for the purpose of earning rewards or interest.
3. **How does DefiLlama get TVL data?**
   * Each project that is listed has an adapter/code that returns its TVL. Preference is given to protocols that make on-chain calls to return the TVL and these make up the majority, but some adapters use subgraphs or APIs. The adapters are all open-source, making the functions used to get the TVL available to everyone.
4. **How is DefiLlama funded?**
   * Previously DefiLlama was funded by self-funding, donations and grants. Currently defillama is mainly funded by kickbacks from aggregators used on LlamaSwap.
5. **What is Pool2 TVL?**
   * Pool2 refers to a type of farm that requires users to take exposure to native tokens issued by the protocol. It is a sort of “secondary pool” to incentivize farmers to continue holding on to the protocol's tokens, or to provide liquidity for these tokens, instead of selling them.
6. **What is Staking TVL?**
   * Staking refers to the locking of crypto tokens issued by the protocol itself(i.e. The protocol’s own tokens) into a smart contract in an effort to earn more of those tokens in return.
7. **What are borrows?**
   * Borrows is a metric found in Lending platforms and refers to the amount of value that has been borrowed through the protocol.
8. **What is doublecount?**
   * Doublecount is a toggle found on the DefiLlama website that gives users the option to count or not count tokens such as receipt tokens or LP tokens received from providing liquidity to a protocol and which have since been deposited to another protocol.
9. **Why doesn't DefiLlama count ADA/SOL/ETH staked?**
   * At DefiLlama we don't count chain staked assets for ANY chain. Users use us to track DeFi adoption, and adding Chain Staking would overshadow the TVL from their DeFi protocols, making chain TVL just a proxy of token marketcap, completely removing usage as a metric to track DeFi adoption. We track liquid staking protocols but these are not included by default into chain TVL.
   * Example: As of 2025-03-20, Cardano TVL is 330m$, while 15.84bn$ in ADA are staked. Thus if we included staked ADA into TVL, DeFi would make up only 2% of TVL, and due to the frequent 2-5% daily movements in the price of ADA, users wouldnt be able to tell anything about DeFi from that chart. DeFi adoption could double overnight but if ADA price drops -2.5% the chart would go down.
10. **How to list your protocol?**
    * Getting your protocol listed on DefiLlama is very easy and only requires that we have an adapter that returns your projects TVL. We have a guide available on how to write an adapter [here](https://docs.llama.fi/list-your-project/submit-a-project).
11. **What is Treasury TVL?**
    * Treasury TVL refers funds in the protocol's treasury, not including the platform's own governance token.
12. **Does DefiLlama have an api?**
    * Yes. Our API is an open API and it free to use. Citing DefiLlama as the source is much appreciated. Find the API docs [here](https://defillama.com/docs/api).
13. **How can I download DefiLlama data?**
    * DefiLlama data is available to download in csv format. You can find a “download” .csv button on various areas of the website: protocol page, chains page, overview and the bottom of the side menu bar. The amount of data retrieved depends on where the button is used, using it on the protocol page retrieves data for the specific protocol, using it on the chains page retrieves info by chain, the others retrieve all TVL data on the website.
