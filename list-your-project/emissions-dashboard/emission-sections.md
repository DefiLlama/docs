---
description: Continuing with the Liquity example...
---

# Emission Sections

## Manual Entry Functions

90% of the time you'll only need the `manual...` functions (imported on line 1). There are 4 of them:

*   manualStep(

    start: unixTimestamp OR date string <- time of the first vest

    stepDuration: unixTimestamp OR date string <- time period between vests

    number: number <- number of discreet unlocks in the vesting schedule

    amount: number <- number of coins emitted **in each** discreet unlock

    dateFormat: string, optional <- if date strings are used, the format can be specified here. Default is "YYYY/MM/DD"

    )
*   manualCliff(

    start: unixTimestamp OR date string <- timestamp of the cliff&#x20;

    amount: number <- number of coins emitted in the cliff

    dateFormat: string, optional <- if date strings are used, the format can be specified here. Default is "YYYY/MM/DD"

    )
*   manualLinear(

    start: unixTimestamp OR date string<- start of the linear vesting period

    end: unixTimestamp OR date string <- end of the linear vesting period

    amount: number <- number of coins emitted over the period

    dateFormat: string, optional <- if date strings are used, the format can be specified here. Default is "YYYY/MM/DD"

    )
*   manualLog(

    start: unixTimestamp OR date string<- start of the linear vesting period

    end: unixTimestamp OR date string <- end of the linear vesting period

    amount: number <- number of coins emitted over the period

    periodLength: number <- the length in time between each change in rate

    percentDecreasePerPeriod: number <- the regular decrease in emission rate &#x20;

    dateFormat: string, optional <- if date strings are used, the format can be specified here. Default is "YYYY/MM/DD"

    )

## More Complex Emission Shapes

Sometimes you might need to make more advanced shapes, for example, if the rate of emissions decrease over time. In this example, We created a rewards() function which makes a complex shape out of smaller linear sections (line 9, used on line 33). NB this particular shape could've been done with the manualLog() function.

{% hint style="info" %}
Emissions sections should return real quantities of tokens. Not percentages, and accounting for any token decimals.
{% endhint %}

## On Chain Data

Some protocol files use on-chain data (Aave, Tornado etc). Where possible we like to use on chain data. The time sensitive nature of emissions schedules can make on-chain data more difficult to research and write. If you think using on-chain data is a better option for you, contact us through Discord and we can help out.
