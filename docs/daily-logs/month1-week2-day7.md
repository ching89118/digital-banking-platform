# Month 1 / Week 2 / Day 7 — 2026-07-26

## 今日任務

- 暖身任務 [10min]：Promise.all vs Promise.race 比較表
- 核心任務 [30min]：非同步 Error Handling 模式（try/catch/finally、Promise.allSettled）

## 今日目標

昨天用 `try/catch` 處理了單一 async 函式的錯誤，今天要延伸到更複雜的情境——當一次要處理**多個**非同步操作時，`Promise.all`、`Promise.race`、`Promise.allSettled` 三者在「部分失敗」情境下的行為完全不同，搞懂這三者的差異，並找出各自適合的錯誤處理模式。

## 完成定義

- [x] Promise.all vs Promise.race 比較表完成，含各自的 resolve/reject 觸發條件
- [x] Error Handling demo 可執行，能證明 `Promise.all` 遇到任一失敗會整體 reject，而 `Promise.allSettled` 不會
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`

## 執行紀錄

### 暖身任務

- 花費時間：10 分鐘
- 筆記重點（比較表）：

  |              | `Promise.all`                                                         | `Promise.race`                                                                              |
  | ------------ | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
  | resolve 條件 | 全部 Promise 都 fulfilled，回傳一個依原順序排列的結果陣列             | 只要有任何一個 Promise 先 settle（不管是 fulfilled 還是 rejected），就立即用那個結果 settle |
  | reject 條件  | 只要有任一 Promise rejected，立即整體 reject（不等其他 Promise 跑完） | 同上，只看「誰最快 settle」，不區分成功失敗                                                 |
  | 適用情境     | 需要「全部都成功才算成功」的批次操作（例如同時打三支 API，缺一不可）  | 需要「最快的結果」的情境（例如同時發請求給多個備援伺服器，取最快回應的那個）                |

  額外補充：`Promise.all` 是「一個失敗、全部失敗」，這在多個非同步操作彼此獨立（沒有互相依賴）時其實不太理想——如果只是想知道「每個各自成功還是失敗」，不該讓一個失敗拖累其他已經成功的結果，這正好帶出核心任務要驗證的 `Promise.allSettled`。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：
  1. 用 `Promise.all` 打包三個非同步操作（其中一個刻意設計成會 reject），驗證整個 `Promise.all` 立即 reject，另外兩個已經成功的結果完全被忽略，拿不到。
  2. 換成 `Promise.allSettled` 打包同樣三個操作，驗證回傳的是一個陣列，每個元素都是 `{ status: "fulfilled", value }` 或 `{ status: "rejected", reason }`，即使有失敗，成功的結果依然保留得到。
  3. 分別用 `try/catch`（搭配 `Promise.all`）跟直接檢查 `allSettled` 回傳陣列裡每個元素的 `status`，比較兩種錯誤處理路徑的程式碼結構差異。
  4. 加了 `finally` 區塊，驗證不管走 `try` 還是 `catch`，`finally` 都會執行（用來模擬「不管成功失敗都要關閉 loading 狀態」這種常見 UI 場景）。
- 遇到的問題：一開始誤以為 `Promise.all` reject 之後，其他還在進行中的 Promise 會被「取消」，實際測試發現並不會——那些 Promise 依然會繼續執行到完成，只是它們的結果不會再被 `Promise.all` 用到（沒有人在監聽了）。
- 解法：這個發現提醒了一個重要限制：JavaScript 原生 Promise 沒有內建取消機制，`Promise.all` 提早 reject 只是「不等了」，不是「叫其他人停下來」。如果真的需要取消非同步操作（例如使用者離開頁面就該中止還在跑的 fetch），需要另外搭配 `AbortController`，這個記錄下來作為之後的延伸主題，不在今天的範圍內。

## Commit

```
feat(js-runtime): add promise.all vs race comparison and error handling demos
```

## 今天學到的三件事

1. `Promise.all` 是「一個失敗、全部失敗」，`Promise.allSettled` 則是「不管成功失敗，全部都要等到、都要回報結果」——選哪個取決於這些非同步操作彼此是不是真的相依（缺一不可用 `all`，各自獨立用 `allSettled`）。
2. `Promise.race` 只看「誰最快 settle」，不管那個結果是成功還是失敗，這跟它的名字「race（賽跑）」完全對應——先跑到終點的就是贏家，不管用什麼方式抵達。
3. `Promise.all` reject 後，其他仍在進行中的 Promise 不會被自動取消，只是不再被監聽——JS 原生 Promise 沒有內建取消機制，真的要取消需要 `AbortController`。

## 面試可能會問的一個問題

Q: `Promise.all`、`Promise.race`、`Promise.allSettled` 的差異是什麼？你會在什麼情境下分別選用？
A: `Promise.all` 要求全部 Promise 都成功才 resolve，只要有一個失敗就立即整體 reject，適合「多個操作缺一不可」的情境，例如同時打三支互相依賴的 API；`Promise.race` 只看誰最快 settle（不論成功失敗），適合「取最快回應」的情境，例如同時對多個備援伺服器發請求；`Promise.allSettled` 不管成功失敗都會等所有 Promise 完成，回傳每個各自的結果狀態，適合「多個獨立操作、想知道各自成敗但不希望一個失敗拖累其他結果」的情境，例如批次上傳多個檔案，想知道哪些成功哪些失敗，而不是一失敗就整批放棄。

## 明日待辦

- debounce/throttle 概念卡（明天暖身任務）
- 核心任務會實作 `debounce()` + `throttle()` 並補上單元測試；今天發現的「Promise 沒有內建取消機制、需要 AbortController」可以先記錄成技術債，等 Month 2-3 串接真實 API 時再回頭處理。
