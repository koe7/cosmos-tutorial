# Blockchain Technology and Cosmos
- Intro
  - 블록체인 기술에 대해 간단히 리뷰
  - Cosmos는 어쩌다가 등장했는가
  - Cosmos는 다음 블록체인 기술 영역들에 어떻게 기여했는가
    - An internet of blockchains
    - A better decentralized application (dApp) user experience
    - A simplified, specialized dApp development experience
    - Facilitated scalability
    - Increased sovereignty
    - Speed and fast finality
> What is a Blockchain ?  
블록체인 프로토콜은 상태(state)를 유지하는 프로그램을 정의하고, 수신된 입력(transaction)에 따라 상태를 수정하는 방법을 설명.  
Consensus mechanism은 블록체인에게 표준 트랜잭션 이력(a canonical transaction history) 보장.  
Blockchain transaction은 deterministic 해야함.(같은 genesis state로 똑같이 transaction 반복하면 결국 같은 state)  
일반적으로 3개의 구조로 나눠짐: Network layer / Consensus layer / Application layer  

## The world of blockchains - From public general-purpose chains to application speicific chains
- 1980~1990년대 이미 블록체인 기술의 구성 요소들 생김
  - append-only, provably correct transaction logs using built-in error checking
  - strong authentication and encryption using public keys
  - mature theories of fault-tolerant systems
  - a widespread understanding of P2P systems
  - the advent of the internet and ubiquitous connectivity
  - powerful client-side computers
- 2008년 사토시 나카모트 디지털 화폐를 위한 P2P network 제안 : 비트코인  
> More on PoW
  - 비트코인 개발로 디앱 개발 시작
  - Monolithic한 코드와 제한된 스크립트 언어
    - 디앱 개발은 지루하고 복잡해
- 2013년 general-purpose blockchains Ethereum 등장
  - Decentralized network 제공에 초점
  - 스마트 컨트랙 기능!
  - EVM(Application layer of Ethereum)
    - 정해진 언어만 사용가능(Solidty)
- Public general-purpose blockchains 문제
  - flexiblity, speed, throughput, scalability state finality, sovereignty
  - speed : 트랜잭션 스피드
    - 비트 10분 이더 15초
    - 트랜잭션이 쌓이는 경우도 있다.
  - throughput : 디앱들마다 요구하는 것이 다름
    - (혼자 생각) 무조건 많으면 좋은게 아닌가 ? Decentralization 등이 약화될 수가 있음
  - State finality : Probablistic finality, Absolute finality
  - Sovereignty : Chain's governance vs. Application's governance
- Application-spepcific
  - 특정 use cases에만 적용
  - 그럼 어쩌라고?
  
## How does Cosmos fit into the general development of blockchain technology?
- 2016년 Jae Kwon과 Ethan Buchman : TBFT 발명
- Cosmos 오픈소수 커뮤니티 프로젝트 => Interchain Foundation 설립되어서 재정 지원
- 비젼
  - 블록체인 프로젝트의 간편한 개발환경
  - 이전 블록체인 프로젝트의 주요 이슈 해결
  - 체인 간 상호 운용성
- 블록체인의 인터넷 ?
  - interoperable network of blockchains
  - free from main chain governance
  - IBC(Inter blockchain protocol) 포함한 toolkit
  - Cosmos SDK and Tendermint

## How does Cosmos solve the scalability issue?
- Horizontal scalability: Scale out
- Vertical scalability: Scale up
  - 합의 메커니즘
  - 어플리케이션 최적화
- Cosmos => 여러 체인에서 돌아가게 할 수 있다 using IBC

## How does Cosmos promote sovereignty?
- 체인 업그레이드는 어플리케이션의 거버넌스를 해칠 수 있다.(two-layer governance)
- 사용자가 직접 application 맞춤 블록체인 쉽게 만들 수 있다.(a one-layer governance)

## How does Cosmos improve user experience?
- User == Developer
- Cosmos SDK !
