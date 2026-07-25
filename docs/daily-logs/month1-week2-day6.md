# Month 1 / Week 2 / Day 6 — 2026-07-25

## 今日任務

- 暖身任務 [5min]：Promise 生命週期筆記
- 核心任務 [30min]：Promise Chain 改寫成 async/await 練習

## 今日目標

進入 Week 2：Async 與工程能力。先搞懂 Promise 的三種狀態（pending/fulfilled/rejected）跟狀態轉換規則，接著把一段多層 `.then()` 巢狀的 Promise Chain 改寫成 async/await，驗證兩種寫法在語意上是等價的——本質上 async/await 只是 Promise Chain 的語法糖，底層排進 microtask queue 的行為跟 Week 1 Day 5 驗證過的規則完全一致。

## 完成定義

- [x] Promise 生命週期筆記完成，能畫出三種狀態的轉換圖，並說明狀態一旦轉換就不可逆
- [x] 改寫練習完成，原本的 Promise Chain 版本跟 async/await 版本執行結果一致
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`（銜接 Week 1 內容，作為 Async 主題的開篇）

## 執行紀錄

### 暖身任務

- 花費時間：5 分鐘
- 筆記重點：
  1. **三種狀態**：`pending`（初始狀態，尚未有結果）→ `fulfilled`（成功，帶有 value）或 `rejected`（失敗，帶有 reason）。狀態一旦從 `pending` 轉換為 `fulfilled` 或 `rejected`，就永久固定，不會再變——這個「settled 之後不可逆」的特性，正是 Promise 比單純用 callback 更可靠的原因，不用擔心同一個結果被「重複觸發」。
  2. `.then()` 掛的兩個 callback 分別對應 fulfilled 跟 rejected 兩種狀態的處理方式；`.catch()` 其實只是 `.then(undefined, onRejected)` 的語法糖；`.finally()` 不管最終是哪個狀態都會執行，且不會拿到 value/reason 當參數。
  3. Promise 物件本身建立的當下就會**立即同步執行** executor 函式（`new Promise((resolve, reject) => {...})` 裡的那段程式碼），只有 `.then()`/`.catch()` 掛的 callback 才會被排進 microtask queue，這點跟 Week 1 Day 5 驗證過的 microtask 行為銜接。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：
  1. 先寫一段 3 層巢狀的 Promise Chain（模擬「取得使用者 → 取得使用者的訂單 → 取得訂單明細」這種依序相依的非同步操作），刻意示範巢狀 `.then()` 讀起來像回調地獄的樣子。
  2. 把它改寫成 async/await 版本：整段邏輯攤平成看起來像同步的循序寫法，用 `try/catch` 取代 `.catch()`。
  3. 分別在兩個版本前後加上 `console.log` 時間戳記，確認兩者的非同步排程行為（何時讓出主執行緒、何時排進 microtask queue）完全一致，只是寫法不同。
- 遇到的問題：一開始在 async/await 版本裡忘記加 `await`，直接把回傳的 Promise 物件當成已經 resolve 的值使用，導致印出 `[object Promise]` 而不是預期的資料。
- 解法：這個錯誤反而是個好教材——它證明了 `await` 不是語法上的裝飾，而是真的在做「暫停當前 async function 執行、等待 Promise settle、再取出 value 繼續往下跑」這件事；少了 `await`，拿到的就是 Promise 物件本身，不是它 resolve 出來的值。

## Commit

```
feat(js-runtime): add promise lifecycle notes and async/await refactor demo
```

## 今天學到的三件事

1. Promise 的狀態轉換是單向且不可逆的：`pending` 只能轉一次，轉成 `fulfilled` 或 `rejected` 後就永久固定，這是它比 callback 更可預測的核心原因。
2. `async/await` 是 Promise 的語法糖，不是另一套非同步機制——底層排程（microtask queue）的行為跟 Week 1 驗證過的規則完全一樣，只是把巢狀的 `.then()` 攤平成看起來同步的寫法。
3. 忘記加 `await` 會直接拿到 Promise 物件而不是它的值，這個錯誤很好地證明了 `await` 實際上在做「暫停等待」這件事，不是純語法裝飾。

## 面試可能會問的一個問題

Q: `async/await` 底層是怎麼運作的？它跟 Promise Chain 有什麼本質上的差異嗎？
A: `async/await` 本質上是 Promise Chain 的語法糖，讓非同步程式碼可以用接近同步的順序寫法表達，並不是一套獨立的非同步機制。`async` 函式本身一定回傳一個 Promise；函式內遇到 `await expression` 時，會暫停當前函式的執行，把 `expression`（通常是一個 Promise）的結果排進 microtask queue 等待 settle，settle 後才恢復函式往下執行。底層排程規則（何時是 microtask、何時清空 queue）跟直接寫 `.then()` 完全一致，差別只在語法可讀性——尤其在多層相依的非同步操作情境下，async/await 能避免巢狀 `.then()` 造成的回調地獄。

## 明日待辦

- Promise.all vs Promise.race 比較表（明天暖身任務）
- 核心任務會做非同步 Error Handling 模式；今天用 `try/catch` 處理 async/await 裡的錯誤，明天要更完整地比較 `Promise.allSettled` 跟單純 `try/catch` 在「多個非同步操作、部分失敗」情境下的處理方式差異。

---

## Week 1 收尾補記

Week 1（JavaScript Runtime）七大主題已於 Day 5 完成，`JavaScript Runtime Notes.md` 涵蓋 Execution Context、Call Stack、Scope Chain、Closure、this/call/apply/bind、Prototype/new、Event Loop。今天起 Week 2 的筆記會延續同一份文件，讓 Async 主題自然接續在 Event Loop 章節之後，維持技術文件的閱讀連貫性。
