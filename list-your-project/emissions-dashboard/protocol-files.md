---
description: Continuing with the Liquity example...
---

# Protocol Files

Protocol files each export a `Protocol` object (line 55) which contains these properties:&#x20;

*   Each distinct section of token allocation, function | function\[], required:

    These sections can take on whatever keys are required. These keys will be used to label the chart on our UI, so please make them readable and nice to look at. The value for each chart section key will usually either be a `manual....()` or `manual....()[]` (more info [on the next page](emission-sections.md)). For example, the Liquity adapter has:

    * Stability Pool rewards
    * Uniswap LPs
    * Endowment
    * Team and advisors
    * Service providers
    * Investors.&#x20;
*   notes, string\[], optional:&#x20;

    Anything you think DefiLlama users should know about the emissions data. Maybe you made an assumption somewhere, or have excluded certain allocations because they were impossible to quantify.&#x20;
*   token, string, required:&#x20;

    The token address, preferably on the chain it was first deployed.&#x20;
*   sources, string\[], required:&#x20;

    Links to wherever you got the data. Official protocol docs are preferred, but discord servers etc can also be used if needed.&#x20;
*   ProtocolIds, string\[], required:

    Please leave this as an empty array (`[]`) and the DefiLlama team will complete this for you.
