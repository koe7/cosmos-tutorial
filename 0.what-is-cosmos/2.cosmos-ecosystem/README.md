# The Cosmos Ecosystem

- BFT 합의 알고리즘에 의해 돌아감
- value / token transfer, 체인간 커뮤니케이션, 체인 주권 보호
- 구성요소들 : 프로토콜, SDK, 토큰, 지갑, 어플리케이션, 서비스

## A whole universe to discover: tokens, wallets, apps, and services
- token including NFT
- Binance Token, Terra, ATOM
- 코스모스는 얼마나 많은 가치들을 관리하고 있을까
  - 2022년 1월 기준 1000억달러(한화 120조 정도)
  - 지갑이랑 block explorer만 해도 35개

## The Cosmos SDK: Modular and Customizable
- 간단한 블록체인 개발에 집중시켜줌
- Customizable & Interoperable chin development
- 3 Points of Cosmos SDK
  - Application-specific blockchains
    - 이전과 달리 컨트랙 대비 체인 개발 난이도가 낮아져서 이러한 것이 가능
  - Fast-finality BFT consensus algorithm
    - 공동의 TBFT 알고리즘이 코스모스로 만들어진 모든 체인에 적용
  - Ecosystem for easy blockchain development
- Go로 구현된 modular framework for application-specific blockchains.
- 디자인 원칙 2가지
  - Modularity
    - Easy to import/adapt
    - 생태계가 커지면서 모듈도 많아지고 이는 개발을 더 용이하게 해준다.
  - Capability-based security
    - Modular component 기반으로 구축되었을때 상상될 수 있는 공격에 대해, 실질적으로 불가하므로 걱정할 필요 없다는 얘기. 세부사항 생략

## The Inter-Blockchain Communication Protocol
- IBC는 Cosmos에서 interoperability의 근간
- 텐더민트의 즉시완결성(fast-finality)
  - value/token을 가능하게 함
  - 체인간 커뮤니케이션도 가능하게 함
  - validator 달라도 서로 상호운용 가능
- IBC 없이 체인간 커뮤니케이션이 힘든 이유
  - Consensus / Network / Application layer가 각자 다 다른 방식으로 구현되어 있기 때문!
  - Cosmos 기반 체인은 Consensus+Network layer가 tendermint로 같다
- IBC의 블록체인 클래스 : Hub 와 Zone
  - Zone
    - 서로 다른 블록체인. 계정 인증, 트랜잭션, 토큰 생성 및 분배, 트랜잭션 실행 등 모두 각자 실행
  - Hub
    - 각 존에 IBC를 통해 연결.
    - Hub의 존재로 모든 체인이 직접 연결될 필요가 없어진다.
- Cosmos Hub
  - 첫번째 Hub. PoS 블록체인이면서 native token으로 ATOM을 가지고 있다.
  - 각 zone 간 연결을 가능하게 해준다. 수수료를 다른 토큰으로도 받을 수 있다.
- Connection to non-Tendermint
  - 아니어도 fast-finality면 가능
- Connection to probablistic-finality chain
  - Peg zone이라는 proxy chain 필요. peg zone이 fast-finality가지고 있어야하고, 각 체인상태를 계속 트래킹해야함.
  - Peg zone은 IBC에 compatible해야함. Bridge 역할

## Starport: building application-specific blockchains with one command
- Tendermint & Cosmos SDK를 CLI로 간단하게. 빌드, 테스트, 런치, 다 편하다. 95%의 개발 프로세스 절약.
- Devnet에서 IBC이용해서 다른 체인이랑 연결 가능하다. 토큰 주고받는 것도 가능하다.
- 프론트엔드 패키지도 만들어준다! (Javascript/typescript/Vue.js)
- 키 관리나 밸리데이터 만드는 거나 토큰 트랜스퍼도 다 CLI로 가능

## Cosmwasm: Multi-chain smart contracts
- Cosmos SDK 기반 체인에서 컨트랙 개발 및 테스팅을 편하게 해줌

## The possibility of using alternative blockchains frameworks and SDKs
코스모스 SDK는 모듈 형식이기 때문에, Go로 구현된 코드를 Cosmos SDK에 포팅 가능하다.
예를 들어, Ethermint를 사용하면, 개발자는 Go Ethereum client의 EVM(Ethereum Virtual Machine)을 Cosmos SDK 모듈로 사용할 수 있다.
이로인해, 개발자들은 솔리디티 컨트랙을 코스모스 에코시스템과 상호작용하게 할 수 있다.
이러면 코스모스 체인에서 이더리움 코드를 돌리면서 TBFT를 사용할 수 있게 된다.
