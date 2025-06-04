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
  * If an oracle failure would result in loss of funds in a protocol, that protocol's TVL is added to the oracle’s TVS.
* **Edge Cases**:
  * **Protocols Using Multiple Oracles**:
    * If the failure of any oracle used by a protocol could cause a total TVL loss, the full protocol TVL is added to the TVS of all involved oracles.
    * Example: Euler uses Chainlink and Uniswap v3 TWAPs for pricing different assets. A failure in either oracle could lead to the protocol being drained. Thus, Euler's TVL is included in the TVS of both Chainlink and Uniswap TWAPs.
  * **Different Oracles for Different Chains**:
    * TVL on each chain is attributed to the oracle used on that chain.
    * Example: If a protocol uses Chainlink on Ethereum and Switchboard on Solana, Ethereum’s TVL counts towards Chainlink, and Solana’s TVL counts towards Switchboard.
  * **Partial Oracle Usage**:
    * If an oracle secures only a small portion of a protocol (e.g., 5% of TVL), and a hack of the oracle won't lead to >50% TVL loss, that oracle is not counted.
    * These cases are rare and generally negligible (<0.1% effect on TVS numbers).&#x20;
  * **Failover Mechanisms**:
    * For protocols with failover oracles, the primary question remains: _If the failover oracle malfunctioned, would assets be lost?_
    * If assets are unaffected by the failover oracle’s failure, its TVS is not impacted. Loss occurs only if both the primary and failover oracles fail simultaneously.
* **Exclusions**:
  * **Centralized Exchanges (CEXs)**:
    * Oracles used in CEXs, such as index price feeds for perpetuals, are excluded from TVS due to:
      1. Aggregation with multiple data sources (e.g., spot prices from other CEXs).
      2. Fail-safes in place (e.g., withdrawal rate limits, rollback of positions in case of manipulation).



## Oracle to Blockchain Assignment

#### Explanation of Fields

**`oracles` Field**

* The `oracles` field is a simple list of oracles used by a protocol. It does not specify blockchain-level granularity.
*   Example:

    ```javascript
    oracles: ["Pyth", "Chainlink"]
    ```

    This implies that the protocol relies on both the Pyth and Chainlink oracles. The total TVL of the protocol will be counted towards the TVS of both oracles.
* **When to Use the `oracles` Field:**
  * Use this field if the protocol is deployed on multiple blockchains **and uses the same oracle(s) across all chains.**
  *   Example:

      ```javascript
      oracles: ["Chainlink"]
      ```

      In this case, Chainlink is used by the protocol on every blockchain it is deployed on, and the TVL on all chains will contribute to Chainlink’s TVS.

**`oraclesByChain` Field**

* The `oraclesByChain` field maps oracles to specific blockchains, providing more detailed information on how a protocol utilizes oracles across chains.
*   Example:

    ```javascript
    oraclesByChain: {
      polygon: ["Chainlink"], // Documentation for oracle usage on Polygon
      polygon_zkevm: ["API3"], // Documentation for oracle usage on Polygon zkEVM
    }
    ```

    In this case:

    * The TVL on Polygon is attributed to Chainlink.
    * The TVL on Polygon zkEVM is attributed to API3.
* **When to Use the `oraclesByChain` Field:**
  * Use this field if the protocol is deployed on multiple blockchains **and uses different oracles on different chains.**
  *   Example:

      ```javascript
      oraclesByChain: {
        ethereum: ["Chainlink"],
        solana: ["Switchboard"],
      }
      ```

      In this case, Chainlink secures the protocol’s TVL on Ethereum, and Switchboard secures its TVL on Solana.

**Important Note: Do Not Set Both Fields**

* For any given protocol, you should only set **either** the `oracles` field **or** the `oraclesByChain` field—not both.
  * Use the `oracles` field for simplicity if the same oracles are used across all chains.
  * Use the `oraclesByChain` field for detailed mapping if the protocol uses different oracles for different chains.

**Using Chain Identifiers**

* Both `oracles` and `oraclesByChain` fields should use the chain identifiers from the [`normalizeChain.ts`](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/utils/normalizeChain.ts) file to ensure consistency. (You need to use the case slug, not the return)
*   Examples of chain identifiers include:

    ```javascript
    case "shido":
      return "Shido";
    case "rbn":
      return "Redbelly";
    case "xsat":
      return "exSat";
    ```

    These identifiers ensure proper mapping between protocols and chains.

### How to Add a New Oracle to DefiLlama

To add a new oracle to DefiLlama, follow these steps:

#### 1. **Identify a Protocol Using the Oracle**

* Find a protocol that uses the oracle and ensure it is tracked by DefiLlama. If the protocol is not yet tracked, please let us know on our discord.

#### 2. **Edit the Appropriate File**

* Locate the `data.ts`, `data1.ts`, or `data2.ts` file in the [DefiLlama Server repository](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/protocols/).
* Add or update the protocol entry in the file, specifying the oracle(s) it uses.

#### 3. **Update the Oracle Fields**

* Depending on how the protocol uses oracles, update **either** the `oracles` field or the `oraclesByChain` field. **Do not set both fields for the same protocol.**
  *   **`oracles` Field**: Use this field for a general list of oracles used by the protocol.\
      Example:

      ```javascript
      oracles: ["Pyth", "Chainlink"]
      ```

      This means the protocol uses both the Pyth and Chainlink oracles, and its total TVL will contribute to the TVS of both oracles.
  *   **`oraclesByChain` Field**: Use this field for specifying chain-level oracle mappings.\
      Example:

      ```javascript
      oraclesByChain: {
        polygon: ["Chainlink"], // TVL on Polygon uses Chainlink
        polygon_zkevm: ["API3"], // TVL on Polygon zkEVM uses API3
      }
      ```

      * If the protocol uses a general set of oracles across all chains, use only the `oracles` field.
      * If the protocol uses different oracles for different chains, use only the `oraclesByChain` field.
*   Use chain identifiers from the [`normalizeChain.ts`](https://github.com/DefiLlama/defillama-server/blob/master/defi/src/utils/normalizeChain.ts) file for consistent mapping. Examples include:

    ```javascript
    case "unit0":
      return "UNIT0";
    case "shido":
      return "Shido";
    case "rbn":
      return "Redbelly";
    ```

#### 4. **Answer the Following Questions**

When submitting your changes, include answers to the following questions in your pull request description:

* **Oracle Provider(s)**: Specify the oracle(s) used (e.g., Chainlink, Band, API3, TWAP, etc.).
* **Implementation Details**: Briefly describe how the oracle is integrated into your project.
* **Documentation/Proof**: Provide links to documentation or other resources that verify the oracle's usage.
* **Failure Scenario & Loss Quantification** _(for non-price oracles)_: Describe a realistic scenario in which your oracle provides incorrect, stale, or manipulated non-price data. How would such a malfunction lead to on-chain asset loss or protocol malfunction, and what is the estimated TVL at risk under that scenario?

#### 5. **Submit a Pull Request**

* Open a pull request in the [DefiLlama GitHub repository](https://github.com/DefiLlama/defillama-server/).
* Include a clear description of the changes, answering the required questions and specifying:
  * The protocol(s) updated.
  * The oracles added and their usage (general or chain-specific).
  * Any relevant references or documentation links.
