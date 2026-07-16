# 延伸資安檢查清單與資源

本文件整理了與本工具箱性質相近、可搭配使用的其他資安檢查清單與框架，並標注各自的適用情境與對應的本工具箱文件。挑選原則：**實務可操作（可直接當 checklist 用）、由知名資安團隊維護、且目前仍在維護中**。

> 建議用法：本工具箱的四份文件作為主要流程；下列資源用於（1）補齊本工具箱未涵蓋的面向（營運安全、私鑰管理、組織成熟度），（2）在特定階段需要更細的技術檢查項目時深入查閱。

## 總覽

| 資源 | 提供者 | 型態 | 適用階段 | 對應本工具箱文件 |
| --- | --- | --- | --- | --- |
| [Building Secure Contracts](https://secure-contracts.com/) | Trail of Bits | 開發指南＋審計準備 | 開發、審計準備 | 開發流程、審計就緒 |
| [Smart Contract Best Practices](https://github.com/Consensys/smart-contract-best-practices) | Consensys Diligence | 開發最佳實務 | 開發 | 開發流程 |
| [Solcurity](https://github.com/transmissions11/solcurity) | transmissions11 | 逐項程式碼檢查清單 | 開發、內部審查 | 審計就緒 |
| [Solodit Checklist](https://solodit.cyfrin.io/checklist) | Cyfrin | 互動式審計檢查清單 | 內部審查、審計 | 審計就緒 |
| [OWASP Smart Contract Security（SCS）](https://scs.owasp.org/) | OWASP | 驗證標準＋Top 10＋檢查清單 | 全階段 | 審計就緒 |
| [SEAL Frameworks](https://frameworks.securityalliance.org/) | Security Alliance (SEAL) | 全面性安全框架 | 全階段（含營運） | 全部四份 |
| [The Rekt Test](https://blog.trailofbits.com/2023/08/14/can-you-pass-the-rekt-test/) | Trail of Bits 等 | 12 題組織安全自評 | 組織成熟度評估 | 上線前、事件應變 |
| [OpenZeppelin Audit Readiness Guide](https://learn.openzeppelin.com/security-audits/readiness-guide) | OpenZeppelin | 審計準備指南 | 審計準備 | 審計就緒 |
| [EEA EthTrust Security Levels](https://entethalliance.org/specs/ethtrust-sl/) | Enterprise Ethereum Alliance | 分級驗證標準 | 審計、合規 | 審計就緒 |
| [CCSS](https://cryptoconsortium.org/) | C4 | 加密資產營運安全標準 | 金鑰／營運安全 | 上線前 |
| [awesome-audits-checklists](https://github.com/TradMod/awesome-audits-checklists) | 社群彙整 | 資源彙整清單 | 查找更多資源 | – |

---

## 開發與內部審查階段

### Building Secure Contracts（Trail of Bits）

- 網站：<https://secure-contracts.com/>（GitHub：[crytic/building-secure-contracts](https://github.com/crytic/building-secure-contracts)）
- 內容：開發準則（code maturity、審計準備）、常見漏洞範例（Not So Smart Contracts，涵蓋 EVM 之外的 Solana、Cosmos、Cairo 等鏈）、以及 Slither／Echidna／Medusa 等自動化分析工具的實作教學。
- 特別實用：其中的 [token integration checklist](https://secure-contracts.com/development-guidelines/token_integration.html) 可在整合任何第三方代幣前逐項檢查（fee-on-transfer、rebasing 等非標準行為）。
- 定位：比本工具箱的開發流程更深入技術細節，適合當工程師的進階教材與工具導入指南。

### Smart Contract Best Practices（Consensys Diligence）

- GitHub：<https://github.com/Consensys/smart-contract-best-practices>
- 內容：Solidity 開發安全的老牌參考文件——通用開發哲學、具體攻擊模式與防範、安全工具清單。
- 定位：搭配開發流程文件作為背景知識庫；新進工程師 onboarding 的必讀材料。

### Solcurity（transmissions11）

- GitHub：<https://github.com/transmissions11/solcurity>
- 內容：非常具體的逐項程式碼檢查清單，按「變數、結構、函式、修飾子、程式碼、外部呼叫、DeFi 專屬」分類，每項是一個可直接對照程式碼檢查的問題（例如「這個外部呼叫的回傳值有檢查嗎？」）。
- 定位：**最接近可直接照表操作的程式碼層級清單**，適合在 PR 審查與送審前的內部審查使用，與本工具箱的審計就緒清單互補（本清單偏流程面、Solcurity 偏程式碼面）。

### Solodit Checklist（Cyfrin）

- 網址：<https://solodit.cyfrin.io/checklist>
- 內容：互動式的審計檢查清單（有進度追蹤），項目彙整自大量真實審計報告的漏洞模式；Solodit 平台本身也可搜尋數萬筆歷史審計發現。
- 定位：內部審查時逐類別檢查；開發特定功能（如 staking、AMM）前，先在 Solodit 搜尋同類協定的歷史漏洞。

## 審計準備與驗證標準

### OWASP Smart Contract Security（SCS）

- 網站：<https://scs.owasp.org/>（GitHub：[OWASP/owasp-scs](https://github.com/OWASP/owasp-scs)）
- 內容三大件：
  - [Smart Contract Top 10](https://scs.owasp.org/sctop10/)：年度更新的十大智能合約風險，適合作為風險意識與訓練材料；
  - [SCSVS](https://scs.owasp.org/SCSVS/)：安全驗證標準，分三個等級（L1 基本／L2 中等／L3 高保證），可依協定的風險等級選擇要達到的驗證深度；
  - [SCS Checklist](https://scs.owasp.org/checklists/)：可下載的逐項檢查清單，直接對應 SCSVS 的驗證要求。
- 定位：需要向客戶或主管機關展示「依循公開標準」時，OWASP 的中立性與知名度最有說服力；SCSVS 分級也適合作為與廠商約定驗收標準的依據。

### OpenZeppelin Audit Readiness Guide

- 網址：<https://learn.openzeppelin.com/security-audits/readiness-guide>
- 內容：OpenZeppelin（大型審計方）自己撰寫的「送審前準備指南」，涵蓋文件、測試、凍結程式碼（code freeze）等審計方期待的交付狀態。
- 定位：與本工具箱審計就緒清單相互印證——一份來自專案方視角、一份來自審計方視角。

### EEA EthTrust Security Levels

- 網址：<https://entethalliance.org/specs/ethtrust-sl/>
- 內容：Enterprise Ethereum Alliance 制定的正式驗證標準，同樣採分級制（Level 1–3），每一級有明確定義的必檢項目，偏向正式規格的寫法。
- 定位：企業級或需要正式合規背書的專案適用；一般新創協定可先用 OWASP SCSVS。

## 營運、組織與事件應變

### SEAL Frameworks（Security Alliance）

- 網站：<https://frameworks.securityalliance.org/>（GitHub：[security-alliance/frameworks](https://github.com/security-alliance/frameworks)）
- 內容：目前 Web3 界涵蓋面最完整的開源安全框架，模組化主題包括：營運安全（OpSec）、錢包與金鑰管理、事件偵測與應變（含[事件應變範本](https://frameworks.securityalliance.org/incident-management/incident-response-template/overview/)）、社群管理、DevSecOps 等。
- 另外：SEAL 營運 **SEAL 911**（緊急事件的白帽救援熱線）——建議把它加入事件應變計畫的外部聯繫清單。
- 定位：本工具箱聚焦在「合約」本身；SEAL Frameworks 補齊「合約以外」的所有面向（人員、裝置、金鑰、社群帳號安全）。**強烈建議作為本工具箱之外的第一優先延伸讀物。**

### The Rekt Test（Trail of Bits 等）

- 介紹：<https://blog.trailofbits.com/2023/08/14/can-you-pass-the-rekt-test/>
- 內容：由 Trail of Bits、Immunefi 等多家資安機構共同制定的 12 題是非題（仿軟體界經典的 Joel Test），涵蓋文件化、角色權限、事件應變計畫、身分驗證、硬體金鑰、bug bounty 等。答「是」越多，組織安全成熟度越高。
- 定位：**最適合給主管層快速評估整體安全狀態**——12 題可在一次會議內完成自評，答「否」的題目就是下一步的改善方向。

### CCSS（CryptoCurrency Security Standard）

- 網站：<https://cryptoconsortium.org/>
- 內容：針對「管理加密資產的資訊系統」的安全標準（金鑰生成、儲存、使用、備援、人員權限），分三個認證等級。
- 定位：涉及託管使用者資產、營運熱錢包／冷錢包的團隊適用，補足智能合約以外的資產保管面向。

## 彙整型資源

### awesome-audits-checklists

- GitHub：<https://github.com/TradMod/awesome-audits-checklists>
- 內容：社群維護的審計檢查清單彙整（含各協定類型專用清單），適合需要找特定主題（如跨鏈橋、借貸協定）檢查清單時查閱。

---

## 與本工具箱的搭配建議（速查）

| 你想做的事 | 用哪份 |
| --- | --- |
| 建立團隊開發 SOP | 本工具箱[開發流程](development-process.zh-TW.md) ＋ Building Secure Contracts |
| PR／內部程式碼審查 | Solcurity ＋ Solodit Checklist |
| 送審前自檢 | 本工具箱[審計就緒清單](audit-readiness-checklist.zh-TW.md) ＋ OpenZeppelin Readiness Guide |
| 與廠商約定驗收標準 | OWASP SCSVS（選定等級）＋ 本工具箱清單 |
| 上線前把關 | 本工具箱[上線前清單](pre-launch-security-checklist.zh-TW.md) ＋ SEAL Frameworks（營運安全） |
| 建立事件應變能力 | 本工具箱[事件應變範本](incident-response-plan-template.zh-TW.md) ＋ SEAL 事件應變框架／SEAL 911 |
| 向主管層報告安全成熟度 | The Rekt Test |
| 整合第三方代幣 | Trail of Bits token integration checklist |
