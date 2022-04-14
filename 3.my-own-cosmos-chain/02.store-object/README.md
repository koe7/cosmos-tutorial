# Store Object - Make a Checkers Blockchain

체커를 만들어봅시다!

우선 게임의 룰이 필요하다. 룰을 처음부터 구현할 필요는 없고, 이미 만들어져 있는 룰을 다운로드해서 사용하자.

```
$ cd checkers
$ mkdir x/checkers/rules
$ curl https://raw.githubusercontent.com/batkinson/checkers-go/a09daeb1548dd4cc0145d87c8da3ed2ea33a62e3/checkers/checkers.go | sed 's/package checkers/package rules/' > x/checkers/rules/checkers.go
```

## 게임을 하기 위해 필요한 storage

```
{
    플레이어1 : red player, string type의 serialized address
    플레이어2 : black player, string type의 serialized address
    Game Proper : rules 에 의해 serialized 된 game. string type
    다음 플레이어 : string type
}
```

## How to Store

- 체커 게임은 여러 개가 블록체인에서 진행이 될 것.
- 따라서 위 object는 ID 로 구분을 할 수 있다.
- Code 가 single thread로 발생하기 때문에 Auto Increment가 적절하지 않을까!

```
ignite scaffold single nextGame idValue:uint --module checkers --no-message
```

- message가 있다는 것은 player에 의해 해당 storage가 변경될 수 있다는 것을 의미.
- 따라서 이 상황에서는 그걸 방지하기 위해 간단한 message를 통해 변경할 수 없도록 한다.

```
# ignite scaffold single : CRUD for data stored in a single location

ignite scaffold single NAME [field]... [flags]

--no-message : Disable CRUD interaction messages scaffolding
```

reference : https://docs.ignite.com/cli/#ignite-scaffold-single

- map 이 필요하다! 게임을 id와 맵핑하기 위해서.

```
ignite scaffold map storedGame game turn red black --module checkers --no-message
```

```
# ignite scaffold map : CRUD for data stored as key-value pairs

ignite scaffold map NAME [field]... [flags]
```

### Result - Protobuf Object

위 두 명령어를 통해 결과적으로 아래와 같은 두 가지 message object가 생성된다.

```Go
// proto/checkers/next_game.proto

message NextGame {
    string creator = 1;
    uint64 idValue = 2;
}
```

```Go
// proto/checkers/stored_game.proto

message StoredGame {
    string creator = 1;
    string index = 2;
    string game = 3;
    string turn = 4;
    string red = 5;
    string black = 6;
}
```

그리고 각각이 컴파일 된 파일도 생성된다.

```Go
// x/checkers/types/next_game.pb.go
type NextGame struct {
    Creator string `protobuf:"bytes,1,opt,name=creator,proto3" json:"creator,omitempty"`
    IdValue uint64 `protobuf:"varint,2,opt,name=idValue,proto3" json:"idValue,omitempty"`
}
# x/checkers/types/stored_game.pb.go
type StoredGame struct {
    Creator string `protobuf:"bytes,1,opt,name=creator,proto3" json:"creator,omitempty"`
    Index   string `protobuf:"bytes,2,opt,name=index,proto3" json:"index,omitempty"`
    Game    string `protobuf:"bytes,3,opt,name=game,proto3" json:"game,omitempty"`
    Turn    string `protobuf:"bytes,4,opt,name=turn,proto3" json:"turn,omitempty"`
    Red     string `protobuf:"bytes,5,opt,name=red,proto3" json:"red,omitempty"`
    Black   string `protobuf:"bytes,6,opt,name=black,proto3" json:"black,omitempty"`
}
```

### Result - query boilerplate

나중에 query를 만들 때 사용할 수 있도록 boilor plate도 만들어준다.

```Go
// proto/checkers/query.proto

message QueryGetNextGameRequest {}

message QueryGetNextGameResponse {
     NextGame NextGame = 1;
}
```

```Go
// proto/checkers/query.proto

message QueryGetStoredGameRequest {
    string index = 1;
}

message QueryGetStoredGameResponse {
    StoredGame StoredGame = 1;
}

message QueryAllStoredGameRequest {
    cosmos.base.query.v1beta1.PageRequest pagination = 1;
}

message QueryAllStoredGameResponse {
    repeated StoredGame StoredGame = 1;
    cosmos.base.query.v1beta1.PageResponse pagination = 2;
}
```

## How Ignite CLI Works

Ignite CLI가 만든 것은 결국 다양한 Protobuf message이다.

- query.proto : state를 읽는데 사용되는 object
- tx.proto : state를 변경하는데 사용되는 object. 다만 --no-message가 있으므로, 여기서는 아무것도 없다.
- genesis.proto : 제네시스를 위한 proto.
- next_game.proto, stored_game.proto :

또한 Query 를 위한 코드도 만들어줌! 위에 있는 query 들
