# Month 1 / Week 1 / Day 2 — 2026-07-21

## 今日任務

- 暖身任務 [10min]：Call Stack 圖解筆記
- 核心任務 [30min]：Scope Chain 實作範例（3 層巢狀函式）

## 今日目標

搞懂 Call Stack 的 push/pop 機制如何對應函式呼叫順序，並用 3 層巢狀函式證明 Scope Chain 是沿著「函式定義時的詞法位置」往外找變數，不是沿著「呼叫順序」找。

## 完成定義

- [x] Call Stack 圖解筆記完成，能畫出至少一個 3 層呼叫的 push/pop 過程
- [x] Scope Chain demo 可執行，能證明內層函式能存取外層變數，但反之不行
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`

## 執行紀錄

### 暖身任務

- 花費時間：10 分鐘
- 筆記重點：
  1. Call Stack 是 LIFO（後進先出）：函式被呼叫時 push 一個新 frame，函式執行完（return 或拋出例外）就 pop 掉。
  2. Stack Overflow 的本質就是 push 的速度大於 pop 的速度，最常見原因是遞迴沒有正確的終止條件。
  3. 用 Loupe 貼了昨天 Execution Context demo 的程式碼，實際看到 `bar()` 呼叫 `foo()` 時，stack 上會同時疊著 `global` → `bar` → `foo` 三層，`foo` 執行完先被 pop，才輪到 `bar` 被 pop。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：寫了 3 層巢狀函式（`outer` → `middle` → `inner`），讓 `inner` 存取 `outer` 定義的變數，驗證 Scope Chain 會沿著函式「定義時」的巢狀關係往外查找，而不是沿著呼叫堆疊往回找。額外測試了「函式在 A 處定義、卻在 B 處被呼叫」的情境，確認 scope chain 認的是定義位置（lexical scoping），不是呼叫位置。
- 遇到的問題：一開始誤以為 Scope Chain 跟 Call Stack 是同一條路徑，寫了一個「在深層呼叫堆疊裡的函式，卻想去存取呼叫者的區域變數」的測試，結果拿到 `ReferenceError`，才意識到兩者是完全不同的機制。
- 解法：把兩個概念拆開驗證：Call Stack 決定「現在執行到哪、等一下要回到哪」；Scope Chain 決定「這個變數名稱去哪裡找」，兩者只有在「函式剛好是巢狀定義」時看起來相關，實際上是獨立運作的。

## Commit

```
feat(js-runtime): add call stack notes and scope chain demo (3-level nesting)
```

## 今天學到的三件事

1. Call Stack 管的是「執行順序與返回位置」，Scope Chain 管的是「變數去哪裡找」——這是今天最重要的釐清，昨天筆記裡其實混在一起講過。
2. Scope Chain 由函式「定義時」的詞法位置決定（lexical scoping），跟函式實際「在哪裡被呼叫」無關。
3. 呼叫堆疊很深的函式，不代表它能存取呼叫鏈上任何一層的區域變數——那條路徑對 Scope Chain 來說根本不存在。

## 面試可能會問的一個問題

Q: Call Stack 跟 Scope Chain 有什麼差別？很多人會搞混，你怎麼解釋？
A: Call Stack 是執行時期的機制，記錄函式呼叫的順序，決定「現在在哪一層、等一下要回到哪裡」；Scope Chain 是編譯/定義時期就決定的變數查找路徑，只跟函式「寫在哪裡（巢狀關係）」有關，跟它實際「被誰呼叫」無關。兩者唯一的交集是：一個函式的 Execution Context 建立時，會根據它定義時的位置去綁定對應的 Scope Chain，但這個綁定跟它在 Call Stack 上疊在哪一層是兩件事。

## 明日待辦

- Closure 觀念 flashcard（明天暖身任務）
- 核心任務會做 Closure 實作 + 3 個應用案例，今天驗證的「lexical scoping」正好是明天 Closure 能成立的前提，可以先想一下：如果 Scope Chain 是定義時決定的，那為什麼外層函式執行完了，內層函式還能繼續存取外層變數？
