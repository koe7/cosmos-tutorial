# Message - Create a Message to Create a Game

## 여태까지 한 일

- storage에 여러 가지 object를 만듦.
- 그러나 object에다 직접적으로 CRUD 를 하는 것을 막음.

## 만들어야 하는 message

checker blockchain에 game을 만드는 message

- creator가 자동으로 지정되도록 한다.
- game의 ID를 직접 지정하지 않고, ID가 자동으로 생성되는(increment) 방식이어야 한다.
- game board를 지정하지 않도록 한다. 게임 룰에 의해서만 컨트롤이 되도록!
- 플레이어 두 명(red, black)이 지정되어야 한다.

따라서 이를 만들기 위해 아래 명령어를 입력하자

```
ignite scaffold message createGame red black --module checkers --response idValue
```

### 결과 - Protobuf object

앞에서 계속 했던 것 처럼, message를 만들었으니 아래와 같은 protobuf 와 컴파일 된 파일이 생성된다.
위 명령어에서 idValue 를 response로 넣었기 때문에 response object도 생성된다.

```Go
// proto/checkers/tx.proto

message MsgCreateGame {
    string creator = 1;
    string red = 2;
    string black = 3;
}

message MsgCreateGameResponse {
    string idValue = 1;
}
```

### 결과 - (de)serialization engine

```GO
// x/checkers/types/codec.go

func RegisterCodec(cdc *codec.LegacyAmino) {
    cdc.RegisterConcrete(&MsgCreateGame{}, "checkers/CreateGame", nil)
}

func RegisterInterfaces(registry cdctypes.InterfaceRegistry) {
    registry.RegisterImplementations((*sdk.Msg)(nil),
        &MsgCreateGame{},
    )
    ...
}
```

### 결과 - Message conform boilorplate code

```Go
func (msg *MsgCreateGame) GetSigners() []sdk.AccAddress {
    creator, err := sdk.AccAddressFromBech32(msg.Creator)
    if err != nil {
        panic(err)
    }
    return []sdk.AccAddress{creator}
}

```

메세지를 확인하기 위해 사용되는 보일러플레이트도 만들어준다!

### 결과 - Protobuf service Msg Interface

gRPC 인터페이스에다 추가한 message를 받고 보낼 수 있는 인터페이스를 추가한다.
이 인터페이스가 `service Msg` 이다.
여태는 --no-message 였기 때문에 없었으나, 위는 아니기 때문에 생성된 tx.proto 이다.

인터페이스이기 때문에 실제로 구현된 것은 없다.

```Go
# proto/checkers/tx.proto

service Msg {
    rpc CreateGame(MsgCreateGame) returns (MsgCreateGameResponse);
}

```

### Next up

이제 게임을 만들기 위해 사용하는 함수인 CreateGame 의 내용을 채워야 한다!

```Go
func (k msgServer) CreateGame(goCtx context.Context, msg *types.MsgCreateGame) (*types.MsgCreateGameResponse, error) {
    ctx := sdk.UnwrapSDKContext(goCtx)

    // TODO: Handling the message
    _ = ctx

    return &types.MsgCreateGameResponse{}, nil
}
```
