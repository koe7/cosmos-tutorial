# Message Handler - Create and Save a Game Properly

여태까지 한 일

- game을 만드는 message를 만든 것
- 위 메세지를 (de)serialization 할 수 있는 메소드를 만들었음.

앞으로 해야 하는 일

- 새로운 game을 만들자!
- 게임을 만들고 storage에 저장하자!
- 새로운 게임의 ID 를 리턴하자.

## CreateGame 만들기

```Go
// x/checkers/keeper/msg_server_create_game.go

func (k msgServer) CreateGame(goCtx context.Context, msg *types.MsgCreateGame) (*types.MsgCreateGameResponse, error) {
	ctx := sdk.UnwrapSDKContext(goCtx)

	// TODO: Handling the message
	_ = ctx

	return &types.MsgCreateGameResponse{}, nil
}
```

여기까지는 이미 만들어져 있는 코드이다!

그 뒤에 같은 파일에서 쭉쭉 진행하자.

### 1. rules import 하기

```
import (
    rules "github.com/alice/checkers/x/checkers/rules"
)
```

### 2. Game Id 가져오는 코드 입력

```Go
nextGame, found := k.Keeper.GetNextGame(ctx)
if !found {
    panic("NextGame not found")
}
newIndex := strconv.FormatUint(nextGame.IdValue, 10)
```

### 3. 게임에 대한 Object 만들기

```Go
storedGame := types.StoredGame{
    Creator: msg.Creator,
    Index:   newIndex,
    Game:    rules.New().String(),
    Red:     msg.Red,
    Black:   msg.Black,
}
```

### 4. storedGame 이 잘 입력되었는지 Validation 하는 함수 만들기

```Go
err := storedGame.Validate()
if err != nil {
    return nil, err
}

```

### 5. StoredGame 저장하는 함수

```Go
k.Keeper.SetStoredGame(ctx, storedGame)
```

이 setter는 Scaffold 에서 만들어주는 함수이다.

### 6. Next game 을 위한 준비

```Go
nextGame.IdValue++
k.Keeper.SetNextGame(ctx, nextGame)
```

### 7. Response에 id 담아서 리턴

```Go
return &types.MsgCreateGameResponse{
    IdValue: newIndex,
}, nil
```

### 완성된 코드

```Go
func (k msgServer) CreateGame(goCtx context.Context, msg *types.MsgCreateGame) (*types.MsgCreateGameResponse, error) {
	ctx := sdk.UnwrapSDKContext(goCtx)

	nextGame, found := k.Keeper.GetNextGame(ctx)
	if !found {
		panic("NextGame not found")
	}
	newIndex := strconv.FormatUint(nextGame.IdValue, 10)
	storedGame := types.StoredGame{
		Creator: msg.Creator,
		Index:   newIndex,
		Game:    rules.New().String(),
		Red:     msg.Red,
		Black:   msg.Black,
	}
	err := storedGame.Validate()
	if err != nil {
		return nil, err
	}
	k.Keeper.SetStoredGame(ctx, storedGame)

	nextGame.IdValue++
	k.Keeper.SetNextGame(ctx, nextGame)

	return &types.MsgCreateGameResponse{
		IdValue: newIndex,
	}, nil
```

## 다음에 할 일

- To add new fields to the stored information.
- To add an event.
- To consume some gas.
- To facilitate the eventual deadline enforcement.
- To add money handling including foreign tokens.
