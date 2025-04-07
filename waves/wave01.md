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

### Web2

## User Flow

The user interacts with the network in two primary ways, the [writer side](https://emptyyourmug.com/) 
or the [reader side](https://pullmythread.com/). Their account, and related actions, live on the 
[primary domain](https://semicolonfingers.com), however, this part will be built out in 
[wave 04](./wave04.md).

### Client side

### Tokenomics