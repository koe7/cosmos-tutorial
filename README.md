# Cosmos Study Session
## Team Heimdallr X Elysia
> [Cosmos Academy](https://tutorials.cosmos.network/academy/0-welcome/)

### 목표
- 엘리시아 개발팀 : Cosmos SDK를 이용한 메인넷 개발이 어느정도 난이도인지, 인적자원 및 시간이 어느정도 걸릴지 추측하기 위함. 코스모스 지식이 전무해서 어느정도 지식 파악 정도로 사용?
- 헤임달 팀 : 프로덕트를 Cosmos SDK를 이용해서 만들어야함. Prediction market은 테라 서비스에 올리고, Oracle은 자체 체인으로 만들어 볼 생각. 테라는 케이스일 뿐. 예측 시장은 우선순위가 높고, 벌써 디자인 & 개발을 진행중임. 하지만 오라클이 있어야 완전한 예측시장.

### 방법?
- go 문법만 간단하게 파악하기
  - [https://mingrammer.com/gobyexample/](https://mingrammer.com/gobyexample/) 예제로 배우는 go
  - 스터디 시작전 모두가 한번씩 하고 오기
- [Cosmos Academy](https://tutorials.cosmos.network/academy/0-welcome/) 읽기
  - fast deep 구분 없이 모두 읽을 예정. 스터디원 총 7명? 나눠서 1주일마다 발췌해서 소개하는 시간 가지면 될듯? 모두가 정해진 일정에 맞춰 읽고, 그 주에 리딩하는 사람은 내용을 요약해와서 공유해야함. 스터디 시간에는 해당 내용 공유하는 정도로만 마무리?
- 모든 스터디의 결과물은 github repo에서 공유할 예정

### 혹시 grants?

이더리움의 경우 learning grants가 있어서, 공부하고 결과물 공유하면 돈을 받을 수 있는데!
코스모스도 혹시 있을까?

### 논의

1. 언제부터 시작? 3월 17일(태건 생일) 부터 매주 목요일 저녁 9시. 비대면 (대면할 사람 백광빌딩으로)
2. go는 어떻게 할래? → 시작전에 알아서 양심껏 다 보고오기. 퀴즈?
3. 첫번째 주 태건(Session Plan / What is Cosmos), 두번째 주 동욱(A Blockchain App Architecture)
4. 팀원들에게 17일까지 go를 모두 보고와야하고, 목요일 저녁 9시 7주간 진행할거라 공유하기.
5. 스터디 담당자가 설명을 해주고, 나머지 사람들은 설명을 듣고나서 다음주까지 해오기

## Study Plan 🗓️

Cosmos turorials를 모두 읽고, 내용을 정리하고 서로 모르는 것을 물어보며 내용을 공유하는 스터디입니다. 결과물로는 https://github.com/koe7/cosmos-tutorial 에 내용을 요약해서 올립니다.  “What is Cosmos”를 제외한 나머지 부분은 총 385,841byte 였으며 한명당 매주 약 64306 bytes씩 내용을 요약하고 공유합니다. 

### 1주차 태건
[@VideoAmp/team-name]( https://github.com/orgs/VideoAmp/teams/team-name/members )

What is Cosmos (4Pages)

### 2주차 동욱 (60740)
* A Blockchain App Architecture (33832) 
* Accounts (13553)
* Transactions (13355)

### 3주차 현민 (65909)
* Messages (13890)
* Modules (20038)
* Protobuf (8235)
* Multistore and Keepers (23746)

### 4주차 은호 (64001)
* BaseApp (11083)
* Queries (5127)
* Events (6386)
* Context (7084)
* Migrations: on-chain upgrades (14515)
* Inter-Blockchain Communication (14453)
* Bridges (5353)

### 5주차 재복 (63352)
* Running a Node, API, and CLI (23102)
* Starport (11780)
* Store Object - Make a Checkers Blockchain (16280)
* Message - Create a Message to Create a Game (6560)
* Message Handler - Create and Save a Game Properly (5630)

### 6주차 수진 (67212)
* Message and Handler - Add a Way to Make a Move (8160)
* Events - Emitting Game Information (4652)
* Message and Handler - Make Sure a Player Can Reject a Game (7098)
* Store FIFO - Put Your Games in Order (10545)
* Store Field - Keep an Up-To-Date Game Deadlin e (4666)
* Store Field - Record the Game Winner (4924)
* EndBlock - Auto-expiring Games (9108)
* Token - Let Players Set a Wager (15235)
* Gas - Incentivize Players (2824)

### 7주차 태건 (64627)
* Query - Help Find a Correct Move (6458)
* IBC Token - Play With Cross-Chain Tokens (4028)
* Migration - Introduce a Leaderboard After Production (30798)
* CosmJS (8342)
* CosmWasm (15001)
