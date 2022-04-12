# Events

### Structure
```go
message Event {
  string                  type       = 1;
  repeated EventAttribute attributes = 2 [
    (gogoproto.nullable) = false,
    (gogoproto.jsontag)  = "attributes,omitempty"
  ];
}
```

events 는, block에 추가적인 정보를 넣어줌으로써 block explorer 서비스나 wallet 서비스에서 해당 정보를 사용가능

### event form
- {eventType}.{attributeKey}={attributeValue}
- ex) message.action=”initiate”
- ex) bank module에서, Transfer type 의 event 는 `Recipient` 랑 `Sender` 를 attributes 로 가지고 있음

### Subscribing to events

- 특정 event category 만 tendermint 의 웹소켓으로 구독할 수 있음
- [Tendermint 웹소켓](https://docs.tendermint.com/master/tendermint-core/subscription.html#subscribing-to-events-via-websocket)

### example of event categories
- **`NewBlock`**
    - BeginBlock 과 EndBlock에 대한 events
- **`Tx`**
    - DeliverTx에 대한 events
- **`ValidatorSetUpdates`**
    - validatorset update 에 대한 events

