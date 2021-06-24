# BSC projects and other chains

### SDK methods

All the methods on the SDK support the optional argument `chain`, which can be set to `bsc` to make that method operate using binance chain nodes instead of ethereum ones:

```javascript
 sdk.api.abi.multiCall({
    calls: [...]
    abi: 'erc20:balanceOf',
    block: 12116271,
    chain: 'bsc'
 });
```

### Token balances

When returning the balances of Binance Chain tokens, the token addresses should be prefixed by `bsc:`, as that allows us to determine the chain where each token is from:

```javascript
{
  // Binance Smart Chain addresses
  'bsc:0xF8A0BF9cF54Bb92F17374d9e9A321E6a111a51bD': '6534470774016144468787',
  'bsc:0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56': '19281233295933226615043458',
  'bsc:0x2170Ed0880ac9A755fd29B2688956BD959F933F8': '12373186859845702202214',
  
  // Ethereum addresses
  '0xAD6cAEb32CD2c308980a548bD0Bc5AA4306c6c18': '152991840161436725823207',
  '0xBf5140A22578168FD562DCcF235E5D43A02ce9B1': '8314540194529016809181'
}
```

### Chains supported

Here's the full list:

* `ethereum`
* `bsc`
* `polygon`
* `heco`
* `fantom`
* `rsk`
* `tomochain`
* `xdai`
* `avax`
* `wan`
* `harmony`
* `thundercore`
* `okexchain`

If you'd like us to add support for any other chain just let us know on discord!

