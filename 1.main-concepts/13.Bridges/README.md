# Bridges

### The Gravity Bridge
- ETH의 ERC-20 token을 그대로 이용하면서 Cosmos의 빠르고 싼 computation을 이용할 수 있는 Bridge

### How it works
- components
  - Gravity.sol. An Ethereum smart contract on the Ethereum blockchain.
  - Cosmos Gravity module. A Cosmos module designed to run on the Cosmos Hub.
  - Orchestrator. A program that runs on Cosmos validators monitoring the Ethereum chain and submitting events that occur on Ethereum to Cosmos as messages.
  - Relayers. A network of nodes that compete for the opportunity to earn fees for sending transactions on behalf of the Cosmos validators.

- ETH에서 Cosmos로의 Token (Erc20) 이동은 ETH쪽에서 Token을 lock하고 cosmos쪽에서 발행되는 방식, 반대 방향으로의 Transfer는 cosmos에서 token이 distory되고 ETH 쪽에서 기존에 예치된 token이 release 되는 
방식

### Key design components: trust in integrity
- validator의 잘못된 signing 시 Cosmos Chain Slashing을 통해 패널티를 받음
- 또한, peg-zone validator들은 신뢰된 이더리움 노드를 유지해야함

### Operational parameters ensuring security
- ETH smart contract 에는 주기적으로 Cosmos의 Validator Set을 업데이트해줘야함
- cosmos validator set 업데이트를 통해 fraudulent/malicious validator 를 피할 수 있음

