# 學習資源 — Month 1 / Week 2 / Day 7：Promise 組合方法 & 非同步錯誤處理

> 對應 `daily-logs/month1-week2-day7.md`。這份資源清單額外補了一個你今天筆記裡沒提到的第四個組合方法 `Promise.any`，以及你發現的「Promise 沒有內建取消機制」這個技術債的完整解法（AbortController）。

## 1. Promise 組合方法完整比較（補上 `Promise.any`）

- [javascript.info — Promise API](https://javascript.info/promise-api)：官方等級的完整參考，<cite index="19-1">Promise.allSettled 會等所有 Promise 都 settle，並把結果整理成物件陣列回傳；Promise.race 則只等第一個 settle 的 Promise，把它的結果（不論成功失敗）當作最終結果</cite>，並且<cite index="19-1">一旦第一個 settle 的 Promise「贏得比賽」，之後其他 Promise 的結果或錯誤都會被忽略</cite>——這點你今天筆記裡已經有記錄到。
- DEV Community — [.all() vs .allSettled() and .race() vs .any()](https://dev.to/shameel/javascript-promise-all-vs-allsettled-and-race-vs-any-3foj)：**補上你今天沒測到的第四個方法**。`Promise.any` 常被拿來跟 `Promise.race`搞混，兩者的差異是：<cite index="12-1">as the name suggests, race 會回傳第一個 settle 的 Promise（不論成功失敗），而 any 則是等到有 Promise 成功為止，只有全部都失敗時才會 reject</cite>。可以想成：`race` 是「誰先到都算數」，`any` 是「誰先成功才算數，全部失敗才算輸」——適合「同時打多個備援 CDN、只要有一個成功就好」的情境。

## 2. `Promise.all` reject 後其他 Promise 為什麼還在跑（今天最重要的發現）

**這是今天最有價值的一個技術債，找到了明確的官方層級討論來源：**

- WHATWG Fetch 規格討論串 — [Issue #1831](https://github.com/whatwg/fetch/issues/1831)：這是瀏覽器標準制定者之間的正式技術討論，明確指出<cite index="20-1">Promise 組合方法（Promise.race、Promise.any、Promise.all、Promise.allSettled）目前有一個重大限制：只要陣列裡任何一個 Promise reject，其他所有 Promise 依然必須在背景繼續執行到完成、直到 settle 為止，Event Loop 不會主動中止它們</cite>。這證實了你今天實測發現的現象不是巧合或誤解，而是**目前 Promise 組合方法規格層級的已知限制**，連瀏覽器規格制定者自己都在討論怎麼解決。
- 同一份討論串也解釋了為什麼修這件事很難：<cite index="17-1">AbortController 物件只能取消「已經開始執行」的 Promise；對一個已經完成的 Promise 執行 abort 應該是靜默無效果（no-op）；而對「正在進行中」的請求執行 abort，在目前 Promise 的設計架構下也存在挑戰</cite>——換句話說，「取消」這個概念在 Promise 的原始設計裡本來就沒有被考慮進去，是後來才用 `AbortController` 這個獨立機制外掛上去的。

## 3. `AbortController`：解決今天記錄的技術債

- [W3Tweaks — AbortController: Cancel Fetch Properly (2026)](https://www.w3tweaks.com/javascript/javascript-abortcontroller-cancel-fetch/)：<cite index="16-1">fetch() 本身沒有內建的方式可以取消一個請求；一旦請求已經送出，即使你不再需要那個結果，它依然會執行到完成</cite>，這會導致兩個實際問題：<cite index="16-1">浪費頻寬去抓沒有人會用到的回應資料，以及很常見的「fetch race condition」——當一個較慢的舊請求比較快的新請求還晚 resolve，結果用過期的舊資料覆蓋掉正確的新資料</cite>。這篇文章有互動 demo，示範在搜尋框快速輸入時，怎麼用 AbortController 取消過期的請求。
- Jamdesk — [JavaScript Promise.all() and Promise.allSettled() in Practice](https://www.jamdesk.com/blog/javascript-promise-all)：給了一個很實用的判斷原則——<cite index="18-1">全部結果都是必要的就用 Promise.all()；可以接受部分失敗就用 Promise.allSettled()</cite>，並提醒<cite index="18-1">Promise.race() 的經典用途是實作 timeout，搭配 AbortController 可以在超時的當下真正把還在進行中的請求取消掉，而不是放著讓它在背景空轉</cite>——這正好回答了你今天筆記裡「需要另外搭配 AbortController」的具體用法。
- DEV Community — [Tackling Asynchronous Bugs: Race Conditions and Unresolved Promises](https://dev.to/alex_aslam/tackling-asynchronous-bugs-in-javascript-race-conditions-and-unresolved-promises-7jo)：提供一段可以直接參考的 React 慣用寫法：<cite index="14-1">在 useEffect 裡建立 AbortController，並在 cleanup function 呼叫 controller.abort()，確保元件卸載時能取消還在進行中的非同步請求，避免對已卸載元件呼叫 setState</cite>——這是 Month 1 Week 3-4（React 專案初始化）跟 Month 2（串接真實 API）會直接用到的模式，值得先記下來。

## 4. 用 `Promise.race` + `AbortController` 實作 Timeout（延伸練習）

Builder.io 的文章給了一個具體範例可以參考：

```js
function withTimeout(promise, ms) {
  const controller = new AbortController();
  const timeout = new Promise((_, reject) =>
    setTimeout(() => {
      controller.abort(); // 真正中止底層請求，而不是放著讓它繼續跑
      reject(new Error("Timed out"));
    }, ms)
  );
  return Promise.race([promise, timeout]);
}
```

<cite index="13-1">如果原本的非同步操作是像 fetch 這種請求，通常會希望在 timeout 觸發時用 AbortController 真正中止這個請求，而不是只是讓 race 產生的 Promise 結束、卻放任原本的請求繼續在背景空轉</cite>——這正是你今天技術債筆記想解決的問題的具體實作方式，之後 Month 2 串接真實 Transaction API 時可以直接套用這個模式。

## 5. 自我檢查（面試題自測）

1. `Promise.all`、`Promise.race`、`Promise.allSettled`、`Promise.any` 四者的 resolve/reject 觸發條件分別是什麼？
2. 為什麼 `Promise.all` reject 之後，其他 Promise 依然會繼續執行？這是設計缺陷還是刻意的規格決定？
3. `AbortController` 解決的是什麼問題？它跟單純用 `Promise.race` 做 timeout 有什麼差異？（提示：只用 race 能不能真正停止底層的 fetch 請求？）
4. 什麼是「fetch race condition」？為什麼在使用者快速輸入搜尋框時特別容易發生？
5. 如果你要批次上傳 5 個檔案，只要有任一個上傳失敗就要整批中止並回滾，你會選哪個 Promise 組合方法？如果是想知道每個檔案各自成功或失敗、失敗的不影響其他檔案呢？

---

> 下一份：`month1-week2-day8.md` 對應資源會聚焦 debounce/throttle 的實作與單元測試——今天記錄的 AbortController 技術債，會在之後串接真實 API（Month 2）時正式派上用場，這裡先建立好觀念即可。
