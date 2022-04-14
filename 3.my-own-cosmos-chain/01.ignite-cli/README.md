# Ignite CLI

## Overview

- 코스모스 SDK는 블록체인을 만들기 위한 빌딩블록(module)을 제공한다.
- 이 때 여러 빌딩블록을 하나로 엮어주기 위해 사용되는게 BaseApp.
- Ignite CLI는 scaffolding module과 BaseApp을 엮어주는 CLI 툴이다.
- 레일즈를 써봤으면 알 수 있을거라고 함.

## Install

아래처럼 설치하자. 그 뒤 `ignite` 를 쳐서 제대로 나오면 성공!

```
curl https://get.ignite.com/cli! | bash
```

## Your Chain - Checker

![checker](https://media.kohlsimg.com/is/image/kohls/3885853?wid=1200&hei=1200&op_sharpen=1)

`checkers` 라는 이름의 가장 기본적인 체인을 만들어보자!

최종 결과물은 checker 게임을 만드는 것!

### Scaffold 생성

```
ignite scaffold chain github.com/alice/checkers
```

그러면 아래와 같은 폴더 구조가 등장

- app: a folder for the application.
- cmd: a folder for the command-line interface commands.
- proto: a folder for the Protobuf objects definitions.
- vue: a folder for the UI.
- x: a folder for all your own modules, in particular checkers.

그럼 체인을 시작해보자!

```
cd checkers
ignite chain serve
```

그럼 아래와 같은 일들이 벌어진다.

1. dependencies 설치
2. Protobuf 파일 빌드하기
3. 컴파일
4. 하나의 validator로 노드 실행하기
5. 계정 추가

이 때 5. 의 계정 추가는 `config.yml` 에서 관리할 수 있다.

### 결과

결과가 CLI에 뜬다!

#### 어떤 계정들을 추가했는지와 그들의 mnemonic code

```
🙂 Created account "alice" with address "cosmos19yljp0z2xw7nfrhc3vy5qrg97ja95ygev8wdjl" with mnemonic: "recall upset multiply search tool sausage empty pattern attract fine flag size labor air shuffle piece pupil glow rival crop mean suspect blue churn"
🙂 Created account "bob" with address "cosmos10dcncmlm3qpg760ycst48gd2vdvv0p8z26wvzd" with mnemonic: "zone menu join movie afford unique they unique limit pumpkin sorry vanish choose donkey dizzy spider message speak air feed person siege claim aspect"
```

#### 내가 만든 블록체인의 endpoint 1

🌍 Tendermint node: http://0.0.0.0:26657
: 아직 잘 모르겠음

#### 내가 만든 블록체인의 endpoint 2

🌍 Blockchain API: http://0.0.0.0:1317
: swagger 가 뜬다! execute도 가능함.

##### 예시

- /cosmos/auth/v1beta1/accounts : 어떤 account 가 있는지 리턴함. 위의 두 친구를 리턴
- /cosmos/bank/v1beta1/balances/{address} : account 가 가진 모든 token의 balance를 리턴

```
{
  "balances": [
    {
      "denom": "stake",
      "amount": "100000000"
    },
    {
      "denom": "token",
      "amount": "20000"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "2"
  }
}
```

이렇게 나옴

#### 내가 만든 블록체인의 endpoint 3

🌍 Token faucet: http://0.0.0.0:4500
: post 로 보내면 faucet이 된다.

##### 예시

```
{
  "address": "cosmos19yljp0z2xw7nfrhc3vy5qrg97ja95ygev8wdjl",
  "coins": [
    "10token"
  ]
}
```

이렇게 보내면 실제로 토큰이 잘 들어간다. 위에 alice 한테 보내고 swagger로 확인하면 진짜 작동함.

## Your GUI

GUI도 제공한다. 아래와 같이 설치하자

```
cd vue
npm install
npm run serve
=> 이게 작동을 안해서 README 읽어보니까
npm run dev
로 하라고 해서 했더니 됐다.
```

### GUI 접속 결과

localhost:3000 으로 가보자!(문서에는 8080이라고 나와있지만, 3000으로 나와서 3000으로 접속)

- Connect Wallet 을 누르니 다짜고짜 keplr 지갑을 연결하란다...
- 그러면 내가 만든 체인을 추가하겠냐는 알람이 뜨고 하겠다고 하면 추가해준다.
- 이후에 가지고 있는 자산을 보내고 받는 기능을 웹앱으로 진행할 수 있으나, 지금 쓰고 있는 지갑에 alice를 추가할 수 없어서 그냥 여기까지만 진행함.

## Your First Message

새로운 모듈을 추가해보자! `Message` 라는 모듈을 추가해봅시다.

```
ignite scaffold message createPost title body
```

- message : 모듈 이름이 message 이다.
- createPost : message 의 name
- title, body : message 의 field. type은 default 가 string

그러면 아래처럼 모듈과 관련한 새로운 파일들이 생기거나 수정된다.

```

**modify proto/checkers/tx.proto**
modify x/checkers/client/cli/tx.go
create x/checkers/client/cli/tx_create_post.go
modify x/checkers/handler.go
create x/checkers/keeper/msg_server_create_post.go
modify x/checkers/module_simulation.go
create x/checkers/simulation/create_post.go
modify x/checkers/types/codec.go
create x/checkers/types/message_create_post.go
create x/checkers/types/message_create_post_test.go

```

그러면 proto/checkers/tx.proto 에 다음과 같이 새로운 메세지 타입이 생긴다!

```
// this line is used by starport scaffolding # proto/tx/message
message MsgCreatePost {
  string creator = 1;
  string title = 2;
  string body = 3;
}
```

여기에 더불어 message를 만들 수 있는 CLI 명령어도 추가되며

```
func CmdCreatePost() *cobra.Command {
  cmd := &cobra.Command{
    Use:   "create-post [title] [body]",
    Short: "Broadcast message createPost",
    Args:  cobra.ExactArgs(2),
    ...
  }
}
```

Vue element에도 추가된다.

```
async MsgCreatePost({ rootGetters }, { value }) {
    try {
        const txClient=await initTxClient(rootGetters)
        const msg = await txClient.msgCreatePost(value)
        return msg
        ...

```
