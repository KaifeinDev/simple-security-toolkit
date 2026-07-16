# 上線前資安檢查清單

- [ ]  在您的 GitHub `README.md` 檔案中，醒目地展示資安聯絡信箱。確保您團隊中確實有人會收到這些郵件。
- [ ]  確保您的 GitHub 儲存庫列出了合約部署的地址。
- [ ]  確保您的前端介面 (UI) 有連結到您的 GitHub 儲存庫。
- [ ]  確保已部署的程式碼包含資安審計報告中建議的修補內容。即使審計已完成，程式碼仍有可能在套用這些修補之前就先被部署上線。
- [ ]  回應審計初稿報告中所有建議的變更，並讓審計人員發布包含您回應確認內容的最終版報告。
- [ ]  如果您在審計過程中收到大量的程式碼修改建議，強烈建議在完成修改後再進行第二次審計，最好是找另一位審計人員進行。問題清單冗長的審計，往往代表審計人員若有更多時間，可能會發現更多問題。
- [ ]  在 Etherscan 上[驗證](https://etherscan.io/verifyContract)您的合約。[Foundry](https://book.getfoundry.sh/forge/deploying.html?highlight=verify#verifying) 或 [Hardhat](https://hardhat.org/plugins/nomiclabs-hardhat-etherscan.html) 都有自動化工具可以協助您完成驗證。
- [ ]  建立漏洞懸賞計畫 (bug bounty program)。[Immunefi](https://immunefi.com/) 或 [HackerOne](https://www.hackerone.com/) 可以協助協調此計畫。無論您一開始認為「高嚴重程度 (High Severity)」問題的合理賞金是多少，都應該再提高 2 到 10 倍（上線後，底線應設為風險資產價值的 1%）。
- [ ]  建立監控與警示機制。您會希望隨時掌握專案的最新狀況，以便在資安事件發生時能迅速應對。舉例來說，您可以設置腳本監控新的治理提案，並在提案出現時發出警示。或者，如果您使用 TWAP，可以設置腳本，每個區塊檢查一次 TWAP，並將其與中心化交易所 (CEX) 的價格來源比對，一旦差異超過 10% 就發出警示。又或者，當合約中超過 20% 的代幣在單一交易中被移出時發出警示。您可以使用像是 [Check the Chain](https://github.com/checkthechain/checkthechain) 搭配 [Grafana](https://grafana.com/) 之類的工具，或使用現成的監控工具，例如 [Tenderly](https://tenderly.co/alerting) 或 OpenZeppelin 的 [Defender Sentinels](https://www.openzeppelin.com/defender)。
- [ ]  準備緊急應變腳本，以便在發生漏洞攻擊時暫停合約或採取其他防禦性行動。
- [ ]  建立事件應變計畫。萬一遭遇駭客攻擊，您會希望事先就知道誰會在作戰室裡、會使用哪個/哪些平台（例如 Discord、Signal 等）以及哪些頻道進行溝通，以及如何啟動防禦性行動。將這一切都事先記錄在文件中會很有幫助，這樣當腎上腺素影響您的判斷力時，您仍能有所依循。您可以使用[這份範本](https://github.com/nascentxyz/simple-security-toolkit/blob/main/zh-TW/incident-response-plan-template.zh-TW.md)來準備。
