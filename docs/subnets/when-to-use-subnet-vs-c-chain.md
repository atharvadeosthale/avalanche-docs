# When to Build on a Subnet vs. on the C-Chain

A Subnet is a subset of Avalanche Primary Network validators agreeing to run the same [Virtual
Machines (VM)](/docs/learn/avalanche/subnets-overview.md#virtual-machines) with its own rules. Subnet
enables extra dimensions of reliability, efficiency, and data sovereignty. It provides the ability
to create custom blockchains for different use cases, while isolating high-traffic applications from
congesting activity on the Primary Network. But such flexibility comes with its own set of
tradeoffs. In this article, we discuss often-overlooked differentiating characteristics of Subnet,
with primary focus on EVM-based applications (for example, C-Chain,
[Subnet-EVM](https://github.com/ava-labs/subnet-evm)). The goal is to identify pros and cons of
building an app on C-Chain versus [Subnet-EVM](https://github.com/ava-labs/subnet-evm), and help
developers make more informed decisions.

## When to Use a Subnet

There are many advantages to running your own Subnet. If you find one or more of these a good match
for your project then a Subnet might be a good solution for you. But make sure to check the reasons
to use the C-Chain instead, as some trade-offs involved might make that a preferred solution.

### We Want Our Own Gas Token

C-Chain is an Ethereum Virtual Machine (EVM) chain; it requires the gas fees to be paid in its
native token. That is, the application may create its own utility tokens (ERC-20) on the C-Chain,
but the gas must be paid in AVAX. In the meantime,
[Subnet-EVM](https://github.com/ava-labs/subnet-evm) effectively creates an application-specific
EVM-chain with full control over native(gas) coins. The operator can pre-allocate the native tokens
in the chain genesis, and mint more using the [Subnet-EVM](https://github.com/ava-labs/subnet-evm)
precompile contract. And these fees can be either burned (as AVAX burns in C-Chain) or configured to
be sent to an address which can be a smart contract.

Note that the Subnet gas token is specific to the application in the chain, thus unknown to the
external parties. Moving assets to other chains requires trusted bridge contracts (or upcoming cross
Subnet communication feature).

### We Want Higher Throughput

The primary goal of the gas limit on C-Chain is to restrict the block size and therefore prevent
network saturation. If a block can be arbitrarily large, it takes longer to propagate, potentially
degrading the network performance. The C-Chain gas limit acts as a deterrent against any system
abuse but can be quite limiting for high throughput applications. Unlike C-Chain, Subnet can be
single-tenant, dedicated to the specific application, and thus host its own set of validators with
higher bandwidth requirements, which allows for a higher gas limit thus higher transaction
throughput. Plus, [Subnet-EVM](https://github.com/ava-labs/subnet-evm) supports fee configuration
upgrades that can be adaptive to the surge in application traffic.

Subnet workloads are isolated from the Primary Network; which means, the noisy neighbor effect of
one workload (for example NFT mint on C-Chain) cannot destabilize the Subnet or surge its gas price.
This failure isolation model in the Subnet can provide higher application reliability.

### We Want Strict Access Control

The C-Chain is open and permissionless where anyone can deploy and interact with contracts. However,
for regulatory reasons, some applications may need a consistent access control mechanism for all
on-chain transactions. With [Subnet-EVM](https://github.com/ava-labs/subnet-evm), an application can
require that “only authorized users may deploy contracts or make transactions.” Allow-lists are only
updated by the administrators, and the allow list itself is implemented within the precompile
contract, thus more transparent and auditable for compliance matters.

### We Need EVM Customization

If your project is deployed on the C-Chain then your execution environment is dictated by the setup
of the C-Chain. Changing any of the execution parameters means that the configuration of the C-Chain
would need to change, and that is expensive, complex and difficult to change. So if your project
needs some other capabilities, different execution parameters or precompiles that C-Chain does not
provide, then Subnets are a solution you need. You can configure the EVM in a Subnet to run however
you want, adding precompiles, and setting runtime parameters to whatever your project needs.

## When to Use the C-Chain

All the reasons for using a Subnet outlined above are very attractive to developers and might make
it seem that every new project should look into launching a Subnet instead of using the C-Chain. Of
course, things are rarely that simple and without trade-offs. Here are some advantages of the
C-Chain that you should take into account.

### We Want High Composability with C-Chain Assets

C-Chain is a better option for seamless integration with existing C-Chain assets and contracts. It
is easier to build a DeFi application on C-Chain, as it provides larger liquidity pools and thus
allows for efficient exchange between popular assets. A DeFi Subnet can still support composability
of contracts on C-Chain assets but requires some sort of oﬀ-chain system via the bridge contract. In
other words, a Subnet can be a better choice if the application does not need high composability
with the existing C-Chain assets. Plus, the upcoming support for cross Subnet communication will
greatly simplify the bridging process.

### We Want High Security

The security of Avalanche Primary Network is a function of the security of the underlying validators
and stake delegators. Some choose C-Chain in order to achieve maximum security by utilizing
thousands of Avalanche Primary Network validators. Some may choose to not rely on the entire
security of the base chain.

The better approach is to scale up the security as the application accrues more values and adoption
from its users. And Subnet can provide elastic, on-demand security to take such organic growth into
account.

### We Want Low Initial Cost

C-Chain has economic advantages of low-cost deployment, whereas each Subnet validator is required to
validate the Primary Network by staking AVAX (minimum 2,000 AVAX for Mainnet). For fault tolerance,
we recommend at least five validators for a Subnet, even though there is no requirement that the
Subnet owner should own all these 5 validators, it still further increases the upfront costs.

### We Want Low Operational Costs

C-Chain is run and operated by thousands of nodes, it is highly decentralized and reliable, and all
the infrastructure (explorers, indexers, exchanges, bridges) has already been built out by dedicated
teams that maintain them for you at no extra charge. Project deployed on the C-Chain can leverage
all of that basically for free. On the other hand, if you run your own Subnet you are basically in
charge of running your own L1 network. You (or someone who you partner with or pay to) will need to
do all those things and you will ultimately be responsible for them. If you don't have a desire,
resources or partnerships to operate a high-availability 24/7 platform, you're probably better off
deploying on the C-Chain.

## Conclusion

Here we presented some considerations both in favor of running your own Subnet and in favor of
deploying on the C-Chain. You should carefully weigh and consider what makes the most sense for your
and your project: deploying on a Subnet or deploying on the C-Chain.

But, there is also a third way: deploy on C-Chain now, then move to your own Subnet later. If an
application has relatively low transaction rate and no special circumstances that would make the
C-Chain a non-starter, you can begin with C-Chain deployment to leverage existing technical
infrastructure, and later expand to a Subnet. That way you can focus on working on the core of your 
project and once you have a
solid product/market fit and have gained enough traction that the C-Chain is constricting you, plan
a move to your own Subnet.

Of course, we're happy to talk to you about your architecture and help you choose the best path
forward. Feel free to reach out to us on [Discord](https://chat.avalabs.org) or other [community
channels](https://www.avax.network/community) we run.
