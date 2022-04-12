# Migration
- blockchain에서 upgrade는 어려움. same height 에 동시에 upgrade되어야함. 아니면 fork 발생
- ETH같은 EVM 의 Smart Contract는 immutable, but COSMOS SDK는 가능

### migration progress
1. Plan
  - 특정 block height에 실행되는 upgrade process로, sidecar를 포함하고 있음
2. Sidecar 
  - node가 돌릴 수 있는 binary로, 업그레이드를 위한 file download 와 compile 과정 등이 담겨있음
3. UpgradeHandler 
  - sidecar process 이후 실행되는 upgrade handler
4. StoreLoader 
  - new binary를 위한 state를 loading
5. Proposal 
  - 준비된 plan 에 대한 governance proposal. 만약 투표에서 reject 시, plan은 실행되지 않음
  
  
 ### Advantages of this kind of migration
- **Avoidance of forks:** all validators move together at a pre-determined block height.
- **Smooth upgrade of binaries:** the new software is adopted in an automated fashion.
- **Reorganizing data stores:** data at rest can be reorganized as needed by processes that are not limited by factors such as a block gas limit.

### cosovisor
- node operator 들이 사용할 수 있는 migration 자동화 온체인 프로세스 app binaries의 wrapper.
- approved upgrade proposal 를 자동으로 detect 하여 new binary 로 upgrade하는데 도움을 주는 tool.

