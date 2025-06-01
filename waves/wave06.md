# :rowboat: Wave 06
> Ship! Ship! Ship!

Finally ship the project, god damn!

## The Plan

- [ ] Publish stories and store on aws (via local ipfs or otherwise)
- [ ] Query LLM for initial hue (mood) and saturation (intensity) values of the story
- [ ] Implement a "random walk" for the users as a discovery mechanism
- [ ] Define initial token drip rate (as a function of the random walk)
- [ ] "Commit reading history to mainnet"
- [ ] Set up an indexer to track network information on AWS
- [ ] Deploy the following contracts on base-sepolia (using CREATE2)
  - [ ] SemicolonPersona :: The main wallet for the user
  - [ ] SemicolonPersonaFactory :: A factory contract that deploys the Personas using CREATE2
  - [ ] SemicolonToken :: The main token contract
  - [ ] SemicolonStory :: The contract that holds the story published to the network; is an ERC-721
  - [ ] SemicolonStoryFactory :: A factory contract that deploys the Stories using CREATE2
  - [ ] SemicolonTreasury :: The main treasury contract for the network
  - [ ] SemicolonAdmin :: effectively a state manager for the network
  - [ ] ReverseProxySMF :: A ENS reverse proxy to provide human readable names for the Personas

## Updates

- updates
- to the plan
- over the
- course of
- the wave