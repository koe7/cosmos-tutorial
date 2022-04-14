# Running a Node, API, and CLI

## See what you are going to do

https://youtu.be/qzUgh8mvyJE

### 영상 내용

#### 설치하기

simapp 으로 블록체인을 만들고 interact 해보자!
git clone 후, build 까지 완료!

```
mkdir cosmos
cd cosmos
git clone https://github.com/cosmos/cosmos-sdk
```

만약 처음 시작하거나, 만든 체인을 리셋하고 싶으면

```
./simd resetall
```

을 입력하면 됨

## 시작하기

```
./simd init demo
```

로 데모 블록체인을 실행하면, 관련한 protobuf 메세지가 등장한다. 다양한 기본값이 있고, chain id도 여기 있다.

영상에서 나온 메세지를 예쁘게 가공해서 살펴보면 다음과 같은 항목들이 존재한다.

- distribution : 체인의 네이티브 토큰 distribution 과 관련한 인자들
  - 블록 제안자에게 주는 보상? : base_proposer_reward, bonus_proposer_reward
- gov : 체인의 거버넌스와 관련한 인자들
  - tally_params : 정족수, 찬성 threshold 등등
  - deposit_params : max_deposit_period, min_deposit : 얼마나 예치해야 하는지, 최대 얼마나 예치해야 하는지
- mint : 체인의 네이티브 토큰의 발행과 관련한 인자들
  - inflation, block_per_year, inflation_max 등등... 이름 보면 대충 유추 가능..
- slashing : 나쁜 짓 하는 밸리데이터에게 부과하는 벌금?
  - downtime_jail_duration, slashing_fraction_double_downtime : 얼마동안 감옥을 보내고, 얼마를 슬래싱하고 등등
- chain_id : 체인 아이디!

이 설정값은

```
cat ~/.simapp/config/genesis.json
```

여기에 있다

## Prepare your account

```
./simd keys list
```

로 키 목록을 봤으나, 당연히 첫 시작이니 없음

```
./simd keys add b9lab
```

으로 계정을 더했다.

./simd keys list
이렇게 치면 더해진 계정이 나오고 니모닉도 나옴.

## Make yourself a proper validator

블록을 만드려는데, 블록을 검증할 계정이 없는 딜레마 상황!
(여담 : docs에 catch-22 situation 라는 말이 나오는데, 이게 딜레마 라는 단어인지 처음 알았다.)
따라서 밸리데이터를 만들어야 한다.

### Validator 에게 stake 토큰 주기

내가 만든 b9lab 를 genesis account로 등록하고, 여기에 이 체인의 네이티브 토큰인 stake 토큰을 100000000개 주자.

```
./simd add-genesis-account b9lab 100000000stake
```

<details><summary>참고</summary>

- stake 는 토큰의 심볼 이름이다! 영상 제작자가 임의로 정한 값으로, 바꿔도 괜찮다.

- reference : https://github.com/cosmos/sdk-tutorials/issues/168

- 단, 숫자는 임의로 바꿨을 때 에러가 날 수 있다. 여기에 있는 값이 이렇게 10^6 이라고 하드코딩이 되어 있다. 이 이상으로 해야 한다.
  https://github.com/cosmos/cosmos-sdk/blob/8c89023e9f7ce67492142c92acc9ba0d9f876c0e/types/staking.go#L26
- https://github.com/cosmos/cosmos-sdk/issues/4656

- 여기 내용에 의하면, 밸리데이터를 만들 때, 10^6 이하의 네이티브 토큰을 스테이킹한 밸리데이터를 만들면 에러가 날 수 있다고 한다. 이유는 그 이하로 넣게 되면 실제로 작동할 때 10^6이 나눠지며 다 잘리게 된다고 한다.
- 이 숫자가 등장한 이유는 ABCI requirement 때문인데, 10^6 uatom = 1atom 과 관련한 이유라고 함...
- 근데 일단 패스...
</details>

그렇게 b9lab 이라는 친구가 밸리데이터를 하기 위해, 받은 네이티브 토큰을 스테이킹하도록 해야 한다.
이 트랜잭션을 만들기 위해서는 아래와 같이 명령어를 작성하는데, 이 때 chain id가 필요하다.
이를 아까 `init demo` 에서 따온 값을 쓰자.

### validator가 self delegation 하도록 하기

```

./simd gentx b9lab 70000000stake --chain-id test-chain-{chain id}

# 설명 : Generate a genesis transaction that creates a validator with a self-delegation

```

70000000이라는 숫자인 이유는 블럭 생성을 위한 threshold 가 2/3 이기 때문이다.

### collcect gentx

그 뒤 만든 제네시스 트랜젝션을 모아야 한다.

```

./simd collect-gentxs

```

### 여기까지 한 작업

1. account 만들기
2. genesis block 에 들어갈 트랜잭션 만들기
3. 이들을 모으기

그러면 제네시스 트랜잭션이 모여 있는 파일이 생성된다.

## Create Blocks

간단하게 블록체인을 시작할 수 있다.

```

./simd start

6:23PM INF starting ABCI with Tendermint
6:23PM INF Starting multiAppConn service impl=multiAppConn module=proxy
6:23PM INF Starting localClient service connection=query impl=localClient module=abci-client
6:23PM INF Starting localClient service connection=snapshot impl=localClient module=abci-client
6:23PM INF Starting localClient service connection=mempool impl=localClient module=abci-client
6:23PM INF Starting localClient service connection=consensus impl=localClient module=abci-client

```

## Query

쿼리도 날릴 수 있음.

```

./simd query bank balances $(./simd keys show b9lab -a)

balances:

- amount: "30000000"
  denom: stake
  pagination:
  next_key: null
  total: "0"

```

## Send transaction

새로운 계정을 만들고, 해당 계정에 트랜잭션을 보내보자!

```

./simd tx bank send $(./simd keys show b9lab -a) $(./simd keys show student -a) 10stake --chain-id test-chain-rT4wZY

{"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address":"cosmos1nw793j9xvdzl2uc9ly8fas5tcfwfetercpdfqq","to_address":"cosmos1m95dh3uc2s7fkn4w6v3ueux3sya96dhdudwa24","amount":[{"denom":"stake","amount":"10"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""},"tip":null},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: D2CCFD91452F8C144BB1E7B54B9723EE3ED85925EE2C8AD843392721D072B895

```

## CLI Routing

어떻게 CLI 로 simd 와 interaction을 가능하게 하는 것일까?

답은 main.go 에 있었다.

```Go
// simapp/simd/main.go


package main

import (
  "os"

  "github.com/cosmos/cosmos-sdk/server"
  svrcmd "github.com/cosmos/cosmos-sdk/server/cmd"
  "github.com/cosmos/cosmos-sdk/simapp"
  "github.com/cosmos/cosmos-sdk/simapp/simd/cmd"
)

func main() {
  // 이친구가 CLI handler이다! 여길 들어거바묜
  rootCmd, _ := cmd.NewRootCmd()

  if err := svrcmd.Execute(rootCmd, simapp.DefaultNodeHome); err != nil {
    switch e := err.(type) {
    case server.ErrorCode:
      os.Exit(e.Code)

    default:
      os.Exit(1)
    }
  }
}

////////////////////////////////////

// simapp/simd/cmd/root.go
func NewRootCmd() (*cobra.Command, params.EncodingConfig) {
	encodingConfig := simapp.MakeTestEncodingConfig()
	initClientCtx := client.Context{}.
		WithCodec(encodingConfig.Codec).
		WithInterfaceRegistry(encodingConfig.InterfaceRegistry).
		WithTxConfig(encodingConfig.TxConfig).
		WithLegacyAmino(encodingConfig.Amino).
		WithInput(os.Stdin).
		WithAccountRetriever(types.AccountRetriever{}).
		WithHomeDir(simapp.DefaultNodeHome).
		WithViper("") // In simapp, we don't use any prefix for env variables.

	rootCmd := &cobra.Command{
		Use:   "simd",
		Short: "simulation app",
		PersistentPreRunE: func(cmd *cobra.Command, _ []string) error {
			// set the default command outputs
			cmd.SetOut(cmd.OutOrStdout())
			cmd.SetErr(cmd.ErrOrStderr())

			initClientCtx, err := client.ReadPersistentCommandFlags(initClientCtx, cmd.Flags())
			if err != nil {
				return err
			}

			initClientCtx, err = config.ReadFromClientConfig(initClientCtx)
			if err != nil {
				return err
			}

			if err := client.SetCmdClientContextHandler(initClientCtx, cmd); err != nil {
				return err
			}

			customAppTemplate, customAppConfig := initAppConfig()
			customTMConfig := initTendermintConfig()

			return server.InterceptConfigsPreRunHandler(cmd, customAppTemplate, customAppConfig, customTMConfig)
		},
	}

  // 여기에서 커맨드가 추가된다.
	initRootCmd(rootCmd, encodingConfig)

	return rootCmd, encodingConfig
}

////////////////////////////////////
// simapp/simd/cmd/root.go

func initRootCmd(rootCmd *cobra.Command, encodingConfig params.EncodingConfig) {
	cfg := sdk.GetConfig()
	cfg.Seal()

	rootCmd.AddCommand(
		genutilcli.InitCmd(simapp.ModuleBasics, simapp.DefaultNodeHome),
    // genutilcli : 모듈 이름! 여기에서 cmd를 불러온다.
		genutilcli.CollectGenTxsCmd(banktypes.GenesisBalancesIterator{}, simapp.DefaultNodeHome),
		genutilcli.MigrateGenesisCmd(),

    ...

}

////////////////////////////////////
// x/genutil/client/cli/collect.go

func CollectGenTxsCmd(genBalIterator types.GenesisBalancesIterator, defaultNodeHome string) *cobra.Command {
	cmd := &cobra.Command{
		Use:   "collect-gentxs",
		Short: "Collect genesis txs and output a genesis.json file",

    ...

	return cmd
}


```

## 기타

(발생한 이슈: 빌드할 때 이런 에러가 나길래, MakeFile 에서 ENABLE_ROCKSDB=true 로 바꿔주니 빌드 됨)

```

Makefile:72: RocksDB support is disabled; to build and test with RocksDB support, set ENABLE_ROCKSDB=true
go build -mod=readonly -tags "netgo ledger" -ldflags '-X github.com/cosmos/cosmos-sdk/version.Name=sim -X github.com/cosmos/cosmos-sdk/version.AppName=simd -X github.com/cosmos/cosmos-sdk/version.Version=0.46.0-beta2-35-ga1aefd036 -X github.com/cosmos/cosmos-sdk/version.Commit=a1aefd0361a1985ece5f77c71f9cf64c656f3bd0 -X "github.com/cosmos/cosmos-sdk/version.BuildTags=netgo,ledger" -X github.com/tendermint/tendermint/version.TMCoreSemVer=v0.35.3 -w -s' -trimpath -o /Users/ijaebog/dev/study/cosmos/cosmos-sdk/build/ ./...

# github.com/keybase/go-keychain

cgo-gcc-prolog:81:11: warning: 'SecKeychainCreate' is deprecated: first deprecated in macOS 12.0 - Custom keychain management is no longer supported [-Wdeprecated-declarations]
/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Security.framework/Headers/SecKeychain.h:301:10: note: 'SecKeychainCreate' has been explicitly marked deprecated here

```
