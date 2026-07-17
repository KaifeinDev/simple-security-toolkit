# 簡易資安工具箱 (Simple Security Toolkit)
本專案是一系列針對智能合約開發、實用導向的資安指南與檢查清單集合，由 [Nascent](https://www.nascent.xyz/) 團隊彙整，分享給我們的投資組合公司以及生態系中其他可能覺得有用的人。本專案並非旨在做到面面俱到；內容偏向實用且帶有主觀立場的建議，我們認為這些建議特別適合開發與管理協定早期版本的團隊。

### 目錄

1. **[開發流程 (Development Process)](https://github.com/nascentxyz/simple-security-toolkit/blob/main/zh-TW/development-process.zh-TW.md)**

擁有安全程式碼庫最關鍵的因素之一，就是紮實的開發流程：「預防勝於治療」。本文件提供了一個 Nascent 團隊認為行之有效的開發流程範例。內容涵蓋從最初的設計與功能需求 -> 規格制定 -> 評估 -> 實作 -> 測試 -> 部署 -> 監控等各個步驟。

2. **[審計就緒檢查清單 (Audit Readiness Checklist)](https://github.com/nascentxyz/simple-security-toolkit/blob/main/zh-TW/audit-readiness-checklist.zh-TW.md)**

審計既昂貴又耗時，且需要提前數個月排定時程。完成這份檢查清單有助於確保程式碼庫已準備好接受外部審查，並盡可能抓出容易發現的問題。這能讓審計人員將時間與精力集中在找出更深層、更關鍵的漏洞上。

3. **[上線前資安檢查清單 (Pre-Launch Security Checklist)](https://github.com/nascentxyz/simple-security-toolkit/blob/main/zh-TW/pre-launch-security-checklist.zh-TW.md)**

在將程式碼部署到主網之前，團隊應完成這份檢查清單，以確保已採取必要步驟，能夠回報並應對潛在的漏洞或資安事件。

4. **[事件應變計畫範本 (Incident Response Plan Template)](https://github.com/nascentxyz/simple-security-toolkit/blob/main/zh-TW/incident-response-plan-template.zh-TW.md)**

沒有任何專案會預期自己遭遇資安事件。事先將應變計畫記錄下來，能幫助團隊在腎上腺素飆升的緊張時刻，仍能迅速且冷靜地做出回應。

5. **[組織安全成熟度自評清單 (Security Maturity Self-Assessment)](security-maturity-self-assessment.zh-TW.md)**

改編自 The Rekt Test 的 12 題是非題自評，供主管層在一次會議內快速評估組織整體的安全成熟度，答「否」的題目即為改善事項。

6. **[營運安全與金鑰管理檢查清單 (Operational Security Checklist)](operational-security-checklist.zh-TW.md)**

改編自 SEAL Frameworks（CC BY-SA 4.0）與 CCSS，涵蓋合約以外的攻擊面：金鑰與多簽管理、人員與裝置、帳號與基礎設施、供應鏈安全。

7. **[第三方代幣整合檢查清單 (Token Integration Checklist)](token-integration-checklist.zh-TW.md)**

協定支援任何新代幣之前的逐項檢查：特權功能盤點、非標準 ERC20 行為（手續費、彈性供給、重入掛勾）與整合實作要求。

8. **[延伸資安檢查清單與資源 (Additional Security Checklists & Resources)](additional-security-checklists.zh-TW.md)**

整理其他知名團隊（Trail of Bits、OWASP、SEAL、Cyfrin 等）維護的同類型檢查清單與安全框架，標注各自的適用階段、與本工具箱文件的搭配方式，以及哪些已改編為上述本地範本。


### 貢獻方式

感謝您對本專案貢獻的興趣！我們對於本儲存庫要新增哪些內容抱持*非常*主觀的立場。不過，我們仍歡迎外部貢獻者提出建議，並鼓勵您這麼做。如果您有新文件的構想，建議先開一個 issue 再開始撰寫 PR，以了解我們的支持程度。若您對現有文件有建議，可以直接開 PR。

歡迎 fork 本儲存庫並打造屬於自己的版本。與您的團隊分享想法並持續迭代。若您發現對更廣泛的團隊可能有用的內容，歡迎考慮開一個 PR！
