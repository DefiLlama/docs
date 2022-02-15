# How to list a DeFi project

The majority of adapters on DefiLlama are contributed and maintained by their respective communities, with all changes being coordinated through the [DefiLlama/DefiLlama-Adapters](https://github.com/DefiLlama/DefiLlama-Adapters) github repo.



If you'd like to list a DeFi project on DefiLlama:

1. Fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page).
2. Add a new folder with the same name as the project to projects/.
3. Write an [SDK adapter](how-to-write-an-sdk-adapter.md) (or a [fetch adapter](how-to-write-a-fetch-adapter.md) if you cant use the SDK for this project) in the new folder.
4. Make a Pull Request with the changes on your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
5. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR as there a monitored regularly.

## How to build an adapter

And adapter is just some code that:

1. Collects data on a protocol by calling some endpoints or making some blockchain calls
2. Computes the TVL of a protocol and returns it

#### Types of adapters

Right now there's two types of adapters co-existing within the repository:

* Fetch adapters: These calculate the TVL directly and just export a `fetch` method
* SDK adapters: These use the SDK and return all the assets locked along with their balances

{% hint style="info" %}
Our SDK is fully compatible with DefiPulse's, so the adapters developed for defillama can also be directly submitted to DeFiPulse, no need repeat the same work!
{% endhint %}

#### Which adapter type should I develop?

Right now our SDK only supports EVM chains, so if your project is in any of these chains you should develop a SDK-based adapter, while if your project is on another chain a fetch adapter is likely the way to go. If your project is not on an EVM chain but you are able to give us historical data, we can help support this if you message us in Discord.

### Next steps

You probably need to write an SDK adapter, for which you could use the following guide:

{% content-ref url="how-to-write-an-sdk-adapter.md" %}
[how-to-write-an-sdk-adapter.md](how-to-write-an-sdk-adapter.md)
{% endcontent-ref %}
