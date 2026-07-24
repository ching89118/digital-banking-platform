# Month 1 / Week 1 / Day 5 — 2026-07-24

## 今日任務

- 暖身任務 [10min]：Prototype Chain + `new` 運作原理筆記
- 核心任務 [30min]：Event Loop / Microtask vs Macrotask 實驗程式（console.log 順序題）

## 今日目標

補完昨天實作 `myBind()` 時沒深究完的部分——`new` 底層到底做了哪四件事，為什麼箭頭函式無法被 `new`；接著銜接 Day 3 提過的 MDN execution model 文章裡「Job queue and event loop」段落，寫一系列 `console.log` 順序題，實際驗證 microtask（Promise）永遠比 macrotask（setTimeout）優先清空的規則。

## 完成定義

- [x] Prototype Chain + `new` 筆記完成，能列出 `new` 運作的四個步驟，並解釋為什麼箭頭函式不能被 `new`
- [x] Event Loop demo 可執行，至少 3 題不同複雜度的 `console.log` 順序題，執行結果與自己的預測一致
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`

## 執行紀錄

### 暖身任務

- 花費時間：10 分鐘
- 筆記重點：
  1. **`new Fn(...)` 底層四步驟**：① 建立一個全新的空物件；② 把這個新物件的 `[[Prototype]]` 設定為 `Fn.prototype`；③ 以這個新物件作為 `this`，執行 `Fn` 函式本體（也就是 `new` 綁定）；④ 如果 `Fn` 回傳的是物件就用那個回傳值，否則自動回傳步驟 ① 建立的新物件。
  2. 回答昨天的疑問：箭頭函式不能被 `new`，根本原因不是「沒有自己的 `this`」這麼簡單，而是箭頭函式在規格層級**根本沒有 `[[Construct]]` 這個內部方法**——`new` 運算子執行時會先檢查目標函式有沒有 `[[Construct]]`，沒有就直接拋出 `TypeError: X is not a constructor`，連「嘗試綁定 this」這一步都不會執行到。
  3. Prototype Chain 跟 Scope Chain 是两條完全不同的查找鏈：Scope Chain 查的是「變數」，順著詞法巢狀關係往外找；Prototype Chain 查的是「物件屬性/方法」，順著 `[[Prototype]]` 一路往上找到 `Object.prototype` 為止。兩者的共同點只有「都是鏈狀往外/往上查找，找不到才算真的不存在」。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：寫了三題 `console.log` 順序題，複雜度遞增：
  1. **基本題**：同步程式碼 + 一個 `setTimeout(fn, 0)` + 一個 `Promise.resolve().then(fn)`，驗證同步程式碼永遠最先跑完，接著是 microtask（Promise），最後才輪到 macrotask（setTimeout），即使 setTimeout 的延遲時間設成 0。
  2. **巢狀 microtask 題**：在一個 `.then()` 裡面再產生新的 `.then()`，驗證新產生的 microtask 會被加進同一輪的 microtask queue 尾端，並在下一個 macrotask 開始前被清空——也就是 microtask queue 沒清空之前，macrotask 絕對輪不到。
  3. **run-to-completion 驗證題**：仿照 Day 3 讀過的 MDN execution model 文章裡的例子，對同一個已解決的 Promise 掛兩個 `.then()`，各自對同一個外部變數做「遞增再印出」，驗證輸出順序永遠是可預測的 `1` `2`，而不會出現交錯的 race condition——因為每個 job 都會完整跑完才輪到下一個。
- 遇到的問題：第 2 題一開始預測錯了，以為巢狀產生的新 `.then()` 要等到「下一輪」macrotask 之後才會執行，實際跑出來的結果是它還在同一輪就被清空了。
- 解法：回頭重讀 Day 3 那篇 MDN 文章裡的敘述——engine 每次處理 job 時是「先把 microtask queue 清空，才處理下一個 macrotask」，而且清空的過程是「持續清到 queue 真的空了為止」，不是只清「目前這一批」；所以只要 microtask 裡還在不斷產生新的 microtask，macrotask 就會一直被延後，這也是為什麼寫錯的遞迴 Promise chain 可能會讓畫面卡住不渲染的原因。

## Commit

```
feat(js-runtime): add prototype/new notes and event loop ordering demos
```

## 今天學到的三件事

1. `new` 底層是「建立空物件 → 設 prototype → 綁 this 執行建構函式 → 決定回傳值」四步驟，箭頭函式無法被 `new` 的根本原因是規格上就沒有 `[[Construct]]`，不是單純「沒有自己的 this」這麼簡單。
2. Microtask queue 會被「持續清空到真的空了」才輪到下一個 macrotask，這代表巢狀不斷產生的 microtask 有機會餓死 macrotask（甚至讓畫面卡住不渲染）。
3. Promise 的 `.then()` 執行順序完全可預測，不是競態條件——因為每個 job 都遵守 run-to-completion，執行到一半不會被其他 job 插隊。

## 面試可能會問的一個問題

Q: `setTimeout(fn, 0)` 跟 `Promise.resolve().then(fn)` 同時存在時，誰先執行？為什麼？
A: `Promise.resolve().then(fn)` 先執行。因為 `setTimeout` 產生的 callback 屬於 macrotask，而 Promise 的 `.then()` callback 屬於 microtask；JS engine 的規則是每次同步程式碼執行完畢後，會先把 microtask queue **完全清空**，才會去處理下一個 macrotask。即使 `setTimeout` 的延遲時間設成 0，它排進的仍然是 macrotask queue，排隊順序上永遠排在當前這一輪的所有 microtask 之後。

## 明日待辦

- Promise 生命週期筆記（下週一暖身任務，進入 Week 2：Async 與工程能力）
- 這週的 JavaScript Runtime 主題到此告一段落，週五驗收時要檢查 `JavaScript Runtime Notes.md` 是否完整涵蓋 Execution Context、Call Stack、Scope Chain、Closure、this/call/apply/bind、Prototype/new、Event Loop 七大主題，並整理成可以獨立閱讀的技術文件。
