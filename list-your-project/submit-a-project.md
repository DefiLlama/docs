# How to list a DeFi project

The majority of adapters on DefiLlama are contributed and maintained by their respective communities, with all changes being coordinated through the [DefiLlama/DefiLlama-Adapters](https://github.com/DefiLlama/DefiLlama-Adapters) github repo.

{% hint style="info" %}
Do you want to list a project on our yields or stablecoin dashboard? Check the following guides then:

* [Yields](https://github.com/DefiLlama/yield-server/blob/master/README.md)
* [Stablecoin](https://github.com/DefiLlama/peggedassets-server/blob/master/README.md)
* [Liquidations](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/liquidations/README.md)
* [Volume and fees dashboards](other-dashboards/)
{% endhint %}

If you'd like to list a DeFi project on DefiLlama:

1. Fork the [Adapters repo](https://github.com/DefiLlama/DefiLlama-Adapters) (button towards the top right of the repo page).
2. Add a new folder with the same name as the project to projects/.
3. Write an [SDK adapter](how-to-write-an-sdk-adapter.md) in the new folder.
4. Make a Pull Request with the changes on your fork, to the main DefiLlama Adapters repo, with a brief explanation of what you changed.
5. Wait for someone to either comment on or merge your Pull Request. There is no need to ask for someone to check your PR as there a monitored regularly.
6. Once your PR has been merged, please give 24 hours for the front-end team to load your listing onto the UI.

## How to build an adapter

And adapter is just some code that:

1. Collects data on a protocol by calling some endpoints or making some blockchain calls
2. Computes the TVL of a protocol and returns it

### Next steps

You probably need to write an SDK adapter, for which you could use the following guide:

{% content-ref url="how-to-write-an-sdk-adapter.md" %}
[how-to-write-an-sdk-adapter.md](how-to-write-an-sdk-adapter.md)
{% endcontent-ref %}
