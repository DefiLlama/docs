---
description: >-
  Our TVS methodology is guided by the principle: "If the oracle malfunctions,
  would assets be lost?"
---

# Oracles TVS

**Total Value Secured (TVS)** is a metric used to calculate the value secured by each oracle. It estimates the potential financial loss in case an oracle malfunctions or reports incorrect data. The TVS for an oracle is determined by summing the Total Value Locked (TVL) of all protocols dependent on that oracle, which would be susceptible to loss if the oracle fails.

## Methodology

* **Primary Calculation**:
  * TVS is calculated by aggregating the TVL of all protocols that rely on a specific oracle.
  * If an oracle failure would result in loss of funds in a protocol, that protocol's TVL is added to the oracle's TVS.
* **Edge Cases**:
  * **Protocols Using Multiple Oracles**:
    * If the failure of any oracle used by a protocol could cause a total TVL loss, the full protocol TVL is added to the TVS of all involved oracles.
    * Example: Euler uses Chainlink and Uniswap v3 TWAPs for pricing different assets. A failure in either oracle could lead to the protocol being drained. Thus, Euler's TVL is included in the TVS of both Chainlink and Uniswap TWAPs.
  * **Different Oracles for Different Chains**:
    * TVL on each chain is attributed to the oracle used on that chain.
    * Example: If a protocol uses Chainlink on Ethereum and Switchboard on Solana, Ethereum's TVL counts towards Chainlink, and Solana's TVL counts towards Switchboard.
  * **Partial Oracle Usage**:
    * If an oracle secures only a small portion of a protocol (e.g., 5% of TVL), and a hack of the oracle won't lead to >50% TVL loss, that oracle is not counted.
    * These cases are rare and generally negligible (<0.1% effect on TVS numbers).&#x20;
  * **Failover Mechanisms**:
    * For protocols with failover oracles, the primary question remains: _If the failover oracle malfunctioned, would assets be lost?_
    * If assets are unaffected by the failover oracle's failure, its TVS is not impacted. Loss occurs only if both the primary and failover oracles fail simultaneously.
* **Exclusions**:
  * **Centralized Exchanges (CEXs)**:
    * Oracles used in CEXs, such as index price feeds for perpetuals, are excluded from TVS due to:
      1. Aggregation with multiple data sources (e.g., spot prices from other CEXs).
      2. Fail-safes in place (e.g., withdrawal rate limits, rollback of positions in case of manipulation).

## Oracle to Blockchain Assignment

### Explanation of oraclesBreakdown

Use the `oraclesBreakdown` field to declare how your protocol uses oracles.

⚠️ **Do not use `oracles` or `oraclesByChain` — they are deprecated.**

Add the `oraclesBreakdown` field to your protocol entry in `data.ts`, `data1.ts`, `data2.ts`, `data3.ts` or `data4.ts` in the DefiLlama Server repo.

**Example:**

```javascript
oraclesBreakdown: [
  {
    name: "Chainlink",
    type: "Primary",
    proof: ["https://docs.yourprotocol.com/oracles"],
    startDate: "2023-01-01",
    chains: [
      { chain: "ethereum" },
      { chain: "polygon", startDate: "2023-03-01" },
    ]
  },
  {
    name: "RedStone",
    type: "Fallback",
    proof: ["https://github.com/yourprotocol/contracts/blob/main/RedstoneOracle.sol"]
  },
  {
    name: "AnotherOracle",
    type: "RNG",
    proof: ["https://docs.yourprotocol.com/randomness"]
  }
]
```

**Supported type values:**
* **"Primary"**: Main oracle that secures >50% of TVL. If compromised, a majority of funds would be at risk.
* **"Secondary"**: Actively used oracle that secures <50% of TVL.
* **"Fallback"**: Not used under normal conditions; used only if primary/secondary fails.
* **"Aggregator"**: Used alongside other oracles in a combined feed (e.g., median across 3 sources). Failure alone does not cause TVL loss.
* **"RNG"**: Used for randomness only (e.g., in games or lottery apps). No TVL is at risk.
* **"Reference"**: Used for price display or off-chain quoting. Not directly used for critical protocol operations that would result in TVL loss if the oracle fails.
* **"PoR"**: Proof of Reserve oracle, used to prove the protocol has the reserves to back the TVL

**Required fields:**
* `name`: Name of the oracle provider (e.g., Chainlink, Pyth).
* `type`: Oracle classification as defined above.
* `proof`: At least one link (docs, code, or audit) showing the oracle integration.

**Optional fields:**
* `startDate` / `endDate`: Use these to indicate the active period (format: YYYY-MM-DD).  
  If `startDate` is not provided, we assume the oracle has been active since the protocol's launch.
* `chains`: List of chain slugs from [`normalizeChain.ts`](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/utils/normalizeChain.ts), with optional start/end dates per chain.  
  If `chains` is not specified, we assume the oracle is used on **all chains** where the protocol is live.
This structured format ensures accurate attribution of TVL to oracles based on actual usage and risk exposure.

## How to Add a New Oracle to DefiLlama

To add a new oracle to DefiLlama, follow these steps:

#### 1. **Identify a Protocol Using the Oracle**

* Find a protocol that uses the oracle and is tracked by DefiLlama. If it's not yet tracked, let us know on Discord.

#### 2. **Edit the Appropriate File**

* Locate the appropriate `data.ts`, `data1.ts`, `data2.ts`, `data3.ts` or `data4.ts` file in the [DefiLlama Server repository](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/protocols/).
* Add or update the protocol entry using the `oraclesBreakdown` format.

#### 3. **Include the Following in Your Pull Request**

When submitting your changes, include answers to the following questions in your pull request description:

* **Oracle Provider(s)**: Specify the oracle(s) used (e.g., Chainlink, Band, API3, TWAP, etc.).
* **Implementation Details**: Briefly describe how the oracle is integrated into your project.
* **Documentation/Proof**: Provide links to documentation or other resources that verify the oracle's usage.
* **Failure Scenario & Loss Quantification** _(for non-price oracles)_: Describe a realistic scenario in which your oracle provides incorrect, stale, or manipulated non-price data. How would such a malfunction lead to on-chain asset loss or protocol malfunction, and what is the estimated TVL at risk under that scenario?

#### 4. **Submit a Pull Request**

* Open a PR in the [DefiLlama GitHub repository](https://github.com/DefiLlama/defillama-server/).
* Clearly describe the protocol(s) updated and the oracle usage.
* Include all necessary references and documentation.
