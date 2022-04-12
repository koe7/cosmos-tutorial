# Context

- app 안에서 function to function 간의 정보 이동
- information about the current state of the application, the block, and the transaction

### context properties
- Context (from golang)
- MutiStore
- ABCI Header (state of the blockchain such as block height and proposer of the current block)
- Chain ID
- Transaction bytes
- Logger
- VoteInfo
- GasMeters
- CheckTx Mode
- MinGasPrice
- Consensus params
- Event Manager


### pattern of usage
- parent process로 부터 ctx 전달받음
- `ctx.ms` 라는 multistore 의 branch store 사용
- execution 중에 ctx read/write
- if success → cachemultistore 를 `ctx.ms` 에 write
- if fail → ctx is discarded

### process
- tx안의 msg의 runMsgs 실행전에 `app.cacheTxContext()` 사용 (branching & cache the context and multistore)
- checkTxMode → no need to write
- deliverTxMode → branched multistore is written


