# How to add a new chain

Check if your chain is already part of the rpc list that we collected:

https://unpkg.com/@defillama/sdk/build/providers.json

If no, then you need to add your chain's unique label to [chains.json](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/helper/chains.json) and add the rpc with key "(chain name)_RPC" to [env.js](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/helper/env.js#L6)


For example, to add a chain called `llama` with rpcs `https://rpc1.llama.defi`, `https://rpc2.llama.defi`

  1.  add "llama" to [`chains.json`](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/helper/chains.json)
  2.  Add new entry to [`env.js`](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/helper/env.js#L6)

```javascript
LLAMA_RPC: "https://rpc1.llama.defi,https://rpc2.llama.defi"
``` 


**NOTE:** Adding new chain config doesnt automatically list it in Defillama, one would still need to add an adapter for defi app on given chain for it to show up