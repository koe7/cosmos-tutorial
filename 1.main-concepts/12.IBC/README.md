# Inter-blockchain communications

### IBC 를 통해 할 수 있는 것들
- token transfers
- atomic swaps
- multi-chain smart contracts
- data and code sharding

### IBC 통신을위한 requirements
- bind to one or more ports
- define the packet data
- define optional acknowledgement structures and methods to encode and decode them
- implement the IBCModule interface

### supported IBC clientes are:
- solomachine light client (phone, browsers, laptops)
- tendermint light client: cosmos sdk-based chains
- [localhost](http://localhost) (loopback) client

### proofs and paths
미리 정의된 path를 통해서 데이터 전달
- IBC 통신 path에 필요한 requirements:  [https://github.com/cosmos/ibc/tree/master/spec/core/ics-024-host-requirements](https://github.com/cosmos/ibc/tree/master/spec/core/ics-024-host-requirements)
- IBC 통신 proof format requirements: https://github.com/confio/ics23

### Capabilities
- IBC의 기본 구조는 각 module 들이 서로를 신뢰할 필요 없이 port 와 channel에 대한 인증만 받으면 되도록 짜여져 있음
- 특정 모듈에 대한 port binding이나 channel 생성을 수행하면 dynamic capability 가 return 됨
- 해당 capability 가 있는 module 만 action 을 수행할 수 있음
- 인터넷 연결에 비유하면, channel은 IP address, port는 IP port, IBC Module은 인터넷 앱으로 비유 가능

### Channels between IBC ports: transferring data with packets
- IBC 채널을 통해 이동하는 IBC Packet에는 다음과 같은 정보가 포함됨
  - The source portID
  - The source channelID
  - The destination portID
  - The destination channelID
  - A sequence to optionally enforce ordering
  - TimeoutTimestamp
  - TimeoutHeight
 
- Handshake 과정
   - Chain A sends a ChanOpenInit message to signal a channel initialization attempt with chain B.
   - Chain B sends a ChanOpenTry message to try opening the channel on chain A.
   - Chain A sends a ChanOpenAck message to mark chain A's channel end status as open.
   - Chain B sends a ChanOpenConfirm message to mark chain B's channel end status as open.
   - Channel is open on both sides.

### Non-Tendermint chains and IBC
- Fast-finality consensus 알고리즘을 가진 chain의 경우, tendermint가 아니여도 IBC를 통해 연결 가능
- Fast-finality 가 아닌 PoW 체인도 peg-zone이라 불리는 proxy-chain을 통해 연결 가능
- ETH peg-zone example: https://github.com/cosmos/gravity-bridge


### More information
- https://ibcprotocol.org/relayers/
