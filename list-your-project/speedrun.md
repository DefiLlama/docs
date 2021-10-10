---
description: >-
  Just a bunch of adapter examples so you can try to speedrun by
  pattern-matching
---

# Speedrun

The adapter is just a function that takes a timestamp and block-height (on Ethereum) and returns the balances of tokens locked in your protocol's smart contracts at that point in time:

#### TrueFi

{% tabs %}
{% tab title="Code" %}
{% code title="projects/truefi/index.js" %}
```javascript
const sdk = require('@defillama/sdk');
const abi = require('./abi.json');

const POOL = '0xa1e72267084192db7387c8cc1328fade470e4149';
const stkTRU = '0x23696914Ca9737466D8553a2d619948f548Ee424';
const TRU = '0x4C19596f5aAfF459fA38B0f7eD92F11AE6543784';
const TUSD = '0x0000000000085d4780B73119b644AE5ecd22b376';


async function tvl(timestamp, block) {
    let balances = {};

    const poolTVL = await sdk.api.abi.call({
      target: POOL,
      abi: abi['poolValue'],
      block: block 
    });
    const truTVL = await sdk.api.abi.call({
      target: stkTRU,
      abi: abi['stakeSupply'],
      block: block 
    });
    
    balances[TUSD] = poolTVL.output;
    balances[TRU] = truTVL.output;
    
    return balances;
}

module.exports = {
  ethereum:{
    tvl,
  },
  tvl
}
```
{% endcode %}
{% endtab %}

{% tab title="ABI" %}
{% code title="projects/truefi/index.js" %}
```javascript
{
    "poolValue": {
        "inputs": [],
        "name": "poolValue",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    "stakeSupply": {
        "inputs": [],
        "name": "stakeSupply",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Calls" %}
```
Input 1: (1616784572, 12116367)

Output 1:
{
  '0x0000000000085d4780B73119b644AE5ecd22b376': '63673889181720756238069312',
  '0x4C19596f5aAfF459fA38B0f7eD92F11AE6543784': '6631227042218918'
}

Input 2: (1615453837, 12016392)

Output 2:
{
  '0x0000000000085d4780B73119b644AE5ecd22b376': '46862724247312775912594739',
  '0x4C19596f5aAfF459fA38B0f7eD92F11AE6543784': '5726346484318134'
}

Input 3: (1614123633, 11916399)

Output 3:
{
  '0x0000000000085d4780B73119b644AE5ecd22b376': '50438152493838773769557145',
  '0x4C19596f5aAfF459fA38B0f7eD92F11AE6543784': '3848190457433238'
}

Input 4: (1603507428, 11116398)

Output 4:
{} // Contracts didn't exist
```
{% endtab %}

{% tab title="CLI" %}
```
$ npx @defillama/sdk projects/truefi/index.js

TUSD                      63.59 M
TRU                       26.03 M
Total: 89.62 M
```
{% endtab %}
{% endtabs %}

#### Pool Together

{% tabs %}
{% tab title="Code" %}
{% code title="projects/pooltogether/index.js" %}
```javascript
const sdk = require("@defillama/sdk");
const { request, gql } = require("graphql-request");
const abi = require('./abi.json')

const graphUrls = [
  'https://api.thegraph.com/subgraphs/name/pooltogether/pooltogether-v3_1_0',
  'https://api.thegraph.com/subgraphs/name/pooltogether/pooltogether-v3_3_2'
]
const graphQuery = gql`
query GET_POOLS($block: Int) {
  prizePools(
    block: { number: $block }
  ) {
    id
    underlyingCollateralSymbol
    underlyingCollateralToken
    compoundPrizePool{
      cToken
    }
  }
}
`;

async function tvl(timestamp, block) {
  let balances = {};

  let allPrizePools = []
  for (const graphUrl of graphUrls) {
    const { prizePools } = await request(
      graphUrl,
      graphQuery,
      {
        block,
      }
    );
    allPrizePools = allPrizePools.concat(prizePools)
  }
  const lockedTokens = await sdk.api.abi.multiCall({
    abi: abi['accountedBalance'],
    calls: allPrizePools.map(pool => ({
      target: pool.id
    })),
    block
  })
  lockedTokens.output.forEach(call => {
    const underlyingToken = 
      allPrizePools.find(pool => pool.id === call.input.target)
      .underlyingCollateralToken;
    const underlyingTokenBalance = call.output
    sdk.util.sumSingleBalance(balances, underlyingToken, underlyingTokenBalance)
  })
  return balances
}


module.exports = {
  ethereum:{
    tvl,
  },
  tvl
}
```
{% endcode %}
{% endtab %}

{% tab title="ABI" %}
{% code title="projects/pooltogether/abi.json" %}
```javascript
{
    "accountedBalance": {
        "inputs": [],
        "name": "accountedBalance",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Calls" %}
```
Input 1: (1616785431, 12116436)

Output 1:
{
  '0x0d8775f648430679a709e98d2b0cb6250d2887ef': '106905291853415622153',
  '0x1f9840a85d5af5bf1d1762f925bdaddc4201f984': '550930210187159868576758',
  '0x6b175474e89094c44da98b954eedeac495271d0f': '49090672849737076366112325',
  '0xdac17f958d2ee523a2206206994597c13d831ec7': '0',
  '0x0cb42d5c9c4661fee5d2ecfcbad5bccefa0727a9': '2571305570268888809939',
  '0x3449fc1cd036255ba1eb19d65ff4ba2b8903a69a': '948456261968725066468',
  '0x58014f7eef95bcaaf84b8f4aa07507e1664e40b2': '1000000000000000000',
  '0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9': '0',
  '0x6b3595068778dd592e39a122f4f5a5cf09c90fe2': '9784581049508251225',
  '0xd2dda223b2617cb616c1580db421e4cfae6a8a85': '141130243431856096745605',
  '0x56687cf29ac9751ce2a4e764680b6ad7e668942e': '0',
  '0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48': '56990267448502',
  '0x57bc752ec42238bb60a6e65b0de82ef44013225d': '722305738252165754257',
  '0x2260fac5e5542a773aa44fbcfedf7c193bc2c599': '10684189',
  '0xa323fc62c71b210e54171887445d7fca569d8430': '64131624673594797066',
  '0x334cbb5858417aee161b53ee0d5349ccf54514cf': '0',
  '0xe41d2489571d322189246dafa5ebde1f4699f498': '144912629676243436495284',
  '0x57ab1ec28d129707052df4df418d58a2d46d5f51': '0',
  '0x1494ca1f11d487c2bbe4543e90080aeba4ba3c2b': '1911234433441576710220',
  '0x0000000000095413afc295d19edeb1ad7b71c952': '5382329275853684605192',
  '0xc00e94cb662c3520282e6f5717214004a7f26888': '15415779749925621232272',
  '0x0391d2021f89dc339f60fff84546ea23e337750f': '14324003410385851045633',
  '0x03ab458634910aad20ef5f1c8ee96f1d6ac54919': '2195597081860488093659',
  '0xb0dfd28d3cf7a5897c694904ace292539242f858': '5470600000000000000000',
  '0x3472a5a71965499acd81997a54bba8d852c6e53d': '0',
  '0x0cec1a9154ff802e7934fc916ed7ca50bde6844e': '335400609558747166862488',
  '0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2': '18033361926134999892'
}

Input 2: (1614124163, 11916434)

Output 2:
{
  '0x1f9840a85d5af5bf1d1762f925bdaddc4201f984': '607534430161781994697635',
  '0x6b175474e89094c44da98b954eedeac495271d0f': '33933834806526125987455748',
  '0x3449fc1cd036255ba1eb19d65ff4ba2b8903a69a': '948456261968725066468',
  '0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9': '0',
  '0x6b3595068778dd592e39a122f4f5a5cf09c90fe2': '0',
  '0x56687cf29ac9751ce2a4e764680b6ad7e668942e': '0',
  '0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48': '36389198274305',
  '0x57bc752ec42238bb60a6e65b0de82ef44013225d': '74305738252165754257',
  '0x334cbb5858417aee161b53ee0d5349ccf54514cf': '0',
  '0x57ab1ec28d129707052df4df418d58a2d46d5f51': '0',
  '0x1494ca1f11d487c2bbe4543e90080aeba4ba3c2b': '0',
  '0xc00e94cb662c3520282e6f5717214004a7f26888': '13347137704383655139121',
  '0x0391d2021f89dc339f60fff84546ea23e337750f': '6571262578549837484949',
  '0x03ab458634910aad20ef5f1c8ee96f1d6ac54919': '70426514325056410426985',
  '0xb0dfd28d3cf7a5897c694904ace292539242f858': '8546600000000000000000'
}
```
{% endtab %}

{% tab title="CLI" %}
```
$ npx @defillama/sdk projects/pooltogether/index.js

Couldn't find the price of token at 0x0cb42d5c9c4661fee5d2ecfcbad5bccefa0727a9, assuming a price of 0 for it...
Couldn't find the price of token at 0x58014f7eef95bcaaf84b8f4aa07507e1664e40b2, assuming a price of 0 for it...
Couldn't find the price of token at 0x57bc752ec42238bb60a6e65b0de82ef44013225d, assuming a price of 0 for it...
Couldn't find the price of token at 0xa323fc62c71b210e54171887445d7fca569d8430, assuming a price of 0 for it...
Couldn't find the price of token at 0x334cbb5858417aee161b53ee0d5349ccf54514cf, assuming a price of 0 for it...
BAT                       113.31960936462058
UNI                       15.65 M
DAI                       49.09 M
USDT                      0
EM3                       0
BAC                       275.1357801219835
IDT                       0
AAVE                      0
SUSHI                     161.93481636936156
BONDLY                    66.85 k
JAMM                      0
USDC                      56.98 M
ARTO                      0
WBTC                      5.73 k
UNI-V2                    0
PcDAI                     0
ZRX                       198.53 k
sUSD                      0
DPI                       751.94 k
LON                       36.01 k
COMP                      5.76 M
BOND                      745.42 k
RAI                       6.52 k
LOTTO                     1.20 k
BADGER                    0
POOL                      8.69 M
WETH                      29.87 k
Total: 138.01 M
```
{% endtab %}
{% endtabs %}

#### Alpha Homora (BSC)

{% tabs %}
{% tab title="Code" %}
{% code title="projects/alpha-homora/index.js" %}
```javascript
const sdk = require("@defillama/sdk");
const BigNumber = require("bignumber.js");
const { request, gql } = require("graphql-request");
const { unwrapUniswapLPs } = require("../helper/unwrapLPs")

const GET_GOBLIN_SUMMARIES = gql`
  query GET_GOBLIN_SUMMARIES($block: Int) {
    goblinSummaries(block: { number: $block }) {
      id
      totalLPToken
    }
  }
`;
const wBNB = '0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c'

function getBSCAddress(address) {
  return `bsc:${address}`
}

async function tvl(timestamp, blockETH, chainBlocks) {
  const block = chainBlocks["bsc"];
  const balances = {}
  const {
    goblinSummaries
  } = await request('https://api.thegraph.com/subgraphs/name/hermioneeth/alpha-homora-bank-bsc', GET_GOBLIN_SUMMARIES, {
    block,
  });
  const lpTokens = (await sdk.api.abi.multiCall({
    block,
    abi: { "constant": true, "inputs": [], "name": "lpToken", "outputs": [{ "internalType": "contract IUniswapV2Pair", "name": "", "type": "address" }], "payable": false, "stateMutability": "view", "type": "function" },
    calls: goblinSummaries.map(goblin => ({
      target: goblin.id
    })),
    chain: 'bsc'
  })).output
  await unwrapUniswapLPs(balances, goblinSummaries.map(goblin => {
    const lpToken = lpTokens.find(call => call.input.target === goblin.id).output;
    return {
      token: lpToken,
      balance: goblin.totalLPToken
    }
  }),
    block,
    'bsc',
    (addr) => `bsc:${addr}`)
  const unusedBNB = await sdk.api.eth.getBalance({
    target: '0x3bB5f6285c312fc7E1877244103036ebBEda193d',
    block,
    chain: 'bsc'
  })
  balances[getBSCAddress(wBNB)] = BigNumber(balances[getBSCAddress(wBNB)] || 0).plus(BigNumber(unusedBNB.output)).toFixed(0)
  return balances
}

module.exports = {
  bsc:{
    tvl,
  },
  tvl,
};

```
{% endcode %}
{% endtab %}

{% tab title="Calls" %}
```
Input 1: (1616785946, 12116483)

Output 1:
{
  'bsc:0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c': '558286715809884029214427',
  'bsc:0xF8A0BF9cF54Bb92F17374d9e9A321E6a111a51bD': '6541688163793370635285',
  'bsc:0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56': '19291091455439572439988346',
  'bsc:0x2170Ed0880ac9A755fd29B2688956BD959F933F8': '12368452624118152630273',
  'bsc:0x88f1A5ae2A3BF98AEAF342D26B30a79438c9142e': '414899373065096234',
  'bsc:0x55d398326f99059fF775485246999027B3197955': '21036251365092442220343193',
  'bsc:0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c': '776608041991790197333',
  'bsc:0xa1faa113cbE53436Df28FF0aEe54275c13B40975': '4144144112086134531012116',
  'bsc:0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82': '867542163078831908999094',
  'bsc:0xAD6cAEb32CD2c308980a548bD0Bc5AA4306c6c18': '152970232357957589510610',
  'bsc:0xBf5140A22578168FD562DCcF235E5D43A02ce9B1': '8361293755666516877876'
}
```
{% endtab %}

{% tab title="CLI" %}
```
$ npx @defillama/sdk projects/alpha-homora/index.js

Couldn't find the price of token at bsc:0x88f1A5ae2A3BF98AEAF342D26B30a79438c9142e, assuming a price of 0 for it...
Couldn't find the price of token at bsc:0xAD6cAEb32CD2c308980a548bD0Bc5AA4306c6c18, assuming a price of 0 for it...
WBNB                      139.28 M
LINK                      175.78 k
BUSD                      19.28 M
ETH                       20.50 M
YFI                       0
USDT                      21.04 M
BTCB                      41.56 M
ALPHA                     7.87 M
Cake                      12.48 M
BAND                      0
UNI                       236.37 k
Total: 262.42 M

```
{% endtab %}
{% endtabs %}

#### WBTC

{% tabs %}
{% tab title="Code" %}
{% code title="projects/wbtc/index.js" %}
```javascript
const sdk = require('@defillama/sdk');
const wbtcContract = "0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599"

async function tvl(timestamp, block) {
  const totalSupply = (await sdk.api.erc20.totalSupply({
    block,
    target: wbtcContract,
  })).output;

  return { [wbtcContract]: totalSupply };
}

module.exports = {
  ethereum:{
    tvl,
  },
  tvl,
};
```
{% endcode %}
{% endtab %}

{% tab title="Calls" %}
```
Input 1: (1616786616, 12116526)

Output 1:
{
  '0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599': '13790365865712'
}
```
{% endtab %}

{% tab title="CLI" %}
```
$ npx @defillama/sdk projects/wbtc/index.js

WBTC                      7.41 B
Total: 7.41 B
```
{% endtab %}
{% endtabs %}
