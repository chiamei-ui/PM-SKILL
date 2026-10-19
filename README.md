# PM-SKILL

給本職之外還兼任 PM 的人使用的 Agent Skill。讓 AI 讀懂專案紀錄，找出卡點、釐清誰該決定、整理交接，再協助完成已授權的行動。

初期以 **ECOCO 員工的實際專案作為 MVP 測試場景**。目前為初版，尚未證明能減少溝通時間或提升交付成效。

## 要解決的痛點

| 常見情境 | Skill 要讓 Agent 做的事 |
| --- | --- |
| 本職與 PM 同時運作，所有不清楚的事都落到自己身上 | 區分專業品質、PM 協調、工程評估與主管拍板，指出目前缺誰的輸入 |
| 求助時先講「我要一個 PM／一個平台」，對方卻不懂為什麼 | 先寫清卡住什麼、影響什麼、需要什麼具體協助，再評估解法 |
| 工程回覆技術困難，自己又得重新想完整方案 | 整理要工程提供的限制證據、可行選項、時間與風險，讓 PM 有依據取捨 |
| 合理的跨部門協作變成拜託人情，交付反覆重講 | 產出有輸出、期限、驗收與備案的交接草稿，沿用團隊約定 |
| 小卡點一路擴成組織或系統改革 | 先解鎖眼前工作，把後續議題分開，控制本次範圍 |
| KPI 看起來漂亮，卻不知道怎麼量測；日期排好了卻沒人確認 | 查基準值、定義與資料來源，標示假設與待確認估時，顯示範圍取捨 |

## 使用方式

這個專案提供 Markdown 指令與參考規則，由支援 Skill 的 Agent 讀取執行。日常成果是對話分析、決策選項、求助／交接草稿與必要的既有記錄更新，無需另外操作介面。

### 安裝前：準備兩個 Skill

本專案需要 `project-kickoff-planner` 與 [`i-have-adhd`](https://github.com/iizcm/i-have-adhd-skill)。前者分析專案，後者負責讓人容易理解並採取決策。請下載本倉庫，以及 `i-have-adhd` 的完整 Skill 資料夾，分別安裝到下列位置；資料夾最上層都必須有 `SKILL.md`。若已有同名版本，先保留備份再更新。

`i-have-adhd` 是外部依賴，本倉庫未附帶其內容；安裝後請在首次使用時明確啟用。缺少或尚未能載入時，Agent 可先讀資料與分析，但會暫緩交付給人決策的內容，說明如何補齊。

### 安裝到 Codex

將儲存庫中的 `skills/project-kickoff-planner` 整個資料夾放入自己的 `~/.codex/skills/`，依使用環境重新載入技能。Windows 預設位置為 `C:\Users\<使用者名稱>\.codex\skills\`。若已安裝同名舊版，先保留備份，再更新該資料夾。

將 `i-have-adhd` 資料夾同樣放入 `~/.codex/skills/`。完成後應有：

```text
~/.codex/skills/project-kickoff-planner/SKILL.md
~/.codex/skills/i-have-adhd/SKILL.md
```

### 安裝到 Claude Code

1. 將本倉庫的 `skills/project-kickoff-planner` 整個資料夾放入 `~/.claude/skills/`。
2. 將下載的 `i-have-adhd` 資料夾同樣放入 `~/.claude/skills/`。
3. 開啟 Claude Code，在對話中先輸入 `/i-have-adhd`，再輸入 `/project-kickoff-planner` 與專案問題。

Windows 的個人安裝位置為 `C:\Users\<使用者名稱>\.claude\skills\`。若只想讓特定專案使用，兩個資料夾也可放入該專案的 `.claude/skills/`。這裡說明的是 Claude Code；Claude 網頁版或 Cowork 的安裝入口不同。路徑與呼叫方式依 [Claude Code 官方 Skill 文件](https://code.claude.com/docs/en/skills)。

完成後應有：

```text
~/.claude/skills/project-kickoff-planner/SKILL.md
~/.claude/skills/i-have-adhd/SKILL.md
```

技能沿用 `project-kickoff-planner` 名稱，延伸啟動、變更與卡點處理能力；不預設產出 HTML。其他 Agent 可直接讀取 [SKILL.md](skills/project-kickoff-planner/SKILL.md)，並載入 `i-have-adhd`；是否自動載入取決於該工具的機制。

### 直接這樣問

```text
使用 $project-kickoff-planner。
同時使用 $i-have-adhd，讓輸出適合人閱讀與決策。
我本職是行銷，兼任這個專案的 PM。
請讀這份會議紀錄與目前任務，分析真正卡住的點、我該決定與該請誰提供的輸入。
先給一個現在能做的行動，再幫我寫交接草稿。
```

上方為 Codex 範例。Claude Code 請先輸入 `/i-have-adhd`，接著用 `/project-kickoff-planner` 加上同一段專案描述。

### 給人決策的輸出規則

Agent 必須先使用 `i-have-adhd`，再交付計畫、選項比較、主管摘要或交接草稿：先呈現下一步與目前狀態，多步工作編號，長內容分組，保留證據與選項代價，尚有待辦時以一個可立即開始的行動收尾。這是人類決策內容的必要步驟，並非僅建議文字簡短；使用者明確停用時依其指示處理。

也可以問：「工程說來不及，幫我整理該問的原因與取捨」、「這個新增需求要怎麼處理」、「把這份復盤轉成下次遇到同類問題的做法」。提供現有文件、畫面、決策或任務即可；不必先填完整表單。

Agent 會依情境給出：卡點與證據、已決定／待決定／誰決定、推薦行動與代價、可使用的草稿、完成與驗證條件。資料不足會明確標示，不補造數字、人力或承諾。

## 執行界線

Skill 不取代專業 PM、工程可行性評估或主管授權。它也不會自動取得 ECOwork 或其他內部系統的權限；有連線且有授權才能更新，完成後需讀回。沒有連線時提供草稿，並清楚說明未同步。

整理與已授權的可逆工作可直接推進；發訊、指派、外部寫入與高代價決策依使用者授權處理。草稿完成不等於問題解除，計畫確認不等於正式上線。

沿用現有任務／文件作為主紀錄，不建立第二套任務系統。內部個資、憑證、原始會議紀錄與敏感商業資料不應因儲存庫為私人就直接上傳。

## ECOCO MVP 怎麼驗證

由願意參與的兼任 PM 員工，以可分享或去識別化的真實案例試用。參與者、時程與通過標準待測試負責人確認，不在本文件指定同事工作。

1. 先記錄原本卡點、花費時間與需要的協作輸出。
2. 讓 Agent 讀取 Skill 與案例，產生診斷及下一步。
3. 由使用者與相關接收者確認：問題有沒有抓對、責任有沒有清楚、交付能否直接使用。
4. 記錄實際是否解鎖工作、來回次數、使用者投入時間、錯誤或越權；和原本情境比較，不以主觀好評當唯一證據。
5. 依案例修正規則；沒有減少負擔就縮減格式或重新診斷，不先新增介面。

測試紀錄可沿用既有工具；本倉庫只保留去識別化案例。行為檢查見 [試用情境](tests/scenarios.md)，尚未執行真人 MVP。

## 內容結構

- [SKILL.md](skills/project-kickoff-planner/SKILL.md)：Agent 的核心分析與行動規則。
- [協作規則](skills/project-kickoff-planner/references/coordination.md)：啟動、交接、卡關與變更。
- [目標與時程](skills/project-kickoff-planner/references/outcomes.md)：KPI、估時與取捨。
- [設計依據](docs/design-rationale.md)：從復盤內化的做法與適用界線。

這是可迭代的 Skill 初版，GitHub 儲存庫已公開。請勿提交內部敏感資料。
