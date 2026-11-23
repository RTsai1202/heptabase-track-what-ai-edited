# Heptabase Track Changes

[English](README.md)

讓外部 AI 改 Heptabase 卡片時，你看得出改了什麼的 Heptabase Skill。

Heptabase 內建 AI 修改時會先用紅綠 diff 給你看。外部 AI 透過 MCP 或 CLI 修改時會直接寫入，看不出差異。這個 Skill 讓外部 AI 把每處修改標在原地：

| 修改 | 呈現方式 |
| --- | --- |
| 刪掉的文字 | 紅字加刪除線 |
| 新增的文字 | 綠字 |
| 每一處修改 | 上方加一個 ToDo：`🤖 AI edit E1 · 2026-11-23 14:05 · Claude Code · 精簡結論段` |

ToDo 標記讓你在白板上一眼找到修改處並跳過去。審完後，勾選你接受的標記，再請 AI 收尾。AI 會清掉標記：接受的改成一般文字，拒絕的還原。

## 為什麼做成 Heptabase Skill

Heptabase 把 Skill 存成你空間裡的一張指令卡。Heptabase 內建 AI 和所有連線的 AI（Claude Code、Codex、Claude.ai、ChatGPT 等）都讀同一張卡，只要維護一個地方。這個 repo 只放安裝用的原文。

Heptabase 內建的 diff 畫面只存在 App 裡，不會存進卡片內容；「Created by AI」也只標記新建的卡片。顏色和 ToDo 是一般內容，所以每個 AI 都能用，同步也不會掉。

## 安裝

對已連上 Heptabase（MCP 或 CLI）的 AI 說：

> 依照 https://github.com/RTsai1202/heptabase-track-changes 的 INSTALL.md 安裝這個 Heptabase Skill。

或在 Heptabase 手動建立：新增一個 Skill，指令、名稱、說明照 [`skill/skill.json`](skill/skill.json)，指令內容貼上 [`skill/instructions.md`](skill/instructions.md)。

選用：有自己 skill 資料夾的 AI 工具，可另外安裝 [`agent-skill/heptabase-track-changes`](agent-skill/heptabase-track-changes/SKILL.md)。它只是一個指標，讓工具每次改 Heptabase 內容時都會去讀 Heptabase 裡的 Skill。

## 使用

- 「改寫這張卡片的第二段。」AI 先問一次要標記還是直接改，再標記修改。
- 「幫這張卡片的 AI 修改收尾。」勾選的標記會被接受，沒勾的保留。
- 「接受 E1，拒絕 E2。」／「全部接受。」
- 在 Heptabase App 裡輸入 `/track-changes`。

## 限制

- 程式碼區塊和數學公式區塊不能上色。AI 會保留舊區塊，把新區塊放在下方。
- 標記模式下，AI 不刪圖片、嵌入內容和複雜表格，只在 ToDo 裡描述建議。
- 收尾只處理標記範圍內的紅色刪除線與綠字，你在別處自己用的顏色不受影響。

## 授權

MIT
