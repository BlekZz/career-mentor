# 職涯履歷導師 Career Mentor

> 專業職涯履歷 AI 助理知識庫，提供履歷優化、工作經歷訪談萃取、職缺風險解剖與面試準備三大服務。  
> 支援 Claude Code CLI、Claude.ai Web、Gemini Gems、ChatGPT GPTs 四個平臺部署。

---

## 服務概覽

| 服務 | 說明 | 主要步驟 |
|------|------|---------|
| **A — 履歷檢視 + 優化 + 生成** | 評估現有履歷，提供具體回饋，生成 ATS 優化版本 | 7 步驟 |
| **A-1 — 中翻英支線** | 已有中文履歷，只需英文版，服務 A 步驟 2 分叉，翻譯確認後結束 | — |
| **B — 訪談萃取 + 新工作經歷** | 以 STAR 訪談挖掘真實貢獻，轉化為履歷條目；可選製作 60 秒口頭版 | 7 步驟 |
| **C — 職缺風險解剖 + 面試準備** | 分析 JD 紅旗、評估履歷匹配度、試做題合理性，生成面試問答集 | 9 步驟 |

### 跨服務串聯建議

```
完整作戰序列：B（素材萃取）→ A（生成履歷）→ C（JD 匹配 + 面試準備）
```

---

## 專案結構

```
career-mentor/
├── CLAUDE.md                    ← AI 開發工作指引（不上傳至任何平臺）
├── Draft_Plan.md                ← 版本記錄與未來開發路線圖
│
├── MainFiles/                   ← 唯一知識庫來源，所有平臺的內容均從此複製
│   ├── instructions.md          ← 角色定義、全域規則、服務路由主控
│   ├── Glossary.md              ← 全系統術語規範
│   ├── Service_A.md             ← 服務 A 步驟流程（含 A-1 中翻英支線）
│   ├── Service_B.md             ← 服務 B 步驟流程（含口頭版製作）
│   ├── Service_C.md             ← 服務 C 步驟流程（含試做題評估）
│   ├── Service_Interview.md     ← STAR 4 階段訪談方法論 + 面試題型框架
│   ├── Avoid_Risk.md            ← 台灣職場風險知識庫（Service C 完整引用 / B 限 Ch.2）
│   ├── Resume_Template.md       ← 履歷 8 區塊格式規範
│   ├── Special_Cases.md         ← 空窗期、非典型工作、應屆生、中高齡（45+）、轉職者
│   └── Few_Shot_Examples.md     ← 三個服務的完整輸出示範
│
├── DEPLOYMENT/                  ← 各平臺部署資料夾（由 SCRIPT/ 管理）
│   ├── Claude_Code/
│   │   ├── Claude_Code_DEPLOY-GUIDE.md  ← 安裝說明（永不刪除）
│   │   ├── SKILL.md                     ← 平臺專屬 skill 定義（永不刪除）
│   │   └── output/                      ← 部署產物（每次 deploy 完整重建）
│   │       ├── SKILL.md
│   │       └── references/              ← MainFiles 內容副本
│   ├── Claude_Web/
│   │   ├── Claude_Web_DEPLOY-GUIDE.md
│   │   ├── SKILL.md
│   │   ├── career-mentor-v1.skill       ← 自動打包的壓縮檔
│   │   └── output/
│   │       ├── SKILL.md
│   │       └── references/
│   ├── Gemini_Gem/
│   │   ├── Gemini_DEPLOY-GUIDE.md
│   │   └── output/                      ← 平鋪 10 個文件
│   └── GPTs/
│       ├── GPTs_DEPLOY-GUIDE.md
│       └── output/                      ← 平鋪 10 個文件
│
└── SCRIPT/                      ← 部署自動化腳本
    ├── deploy.ps1               ← 互動式入口（詢問平臺後呼叫對應腳本）
    ├── deploy_claude_code.ps1
    ├── deploy_claude_web.ps1
    ├── deploy_gemini_gem.ps1
    └── deploy_gpts.ps1
```

---

## 部署流程

### 互動式部署（推薦）

在終端機執行：

```powershell
.\SCRIPT\deploy.ps1
```

依提示選擇平臺（1–4），腳本自動完成：
1. 清除對應 `output/` 資料夾
2. 從 `MainFiles/` 複製所有知識庫文件
3. Claude Web 額外重新打包 `career-mentor-v1.skill` 壓縮檔

### 直接執行平臺腳本

```powershell
.\SCRIPT\deploy_claude_code.ps1   # Claude Code CLI
.\SCRIPT\deploy_claude_web.ps1    # Claude.ai Web
.\SCRIPT\deploy_gemini_gem.ps1    # Gemini Gems
.\SCRIPT\deploy_gpts.ps1          # ChatGPT GPTs
```

### 各平臺部署方式

| 平臺 | 部署方式 | 參考文件 |
|------|---------|---------|
| **Claude Code CLI** | 將 `output/` 複製至 `~/.claude/skills/career-mentor/` | `Claude_Code_DEPLOY-GUIDE.md` |
| **Claude.ai Web — 多文件** | `output/references/instructions.md` 貼入 Project Instructions，其餘 9 個文件上傳至 Knowledge | `Claude_Web_DEPLOY-GUIDE.md` |
| **Claude.ai Web — 單文件** | 直接使用 `career-mentor-v1.skill` 壓縮包中的合併文件 | `Claude_Web_DEPLOY-GUIDE.md` |
| **Gemini Gems** | `output/instructions.md` 貼入 Instructions，其餘 9 個文件上傳至 Knowledge | `Gemini_DEPLOY-GUIDE.md` |
| **ChatGPT GPTs** | `output/instructions.md` 貼入 Instructions，其餘 9 個文件上傳至 Knowledge | `GPTs_DEPLOY-GUIDE.md` |

---

## 知識庫文件職責

| 文件 | 職責 | 下游依賴 |
|------|------|---------|
| `instructions.md` | 角色定義、嚴格規則（8 條）、服務路由主控、求職階段推薦邏輯 | 所有文件 |
| `Glossary.md` | 四組術語規範（履歷 / 工作經歷 / STAR 框架 / 服務動詞），禁止替代用法 | 所有文件 |
| `Service_A.md` | 服務 A 7 步驟 + A-1 中翻英支線分叉邏輯 | `Few_Shot_Examples.md` |
| `Service_B.md` | 服務 B 7 步驟 + 違法工作條件偵測 + 口頭版製作 | `Few_Shot_Examples.md` |
| `Service_C.md` | 服務 C 9 步驟 + 試做題合理性評估 | `Few_Shot_Examples.md` |
| `Service_Interview.md` | STAR 4 階段訪談方法論；Service B 執行訪談，Service C 生成題目不執行 | `Service_B.md`、`Service_C.md` |
| `Avoid_Risk.md` | 台灣職場紅旗知識庫、JD 語言紅旗速查表、數位偵查工具清單、試做題邊界指引 | `Service_C.md`（完整）、`Service_B.md`（限 Ch.2） |
| `Resume_Template.md` | 履歷 8 區塊格式規範（自介 / 自傳 / 工作經歷 / 代表專案 / 技能 / 教育 / 性格特徵 / 期望待遇） | `Service_A.md`、`Service_B.md` |
| `Special_Cases.md` | 空窗期、非典型工作、應屆生/實習生、中高齡（45+）、轉職者（跨產業/跨職能）、自僱/創業重返 | `Service_A.md`、`Service_B.md` |
| `Few_Shot_Examples.md` | 三個服務的完整輸出示範，純參考不執行 | — |

---

## 服務路由圖

```
使用者輸入
    │
    ▼
instructions.md（路由主控）
    │
    ├──► 服務 A — 履歷檢視 + 優化 + 生成
    │        主要：Service_A.md
    │        輔助：Resume_Template.md、Special_Cases.md（邊界觸發）
    │        └─► A-1 支線（中翻英）：步驟 2 分叉，翻譯確認後結束
    │
    ├──► 服務 B — 訪談萃取 + 新工作經歷
    │        主要：Service_B.md、Service_Interview.md
    │        輔助：Resume_Template.md、Special_Cases.md（邊界觸發）
    │        限定引用：Avoid_Risk.md Chapter 2（違法工作條件）
    │
    └──► 服務 C — 職缺風險解剖 + 面試準備
             主要：Service_C.md、Avoid_Risk.md
             題型框架：Service_Interview.md（生成題目，不執行訪談）
```

---

## 更新知識庫

只需編輯 `MainFiles/` 內的對應文件，然後重新部署：

```powershell
# 編輯完成後，部署至所有平臺
.\SCRIPT\deploy_claude_code.ps1
.\SCRIPT\deploy_claude_web.ps1
.\SCRIPT\deploy_gemini_gem.ps1
.\SCRIPT\deploy_gpts.ps1
```

> **重要**：`DEPLOYMENT/*/output/` 是自動管理的產物資料夾，請勿直接編輯。  
> `DEPLOYMENT/Claude_Code/SKILL.md` 和 `DEPLOYMENT/Claude_Web/SKILL.md` 是平臺專屬文件，不受 deploy 腳本覆蓋。

---

## 角色設定摘要

- **語言**：所有對話以繁體中文進行
- **稱謂**：對使用者一律稱「您」
- **語氣**：專業、平靜、客觀；無情緒鼓勵
- **範疇外拒絕**：`【抱歉，您詢問的問題不在我職能的回答範疇内，請詢問我關於職涯履歷的相關問題。】`

---

## 版本紀錄

| 版本 | 日期 | 主要變更 |
|------|------|---------|
| v1.0 | 2026-04-29 | 初始版本，服務 A / B / C 基礎流程 |
| v1.1 | 2026-04-29 | Agent review（Workflow Architect + Recruitment Specialist + Technical Writer） |
| v1.2 | 2026-05-14 | 應屆生模組、試做題評估（Service C 步驟 7）、口頭版（Service B 步驟 6）、雙語履歷（A-1 支線）、Few-Shot 擴充、Avoid_Risk 開放 Service B Ch.2 引用 |
| v1.3 | 2026-05-14 | 全系統 Glossary（四組術語規範） |
| v1.4 | 2026-05-14 | 中高齡（45+）/ 轉職者 / 自僱重返職場模組（Special_Cases.md）；JD 語言紅旗速查表；數位偵查工具清單 |
| v1.5 | 2026-05-14 | 跨文件一致性修正（8 個平行 agent 編輯後整合掃描） |
| v1.6 | 2026-05-15 | 專案架構重整：MainFiles + DEPLOYMENT + SCRIPT 三層架構；部署自動化腳本 |

---

## 待開發

- **D-10** — 完整求職作戰計畫：跨服務 B→A→C 串聯的主動引導模式
- **D-11** — 服務 D：求職推薦信撰寫模組（Service_D.md + 格式規範 + Few-Shot 範例）
- **外籍工作者 / 歸國留學生**：台灣工作許可文件、海外學歷在台灣職場的呈現策略
