# 審計就緒檢查清單

### 基本品質檢查清單

- [ ]  使用 Solidity 的[最新](https://docs.soliditylang.org/en/latest/)主要版本。
- [ ]  盡可能使用已知且成熟的函式庫。[OpenZeppelin contracts](https://github.com/OpenZeppelin/openzeppelin-contracts/) 是首選，因為它們將安全性置於一切之上，而且大多數審計人員已經對它們相當熟悉。[Solmate contracts](https://github.com/Rari-Capital/solmate) 則是在 gas 優化至關重要的函式上，一個不錯的替代方案。
- [ ]  針對「正常路徑 (happy path)」的使用情境撰寫測試，並針對預期應該失敗的操作撰寫預期會 revert 的測試。所有測試都應該通過。
- [ ]  導入模糊測試 (fuzz test)。模糊測試已被證實在挖掘漏洞上相當有效。像 [Foundry](https://github.com/foundry-rs/foundry) 和 [Echidna](https://github.com/crytic/echidna) 這類工具支援無狀態與有狀態的模糊測試。[Foundry 不變量 (Invariants) 參考指南](https://book.getfoundry.sh/forge/invariant-testing?highlight=invariant#invariant-testing) 提供了如何設定合約不變量以進行模糊測試的概觀。如果您使用的是 [Hardhat](https://github.com/NomicFoundation/hardhat)，也值得額外導入上述其中一項工具，以強化您的測試能力。
- [ ]  對您的程式碼執行靜態分析工具（首選 [Slither](https://github.com/crytic/slither)，[MythX](https://mythx.io/) 為替代方案），並仔細思考其分析結果。這些工具經常會針對非問題發出警告，但有時也能抓到一些容易發現的問題，因此仍值得執行。
- [ ]  準備部署腳本以及模擬升級腳本（若適用），並將其納入審計範圍。部署與升級的重要性不亞於執行期程式碼，需要投入同等程度的資安關注。
- [ ]  為所有函式撰寫文件。針對 `public`/`external` 函式使用 [NatSpec 文件](https://docs.soliditylang.org/en/develop/natspec-format.html)。請將這視為合約公開介面的一部分。
- [ ]  合約編譯時不應有任何編譯器錯誤或警告。
- [ ]  對程式碼執行拼字檢查。
- [ ]  盡量避免使用組合語言 (assembly)。使用組合語言會拉長審計時間，因為它捨棄了 Solidity 的防護機制，必須被更謹慎地檢查。
- [ ]  記錄 `unchecked` 的使用情況。具體說明*為何*可以安全地在該程式碼區塊不進行算術檢查，最好是針對每個運算分別說明。
- [ ]  任何可以改為 `external` 的 `public` 函式，都應該改為 `external`。這不僅是 gas 上的考量，也能降低審計人員的認知負擔，因為這減少了該函式可能被呼叫的情境數量。
- [ ]  盡可能在各處使用[函式需求-效果-互動-協定不變量 (FREI-PI) 模式](https://www.nascent.xyz/idea/youre-writing-require-statements-wrong)。將所有代幣與 ETH 轉帳都視為「互動 (interactions)」。在每次互動結束時，驗證您系統層級的協定不變量是否仍然成立。
- [ ]  找至少一位您組織之外、值得信任的 Solidity 開發者或資安人員，對您的合約進行合理性檢查 (sanity check)。如果您的程式碼問題重重、需要大幅修改，您會希望盡早（並且免費地）從一位可信賴的朋友那裡得知這件事，而不是在昂貴的審計之後才發現。


### 加分項目

- [ ]  使用形式化驗證 (formal verification) 工具來驗證不變量，但請注意——就實務而言——目前的形式化驗證工具並非萬靈丹，仍存在一些未被處理的邊緣案例。[Certora](https://www.certora.com/) 和 [Runtime Verification](https://runtimeverification.com/) 是這類常用（付費）工具的範例。
- [ ]  寫下您額外的資安假設。這不需要非常正式。例如：「我們假設 `owner` 不是惡意的、Chainlink oracle 不會謊報代幣價格、Chainlink oracle 至少每 24 小時會回報一次價格、`owner` 核准的所有代幣都是符合 ERC20 標準且沒有轉帳掛勾 (transfer hooks) 的代幣，並且鏈上永遠不會發生超過 30 個區塊的重組 (reorg)。」這能幫助您理解，*即使您的合約沒有 bug*，事情仍可能出錯的方式。優秀的審計人員能幫助您判斷這些假設是否合理，也可能指出一些您自己都沒意識到自己正在做的假設。
- [ ]  如果您對自己程式碼中的某些部分沒有把握，或是希望審計人員多花些時間關注的地方，請列出清單並與審計人員分享。
- [ ]  為審計人員補充範圍界定 (scoping) 的細節。以下可折疊區塊提供了 [Code4rena](https://code4rena.com/) 準備階段所使用的表單範例。
<details> <summary>審計範圍界定細節</summary>
  
  - 若您有公開的程式碼儲存庫，請在此分享：
  - 納入審計範圍的合約共有幾個？：
  - 這些合約的總程式碼行數 (SLoC) 是多少？：
  - 有多少個外部匯入 (external imports)？：
  - 範圍內的合約有多少個獨立的介面與結構 (struct) 定義？：
  - 您的程式碼大致上是採用組合 (composition) 還是繼承 (inheritance)？：
  - 有多少次外部呼叫？：
  - 您的測試所提供的整體行覆蓋率 (line coverage) 百分比是多少？：
  - 審計此協定的這部分時，是否需要理解程式碼庫的其他部分或取得額外背景資訊？：
  - 若是，請描述所需的背景資訊：
  - 是否使用 oracle？：
  - 該代幣是否符合 ERC20 標準？：
  - 您是否預期 ERC721、ERC777、轉帳收費 (FEE-ON-TRANSFER)、彈性供給 (REBASING) 或其他任何非標準 ERC 代幣會與此智能合約互動？：
  - 是否有任何新穎或獨特的曲線邏輯 (curve logic) 或數學模型？：
  - 是否使用時間鎖 (timelock) 函式？：
  - 這是否為 NFT？：
  - 是否具有 AMM？：
  - 這是否是某個熱門專案的分支 (fork)？：
  - 是否使用 rollup？：
  - 是否為多鏈 (multi-chain)？：
  - 是否使用側鏈 (side-chain)？：
  - 請描述任何您希望特別關注的區域。例如：「請嘗試破解 XYZ。」：
</details>
