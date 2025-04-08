# :chains: Wave 01
> Initial plan and v1 architecture

The goal of this wave was to understand how to best incorporate Zora's Coins Protocol into
Semicolon Fingers. This has resulted in a clear versioning of the project into two parts, each 
version providing a new way to interact with the Coins Protocol.

Additionally, integrating Coins may be the key to making Semicolon Fingers self sustainable, as 
well as allowing the project to bootstrap itself (as a result of 
[Zora rewards](https://support.zora.co/en/articles/2509953))! See [tokenomics](#tokenomics) below 
for more details.

## Versioning

The versions can be thought of in two ways, the core feature enabling Semicolon Fingers (ie, 
[mvp](#mvp-v1) or [sentiment analysis](#svm-v2)), or which side of the network gets access to 
Coins (ie, [readers](#reader-trails) and then [writers](#writer-ngrams)).

### MVP (v1)

This version exists entirely on Base. It is a collection of smart contracts, on base-sepolia and 
base-mainnet that enable the network. See [contracts below](#contracts) for more information about 
the contracts. In this version, only the readers are able to mint Zora Coins, and them doing so
is treated as a _commit_ of their reading history on the network, and helps bridge the tokens
created as a result of their reading to the mainnet.

A key feature of this version is also the data that is collected as a result of normal operations
of the network. Each time a writer publishes a story, they select an emotion and how relieved
they feel on submitting a story. On the reader side, a reader is prompted for their perceived
emotion, intensity, and _heaviness_ of the story they are reading / have read. As a result, each
published story is expected to have multiple data points for the mood / emotion (hue), intensity
(saturation), and perceived heaviness (lightness). These points will help train an onchain model,
in a supervised fashion in [v2](#svm-v2).

A key deliverable of this section will be the user interface(s). Currently, there are three 
interfaces planned, the writer platform ([empty-your-mug](https://emptyyourmug.com/)), the reader
platform ([pull-my-thread](https://pullmythread.com/)), and the account mgmt / main platform
([semicolon-fingers](https://semicolonfingers.com/)). There is another backend feature powering
auth which will also be completed in [wave 02](./wave02.md). Additionally, the main platform will
have a native way to trade Zora Coins, especially those created by readers.

Finally, a bunch of services will remain centralized in this version. For instance, an indexer
will be used to keep track of tokens being created on the testnet and then call a function on
the treasury contract on the mainnet, when a reader creates a Zora Coin. Eventually, these services
should, and will move onchain, and be more decentralized in nature. One thing to note however, is 
that this project / network is aimed at creating an application of value, and it happens to use
web3 as its backend. The goal here is not to be a decentralization maxi, but instead to showcase
the power of decentralization.

### SVM (v2)
> \* the details of this are _tbd_, will be finalized over [wave 05](./wave05.md)

The main characteristic of this version is an L3 rollup that is purpose built around an onchain
support vector machine (svm) for rudimentary sentiment analysis. Version 1 collects a bunch of
data and stores it on the testnet contracts. For every story, we have a original emotion, and many
_perceived_ emotions from the writers. This version finally uses those data points to train an
onchain svm model, where every new story can be classified into an emotion. More realistically,
there will be 4 machines created for the 8 base emotions, arranged to be opposite to each other
by [this wheel](../assets/wheel-of-emotions.png) (created by [Rober 
Plutchik](https://en.wikipedia.org/wiki/Robert_Plutchik#Plutchik's_wheel_of_emotions)).

## Zora Coins integration

Zora Coins will be available to create for both, readers and writers. Each type of Coin will serve
a different purpose, and power Semicolon Fingers accordingly.

### Reader Trails

On the reader side of the network, the readers are presented with a random story which they can 
shuffle out of. This shuffling is a [random walk](https://en.wikipedia.org/wiki/Random_walk) on 
the color wheel and the distance travelled is determined by the time taken before shuffling, and 
how much of the current story is read. Additionally, since each story is converted to a telescopic-
text format, and the reader must click to reveal new parts of the story, the depth of reading is 
also tracked. Hence, we can recreate a reader's journey at the time of their _commit_ or 
submission, and mint a Coin based on it. This incentivizes readers to come into the network
and read stories, and also commit them from time to time.

A key aspect of the network that this _committing_ enables is the distribution of network tokens
on the mainnet. Until the reader commits, the network tokens are dripped to the writers (as their
stories are read) on the testnet, however, on interacting with the mainnet (to create Coins), the 
reader brings all of _their_ activity on mainnet. See [tokenomics](#tokenomics) below for more 
details.

### Writer Ngrams*
> \* the details of this are _tbd_, will be finalized over [wave 05](./wave05.md)

On the writer side of the network, the writer's words will be tokenized and used to train a 
sentiment analysis model. Currently, this model is a support vector machine and the tokens are 
[n-grams](https://en.wikipedia.org/wiki/N-gram) of size 3 or 4. These n-gram tokens are hashed
and stored as bytes2 initially, which means that there will only be 65535 unique n-gram tokens
before collisions occur. Hence, there will be only that many new Coins created, at least in the
first version of the SVM.

## Architecture Overview

Semicolon Fingers [v1](#mvp-v1) will reside entirely on Base, sepolia and mainnet. For the rollup
architecture, it is best to see [wave 05](./wave05.md).

### Contracts
> Please track the progress on the contracts in the [specific 
repository](https://github.com/sliver-labs/semicolon-forge)

The following contracts will power the entire network,
- [`SemicolonPersona`](https://github.com/sliver-labs/semicolon-forge/issues/2) This contract is
deployed for every user and represents their account on the network
- [`SemicolonStory`](https://github.com/sliver-labs/semicolon-forge/issues/4) This contract is
deployed for every story created on the network.
- [`SemicolonToken`](https://github.com/sliver-labs/semicolon-forge/issues/6) This contract is
the main ERC20 for the network.
- [`SemicolonTreasury`](https://github.com/sliver-labs/semicolon-forge/issues/7) This contract is
the treasury contract where users can perform a _burn-to-claim_ action and withdraw USDC (or other
tokens, _tbd_).
- [`SemicolonAdmin`](https://github.com/sliver-labs/semicolon-forge/issues/13) This contract is
the main manager contract for the entire network. The network, in order to remain free, plays
with the concepts of testnets and mainnets, and this contract provides the necessary tooling to
bridge between the two, and eventually the rollup.
- [_`SemicolonTrail`_](https://github.com/sliver-labs/semicolon-forge/issues/8) This contract is
planned to represent the Coins created by readers. However, this contract may not need to exist.
- [_`SemicolonNgram`_](https://github.com/sliver-labs/semicolon-forge/issues/9) Similarly, this 
contract is planned to represent the Coins created by writers. However, this contract may not need
to exist.
- [`SemicolonGas`](https://github.com/sliver-labs/semicolon-forge/issues/12) This contract is about
the gas token for the eventual rollup. More on this in [wave 05](./wave05.md).

### Web2

The network will be accessible through three portal, the [account mgmt 
website](https://semicolonfingers.com), the [writer portal](https://emptyyourmug.com),
and the [reader portal](https://pullmythread.com). Additionally, there will be an auth service
that manages user signups and logins. This is done deliberately to keep the feel of the porject
very Web2 like. For instance, one decision made here is to include a SIWE login, as the goal of 
the project is to onboard new people into crypto. Of course, existing crypto native folk will be
able to add their wallets to their profiles. Another key decision made here is to deploy a new
Modular Smart Wallet for each user, and have that be the main interface for the users with the 
network.

### Tokenomics

The tokenomics of the network revolve around two tokens — `SemicolonToken` and `SemicolonGas`. The
former is the primary token, motivating writers to share their stories. When readers read a story,
new tokens are created for the writers of that story, on the testnet. At some point, the readers 
are expected to _commit_ their reading history on mainnet which can happen in one of two ways,
they can either create a Coin about their reading path or they can mint or collect a particular
story. In either case, they are expected to pay a certain amount (`USDC` for now) to the treasury
on the mainnet, which triggers an airdrop of the `SemicolonToken` to the respective writers. In 
fact, the amount they provide also prices the tokens that were created as a result of their reading 
journey.

The `SemicolonGas` token exists to incentivize running a rollup node. The rollup is planned to be 
a purpose driven chain around sentiment analysis, and as such will include some precompiles to 
assist in the machine learning. This will likely increase compute costs for the node operator, and 
the `SemicolonGas` token exists to compensate them for it.

**Where does Zora fit in?**

Zora's Coins Protocol is an essential part of this architecture. For the first token, readers can 
freely create Coins around their reading history which incentivises them to continually _commit_
their history on mainnet, thereby ensuring that there not a lot of delay in the writers being able
to use the `SemicolonToken`s they earn. Additionally, owing to the rewards program by Zora, any 
potential trades that happen on these Coins helps fund the network over the long term. Yes, an 
individual Coin may not provide enough `WETH/ETH` to sustain the network, however, when scaled over
hundreds of readers each having hundreds of Coins, there should be enough available in the rewards
that helps run the network effectively.

Similarly, on the writers' side, each unique token they submit to the network gets created as a 
Coin. This incentivizes writers to write unique stories, and also be the first at writing them, 
thereby bringing in early adoption for the project. Having said that, the potential for abuse is a 
little high here, and no elegant solution exists right now. As the initial model can support only
65535 unique tokens, and there being a delay (of about two months) in creating Coins, the hope 
is that the initial stories are sufficient in filling up the 65535 slots. The intial stories are 
not written with the expectation of Coins, but instead for the expectation of `SemicolonToken`s.

Finally, as the Coins Protocol allows markets to be created with any token, a potential way this 
can manifest in Semicolon Fingers is the reader-trail Coins be traded with `SemicolonToken` and 
the writer-ngram Coins be traded with `SemicolonGas`. This would directly help in pricing each 
token of the network, but this is something to be decided upon in [wave 05](./wave05.md) after 
considering the game theory aspects of it all.