---
description: Craft Your Own Metrics On-the-Fly
---

# Custom Columns

Transform your DeFiLlama experience with **Custom Columns**. Go beyond our standard metrics and define your own, turning the protocol table into a personalized analysis powerhouse. Calculate bespoke ratios, track hyper-specific growth rates, combine data points in novel ways, or set conditional flags to instantly spot protocols meeting your criteria – all directly within DeFiLlama.

### Adding Custom Columns
You can access this feature on the main page, at the bottom of the column selection dropdown.

After pressing "Add Custom Column" you'll be able to input name and formula for your custom columns

### Defining Your Columns
* Formula: Input your custom expression here. This field supports a rich syntax leveraging available protocol data, mathematical operators, and curated functions (detailed below). Real-time feedback warns about unrecognized inputs, and a preview shows the calculated result.
* Format: Select the desired display format for your calculated values (`Auto`, `Number`, `USD`, `Percent`, `Boolean (✅/❌)`). "Auto" intelligently handles most types, including rendering boolean results as checkmarks.

### Crafting Formulas
The formula engine supports curated set of powerful operations, giving you flexibility in creating the metrics that you need

**Available Data Fields**
*   `mcap`, `tvl`
*   `tvlPrevDay`, `tvlPrevWeek`, `tvlPrevMonth`
*   `change_1d`, `change_7d`, `change_1m` (TVL changes in decimal format, e.g., `0.05`)
*   `fees_24h`, `fees_7d`, `fees_30d`, `fees_1y`, `average_fees_1y`, `cumulativeFees`
*   `pf` (Market Cap / Annualized Fees), `ps` (Market Cap / Annualized Revenue)
*   `revenue_24h`, `revenue_7d`, `revenue_30d`, `revenue_1y`, `average_revenue_1y`, `cumulativeRevenue`
*   `volume_24h`, `volume_7d`, `volumeChange_7d`, `cumulativeVolume`
*   `category` (String, e.g., "DEX", "Lending")

{% hint style="info" %}
Not every fields is available in all protocols, for example volume is only available to protocol with DEXs category, if a protocol lacks a field your formula depends on, the result will be skipped for that row
{% endhint %}

**Operators**
Standard mathematical (`+`, `-`, `*`, `/`, `%`, `^`) and logical (`and`, `or`, `not`) operators are available, along with comparison operators (`==`, `!=`, `>`, `>=`, `<`, `<=`). Use parentheses `()` for grouping. The ternary operator (`condition ? value_if_true : value_if_false`) is supported for conditional expressions.

**Built-in Functions**
We provide a focused set of functions relevant for protocol analysis. Autocomplete (`ƒ` prefix) suggests these as you type, and selecting one adds `()` automatically.

*   `abs(x)`: Absolute value of x.
*   `ceil(x)`: Smallest integer greater than or equal to x (rounds up).
*   `floor(x)`: Largest integer less than or equal to x (rounds down).
*   `round(x)`: Rounds x to the nearest integer.
*   `roundTo(x, n)`: Rounds x to n decimal places. **(Highly useful for ratios)**
*   `sqrt(x)`: Square root of x.
*   `min(a, b, ...)`: Minimum value from the provided arguments.
*   `max(a, b, ...)`: Maximum value from the provided arguments.
*   `if(condition, true_val, false_val)`: Returns `true_val` if `condition` is true, else `false_val`. (Ternary `? :` is often cleaner).
*   `not(x)`: Logical NOT (inverts a boolean value).

**Conditional Expressions & Flags**
Create formulas that evaluate to `true` or `false`. Ideal for flagging protocols meeting specific criteria. With the "Format" set to "Auto" or "Boolean (✅/❌)", these display as intuitive checkmarks.

*   Example: `change_7d > 0.1 and revenue_7d > 100000` (Is 7d TVL growth > 10% AND 7d revenue > $100k?) -> ✅ / ❌
*   Example: `category == "Lending" and tvl > 50000000` (Is it a Lending protocol with TVL > $50M?) -> ✅ / ❌

**Formatting**

*   Use the **Format** dropdown to control the appearance of results (Number, USD, Percent, etc.). "Auto" is often sufficient and handles booleans correctly.

### Use-Case examples
1. High Growth Alert
* Formula: `change_7d > 0.15 and fees_7d > (fees_30d / 30 * 7 * 1.2)`
* This will show you protocols that has >15% gain in the last 7 days and recent 7d fees significantly above prior run rate.

2. Revenue per TVL
* Formula: `revenue_30d / tvl`
* Give you ratio of protocol revenue (for the last 30 days) compared to their TVL

3. Annualized Fee Yield
* Formula: `fees_1y / tvl`
* Measures how well protocol monetizes its TVL
