# 1. BaseApp

BaseApp 이란, Cosmos SDK App 의 bolierplate implementation

```
+---------------------+
|                     |
|    App (BaseApp)    |
|                     |
+--------+---+--------+
         ^   |
         |   | ABCI
         |   v
+--------+---+--------+
|                     |
|                     |
|     Tendermint      |
|                     |
|                     |
+---------------------+
```

Application 은 Tendermint 에서 넘어온 트랜잭션을 해석하는 역할을 수행하므로 (consensus와 network는 Tendermint 역할), 이를 위한 ABCI 와 State Machine 을 구현해야함

---

## Defining an application
BaseApp type 을 상속받아서 정의

```go
type App struct {
  // reference to a BaseApp
  *baseapp.BaseApp

  // list of application store keys

  // list of application keepers

  // module manager
}
```
- BaseApp’s parameters: https://github.com/cosmos/cosmos-sdk/blob/v0.40.0-rc3/baseapp/baseapp.go#L46-L131
- 중요한 Parameter 들
  - **`CommitMultiStore`**
    - 마지막 block 의 상태를 들고있는 main store
  - Database
    - **`CommitMultiStore`** 가 사용하는 DB
  - Msg service router
    - 트랜잭션내의 Msg 의 종류에 따라 맞는 Module 로 매칭시켜 보내주는 router
  - gRPC Query router
    - Query 종류에 따라 맞는 Module 로 매칭시켜 보내주는 router
  - TxDecoder
    - bytes 형태의 raw tx 데이터를 decoding
  - ParamStore
    - consensus parameter 들을 저장하는 store
        - cosensus param: voteInfos, minGasPrices, appVersion, etc.
  - AnteHandler
    - sign verification, fee payment 등을 Handling
  - InitChainer, BeginBlocker, EndBlocker
    - 각각 InitChain / BeginBlock /  EndBlock ABCI msg 받았을 때 실행되는 함수들

---

## States 3가지
- main state (persistent, canonical state of the app)
- checkState
- deliverState

---

## ABCI Msg에 따른 State 변화
- `InitChain` state updates
    - main state (CommitMultiStore) 에서 branching 되어 checkState, deliverState 초기화
    - ![initChain](https://github.com/cosmos/cosmos-sdk/blob/master/docs/core/baseapp_state-initchain.png)
- `CheckTx` state updates
    - AnteHandler 를 통해 트랜잭션내의 각 메시지가 해당하는 서비스 router 가 존재하는지 확인하고, 성공시에만 checkState update
    - ![CheckTx](https://github.com/cosmos/cosmos-sdk/blob/master/docs/core/baseapp_state-checktx.png)
- `BeginBlock`state updates
    - deiverState branch 생성
    - ![BeginBlock](https://github.com/cosmos/cosmos-sdk/blob/master/docs/core/baseapp_state-begin_block.png)
- `DeliverTx` state updates
    - beginBlock → DeliverTx 이후에 message execution result가 성공하는 경우에만 deliverState udpate
    - ![DeliverTx](https://github.com/cosmos/cosmos-sdk/blob/master/docs/core/baseapp_state-deliver_tx.png)
- `Commit` state updates
    - 최종적으로 deliverState 의 상태를 main state (CommitMultiStore) 에 update함
    - 이때 새로 commit된 state 를 checkState 로 update
    - deliverState 는 nil 로 초기화 (BeginBlock 에서 branch)
    - ![Commit](https://github.com/cosmos/cosmos-sdk/blob/master/docs/core/baseapp_state-commit.png)
    

