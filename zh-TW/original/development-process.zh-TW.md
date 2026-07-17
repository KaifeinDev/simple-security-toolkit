# 開發流程
擁有安全程式碼庫最關鍵的因素之一，就是紮實的開發流程：「預防勝於治療」。本文件提供了 [Nascent](https://www.nascent.xyz) 團隊認為行之有效的*範例*開發流程。除了能整體提升程式碼品質之外，本流程也是我們[審計就緒檢查清單](audit-readiness-checklist.zh-TW.md)的搭配文件。如果您依循以下流程進行開發，理應能自然而然地完成該檢查清單中的大部分項目。

```
功能需求 (Feature Request)
 |
 └> 規格制定 (Specification)
     |
     └> 評估 (Evaluation)（時間 + 複雜度 + 風險）
        |
        └> 實作 (Implementation)
           |
           └> 測試 (Testing)
              |
              └> 部署 (Deployment)
                 |
                 └> 監控 (Monitoring)
```
## 規格制定
記錄下會影響此功能的各類變數：
1. 使用者輸入？
2. 時間？
3. 其他協定？
4. 現有狀態？

## 評估
特別撥出時間進行評估：
1. 實際上這需要花多久時間？沒有規格的話，大多數估算都會不準確
2. 這份規格是否過於複雜？複雜度會導致 bug 並使整體程式碼品質變差
3. 此功能相關的風險有哪些？請花充分的時間評估此項。考量該功能可能觸及的每一個模組。接著再回頭檢視被排除的模組，*確認*它們確實不會受到影響

您可能已經（或者上線後將會）有數百萬美元的資產暴露在風險之中。因此，您**必須**考量每個功能可能如何將*全部*資產置於風險之中。一步走錯，代價很可能就是數百萬美元。
## 實作
1. 起草一份 PR
  - 內容包含功能需求與規格說明
2. 撰寫初版實作
  - 先完成一版初步實作
  - 對於任何作為使用者進入點的函式，遵循[函式需求-效果-互動-協定不變量 (FREI-PI) 模式](https://www.nascent.xyz/idea/youre-writing-require-statements-wrong)
  - 記錄所有函式預期的行為（public/external 函式使用 [NatSpec](https://docs.soliditylang.org/en/develop/natspec-format.html)），並加入行內文件/註解
  - 任何 `unchecked` 的使用都應附上如下的「安全性 (safety)」文件說明：
    ```solidity
    // Safety:
    //  1. a + b: a and b are uint64s, and a is casted up to uint128, so a uint128(uint64) + uint64
    //     cannot overflow
    //  2. c * 2: c is casted up to uint256, so a uint256(uint128) * 2 cannot overflow
    uint256 d;
    unchecked {
      uint128 c = uint128(a) + b;
      d = uint256(c) * 2;
    }
    ```
  - **每一行**組合語言 (assembly) 都要有文件說明/註解

## 測試

3. 撰寫初步的具體[測試](https://book.getfoundry.sh/forge/tests.html)
  - 為實作撰寫初版測試。每一次的儲存寫入 (storage write) 都應該被檢查，每一個 revert 都應該被檢查。逐行檢視實作程式碼，找出所有的儲存寫入
  - 對於不會改變狀態的函式，好的做法是設立一個繼承該合約的測試合約，用來執行相關運算
  - 使用覆蓋率工具（例如 Foundry 的[覆蓋率工具](https://github.com/foundry-rs/foundry/pull/1576)）來檢視您的測試對程式碼的涵蓋程度
4. 清理實作程式碼
5. 改善測試，接著根據發現的任何 bug 回到步驟 4
6. 執行 [slither](https://github.com/crytic/slither)
  - 分析輸出結果，若有需要則回到步驟 4
7. 撰寫[模糊測試 (fuzz tests)](https://book.getfoundry.sh/forge/fuzz-testing.html)
  - 既然您現在對自己的實作相當有信心，就丟隻猴子進去試試看吧。好的模糊測試應該考量所有合法的輸入，並盡可能納入多種狀態轉換的斷言（例如：這個函式是否應該單調遞增/遞減、是否應該始終小於某個值等等）
8. 若發現任何 bug，回到步驟 4
9. 撰寫整合測試
  - 您的功能現在很可能確實如您所預期地運作。但在複雜系統中，這樣還不夠。理想情況下，您也應該測試過它對整個系統造成的影響。使用 [Foundry](https://github.com/foundry-rs/foundry) 和 [Echidna](https://github.com/crytic/echidna) 進行有狀態測試，可以讓您測試系統層級的不變量
10. 若發現任何 bug，回到步驟 4
11. 整理文件
12. 設置 CI
  - 透過 Github Actions 進行持續整合 (Continuous Integration)，能在基本事項上協助審查者
  - [Foundry CI](https://github.com/foundry-rs/foundry-toolchain) 與 [Slither CI](https://github.com/foundry-rs/forge-template/blob/36f0bf7cbc953f071027a1c1783e7e5c7d9613ed/.github/workflows/lint.yml) 都很有幫助。如果您使用 [forge-template](https://github.com/foundry-rs/forge-template)，這兩者可以直接開箱即用
13. PR 審查
  - 實作者只是第一道防線。如果您是審查者，請確認實作者是否遵循了上述原則（每個狀態轉換都有測試、每個 revert 都有測試、模糊測試、以及整合測試）
  - 審查文件，確保實作內容與文件所描述的行為相符。若不相符，請與實作者確認應該更新哪一方
  - 確保 CI 通過
  - 檢查常見的疏失（[重入攻擊 (reentrancy)、檢查-效果-互動模式 (checks-effects-interactions pattern) 等](https://docs.soliditylang.org/en/latest/security-considerations.html#pitfalls)）


## 部署
14. 撰寫部署腳本
  - Foundry 提供了[腳本撰寫指南](https://book.getfoundry.sh/tutorials/solidity-scripting)，可用來在本地分支 (local forks) 上測試您的部署流程
15. 撰寫部署測試
  - 撰寫測試*每一個狀態轉換*的測試，確保部署完全按照計畫進行，且不會發生任何非預期的變化。其中一種做法是使用 `record` cheatcode。若是要對現有協定進行升級，可以先列出整個協定的所有地址清單，呼叫 record，接著執行升級。然後，對協定中的每個地址呼叫 `accesses`，確認沒有任何 slot 或地址發生非預期的變動
16. 進行審計
  - 考量此功能/合約是否需要審計。永遠寧可謹慎，也不要事後後悔
  - 讓複雜度與程式碼規模來決定是否需要審計、以及該由誰來審計您的合約
  - 在 Nascent，我們內部有一份審計人員品質的分級清單。建議您向其他開發者請教，了解哪些審計人員值得信賴、哪些則否。有些審計人員只是為了走個過場，也有些是真心在乎能否找出漏洞
17. 落實審計修正
18. 建立監控服務
  - 建立內部工具，監控系統中的重要面向
  - 可使用像是 [Check the Chain](https://github.com/checkthechain/checkthechain) 搭配 [Grafana](https://grafana.com/) 之類的工具，或是使用現成的監控工具，例如 [Tenderly](https://tenderly.co/alerting) 或 OpenZeppelin 的 [Defender Sentinels](https://www.openzeppelin.com/defender)
19. 準備/更新您的[事件應變計畫](incident-response-plan-template.zh-TW.md)
20. 部署合約
  - 恭喜，您在安全開發與部署方面，大概已經超越了 99% 的 Solidity 開發者

## 監控
21. 密切監控接下來幾個小時
  - 使用您所建立的監控服務，仔細觀察是否有非預期的行為發生，並隨時準備採取行動
22. 放鬆一下，喝杯啤酒吧，這是您應得的。
