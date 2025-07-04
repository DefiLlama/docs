---
description: Answers to the most Frequently Asked Questions
---

# Frequently Asked Questions

#### What is DefiLlama?

DefiLlama is the largest TVL aggregator for DeFi (Decentralized Finance). Our data is fully open-source and maintained by a team of passionate individuals and contributors from hundreds of protocols. Our focus is on accurate data and transparency.

#### What is TVL?

Total Value Locked, or TVL, is the sum of the value of crypto assets that have been deposited by users to a protocol for the purpose of earning rewards or interest.

#### How does DefiLlama get TVL data?

Each project that is listed has an adapter/code that returns its TVL. Preference is given to protocols that make on-chain calls to return the TVL and these make up the majority, but some adapters use subgraphs or APIs. The adapters are all open-source, so they are open for review to everyone.

#### How is DefiLlama funded?

Previously DefiLlama was funded by self-funding, donations and grants. Currently DefiLlama is mainly funded by kickbacks from aggregators used on LlamaSwap and subscriptions.

#### At what frequency is data updated on DefiLlama?

* TVL, Total Borrows, Treasury, Stablecoin Supply, CEX Assets and Oracle TVS update every hour
* For DEX Volume, Fees, Revenues, Earnings and other metrics that aren't based on snapshots like TVL but instead on time ranges: Most protocols are updated hourly, but some protocols only update daily, with updates happening on 0:00 UTC
* Yield data (APY, Borrow APY, Pool TVL...) is updated hourly
* Bridge data is updated hourly

#### Why are the numbers from the API different from the numbers on the webpage?

The webpage feeds off the API, so data from API is updated first and then webpage updates afterwards. Because website pages are cached for performance, you might see different values between both sources, but the delay will be 1 hour at most.

#### What is Pool2 TVL?

Pool2 refers to a type of farm that requires users to take exposure to native tokens issued by the protocol. It is a sort of “secondary pool” to incentivize farmers to continue holding on to the protocol's tokens, or to provide liquidity for these tokens, instead of selling them.

#### What is Staking TVL?

* In many cases, a DeFi protocol will have their core smart contracts with TVL that provides some service, such as a DEX or a lending market, and then separately they'll have a contract where users can stake the protocol's governance token to earn rewards (usually revenue distribution or token inflation). Staking TVL refers to the TVL in these staking contracts, which are independent from the main protocol function.
* Examples are CRV locked into veCRV, AAVE locked into stkAAVE, locked CAKE for veCAKE on PancakeSwap...
* Staking TVL is never used for staking in Chains (eg ETH PoS staking), it's only for DeFi protocols.
* Why we don't include this staking into core TVL by default:\
  \- To avoid penalizing protocols that don't have a token or a staking program.\
  \- Because this staking is independent from the core protocol function and so a large staking TVL doesn't mean that protocol has traction, it muddies TVL as a proxy for traction.\
  \- This staking is largely riskless because if the contracts got hacked the protocol can always cover the loss by minting more governance tokens, and the risk of protocol hack is already baked into the protocol's token. However, for other tokens deposited into the protocol, if the protocol is hacked they would be completely lost. So core TVL provides a signal that the market considers the protocol trustworthy, but staking TVL doesn't provide that signal. Also, staking contracts are independent and simpler than core protocol contracts.

#### What are borrows?

Borrows is a metric found in Lending platforms and refers to the amount of value that has been borrowed through the protocol.

#### What is doublecount?

Doublecount is a toggle found on the DefiLlama website that gives users the option to count or not count tokens such as receipt tokens or LP tokens received from providing liquidity to a protocol and which have since been deposited to another protocol.

#### Why doesn't DefiLlama count ADA/SOL/ETH staked into TVL?

* At DefiLlama we don't count chain staked assets for ANY chain. Users use us to track DeFi adoption, and adding Chain Staking would overshadow the TVL from their DeFi protocols, making chain TVL just a proxy of token marketcap, completely removing usage as a metric to track DeFi adoption. We track liquid staking protocols but these are not included by default into chain TVL.
* Example: As of 2025-03-20, Cardano TVL is 330m$, while 15.84bn$ in ADA are staked. Thus if we included staked ADA into TVL, DeFi would make up only 2% of TVL, and due to the frequent 2-5% daily movements in the price of ADA, users wouldn't be able to tell anything about DeFi from that chart. DeFi adoption could double overnight but if ADA price drops -2.5% the chart would go down.

#### How to list your protocol?

Getting your protocol listed on DefiLlama is very easy and only requires that we have an adapter that returns your projects TVL. We have a guide available on how to write an adapter [here](https://docs.llama.fi/list-your-project/submit-a-project).

#### What is Treasury TVL?

Treasury TVL refers funds in the protocol's treasury, not including the platform's own governance token.

#### Does DefiLlama have an api?

Yes. Our API is an open API and it free to use. Citing DefiLlama as the source is much appreciated. Find the API docs [here](https://defillama.com/docs/api).

#### How can I download DefiLlama data?

DefiLlama data is available to download in CSV format. You can find a “download” .csv button on various areas of the website: protocol page, chains page, overview and the bottom of the side menu bar. The amount of data retrieved depends on where the button is used, using it on the protocol page retrieves data for the specific protocol, using it on the chains page retrieves info by chain, the others retrieve all TVL data on the website.

#### Why don't we include assets in other custodian under a CEX's assets?

* Our users use our CEX asset metrics in order to see how much the market trusts a CEX with their capital, which is used as a proxy for how safe a CEX is to leave their money in. Deposits in a different custodian are completely independent from this and don't provide that safety market signal.
* If we included custodians it'd open the door for our metric to be abused, for example by hypothetical CEX with 100m and 2bn in custodian vaults, displaying their assets as 2.1bn would give the signal that 2.1bn trust them with their money, when actually the opposite is true and 95% of their users trust them so little to keep the custody safe that theyre paying extra to have the custody segregated. If that CEX got hacked or had some issue and 95m was withdrawn, our metrics would show a 4.5% outflow, giving the signal that outflow is minimal and there's no danger of a run on the bank, when 95% have been withdrawn and actually the CEX is on the late stages of a bank run.
* TLDR: by including custodian assets we'd end up giving the opposite signals for our users, and our goal is to provide data that helps in their decisions, not the opposite.

#### Why does DefiLlama rank protocols by premium volume instead of notional volume?

There was a huge problem with washtrading options that had huge notional value but tiny premiums, which led to highly inflated notional volumes. That's why we switched to using premium volume because it reflected usage much better.
