# Daily Logs

---

```markdown
# Month 1 / Week 1 / Day 1 — 2026-07-20

## 今日任務

- 暖身任務 [10min]：建立 Git Repository + README Template
- 核心任務 [30min]：JavaScript Execution Context 概念 + 撰寫 demo code

## 今日目標

完成專案 Git 骨架與 README 模板，並搞懂 Execution Context 的組成（Variable Environment、Lexical Environment、This Binding），用 demo code 驗證呼叫時機如何決定執行環境。

## 完成定義

- [x] Git repo 已初始化，README 包含專案狀態、技術棧、結構與路線圖
- [x] Execution Context demo 可執行，能對照 global / function / eval 三種情境
- [x] 已 commit
- [x] 筆記寫入 JavaScript Runtime Notes.md

## 執行紀錄

### 暖身任務

- 花費時間：10 分鐘
- 筆記重點：
  1. README 先寫清「目前階段 / 下一個里程碑」，之後每日進度才有錨點可更新。
  2. 專案結構先預留 `frontend/` `backend/` `docs/`，即使資料夾尚未建立，也能對齊 ROADMAP。
  3. Badge（status / progress）用 Day 計數，方便 Month 1 結束時一眼看出完成度。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：寫了 Execution Context demo，分別展示 global EC 建立、函式呼叫時推入 call stack、以及 `this` 在不同呼叫方式下的綁定差異。
- 遇到的問題：一開始把「作用域（Scope）」跟「執行環境（Execution Context）」混在一起講，導致 demo 註解讀起來像在講 closure。
- 解法：先固定三件事來對照 —— 誰建立了 EC、LE 往外找變數的鏈、以及 `this` 由呼叫方式決定；closure 留給後續任務。

## Commit
```

docs(ai): document daily-logs month1-week1-day1

```

## 今天學到的三件事
1. 每次函式被呼叫都會建立一個新的 Execution Context，並推入 call stack。
2. Lexical Environment 負責變數查找；This Binding 是 EC 的一部分，但由「怎麼被呼叫」決定，不是由「在哪裡定義」決定。
3. README 模板要先寫狀態與結構，之後每日任務才有地方回填進度。

## 面試可能會問的一個問題
Q: 什麼是 Execution Context？它跟 Scope 有什麼差別？
A: Execution Context 是程式碼執行當下的環境，包含變數環境、詞法環境與 this 綁定；Scope 則是變數可見性規則。簡單說，Scope 決定「找得到哪些變數」，Execution Context 決定「這次執行用哪一套環境」。

## 明日待辦
- Call Stack 圖解筆記（明天暖身任務）
```

---
