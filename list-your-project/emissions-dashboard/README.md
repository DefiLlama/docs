---
description: How to add your favourite projects token emission schedules to DefiLlama
---

# Emissions dashboard

## **Example**

Throughout the pages in this guide we will refer to the [Liquity emissions adapter](https://github.com/DefiLlama/emissions-adapters/blob/master/protocols/liquity.ts).

## **Format**

Once a day we run all of the protocol files in the emissions adapters repo (listed in `protocols/index.ts` ). Protocol files give us a schedule and metadata for a specified token.

When adding a new emissions schedule to DefiLlama, it's vital to test your code before you submit it. If your code doesn't produce an expected result it won't be accepted, so please read the [Testing section](testing.md).



## How To Contribute Your Code

Just like for TVL adapters, if you'd like to add an emissions schedule to DefiLlama:

1. Fork the [emissions-adapters repo](https://github.com/DefiLlama/emissions-adapters) (button towards the top right of the repo page).
2. Add a new [protocol file ](protocol-files.md)to protocols/ .
3. Make a Pull Request with the changes on your fork, to the main DefiLlama emissions-adapters repo, with a brief explanation of what you changed.
4. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR as they are monitored regularly.
5. Once your PR has been merged, **please give 24 hours** for the team to load your listing onto the UI.

![prayge](https://cdn.discordapp.com/emojis/1011049613186318376.webp?size=48\&quality=lossless)
