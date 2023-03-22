---
description: Continuing with the Liquity example...
---

# Emission Sections

## Manual Entry Functions

90% of the time you'll only need the `manual...` functions (imported on line 1). There are 3 of them:

*   manualStep(

    start: unixTimestamp <- time of the first vest

    stepDuration: unixTimestamp <- time period between vests

    number: number <- number of discreet unlocks in the vesting schedule

    amount: number <- number of coins emitted **in each** discreet unlock

    )
*   manualCliff(

    start: unixTimestamp <- timestamp of the cliff&#x20;

    amount: number <- number of coins emitted in the cliff

    )
*   manualLinear(

    start: unixTimestamp <- start of the linear vesting period

    end: unixTimestamp <- end of the linear vesting period

    amount: number <- number of coins emitted over the period

    )

## More Complex Emission Shapes

Sometimes you might need to make more advanced shapes, for example, if the rate of emissions decrease over time. In this example, We created a rewards() function which makes a complex shape out of smaller linear sections (line 9, used on line 33).&#x20;

{% hint style="info" %}
Emissions sections should return real quantities of tokens. Not percentages, and accounting for any token decimals.
{% endhint %}

## On Chain Data

Some protocol files use on-chain data (Aave, Tornado etc). Where possible we like to use on chain data. The time sensitive nature of emissions schedules can make on-chain data more difficult to research and write. If you think using on-chain data is a better option for you, contact us through Discord and we can help out.
