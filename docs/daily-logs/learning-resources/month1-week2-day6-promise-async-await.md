# 學習資源 — Month 1 / Week 2 / Day 6：Promise 生命週期 & async/await

> 對應 `daily-logs/month1-week2-day6.md`。這份資源清單有一個重點修正：你筆記裡「async/await 只是 Promise 的語法糖」這句話，嚴格來說不完全精確，值得花點時間釐清。

## 1. Promise 生命週期（三種狀態）

- [MDN — Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)：官方指南，完整說明 pending/fulfilled/rejected 三種狀態，以及 `.then()`/`.catch()`/`.finally()` 的行為，跟你今天筆記的內容完全對應。
- [MDN — Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)：Reference 文件，適合查閱 `Promise.all`/`Promise.race`/`Promise.allSettled` 等靜態方法的精確定義（明天會用到）。

## 2. async/await「只是語法糖」這個說法的重要修正

**這是今天最值得補充的一點**。搜尋資料顯示業界對「async/await 只是語法糖」這個說法有明確的反駁，你的筆記可以更精確一點：

- DEV Community — [async/await is NOT just syntax sugar for Promises](https://dev.to/bishoy_bishai/asyncawait-is-not-just-syntax-sugar-for-promises-312e)：文章用了一個生動比喻——<cite index="12-1">把 async/await 說成只是 Promise 的語法糖，就像把一台高性能跑車說成只是「一匹跑得比較快的馬」；兩者確實都能帶你從 A 到 B，但底層的工程設計、控制能力跟表現力完全是不同等級</cite>，並指出<cite index="12-1">async/await 帶來可預測的錯誤處理——不用再猜哪個 .catch() 會被觸發、或錯誤為什麼憑空消失，try/catch 的行為完全符合預期；同時提供更豐富的 stack trace，讓循序偵錯更省時省力</cite>。
- **一個更技術性的修正（來自實測驗證）**：GitHub Gist — [async await is just syntactic sugar.js](https://gist.github.com/joeytwiddle/ff0a9cf92be0366b274e4283a8b5a935)：<cite index="19-1">這個說法並不完全正確——一個 async 函式會先同步執行，直到遇到 return 或 throw 陳述式讓它轉換成 Promise，或是遇到 await 才會真正暫停執行</cite>。這句話補充了一個重要細節：**async 函式一開始其實是同步跑的**，只有跑到第一個 `await` 才會真正讓出執行權，這比單純說「它就是 Promise Chain」更精確一層。
- 對照參考（另一派意見，強調底層仍是 Promise）— [zhenghao.io — Why async/await is more than just syntactic sugar](https://www.zhenghao.io/posts/await-vs-promise)：<cite index="16-1">async/await 帶來的實際好處是讓我們能使用同步程式設計裡才有的語言結構，寫出更具表達力、更易讀的程式碼</cite>，同時也承認 <cite index="20-1">async/await 底層仍然是為了讓 Promise-based 的非同步程式設計更容易讀寫而存在的語法糖</cite>——兩派說法的分歧其實在於「語法糖」這個詞會不會讓人誤以為它「只是」表面包裝、沒有實質差異，而不是在爭論它底層是不是用 Promise 實作。

### 給你的建議修正說法

比較精確的講法是：**async/await 底層仍然建立在 Promise 之上（回傳值一定是 Promise，錯誤處理仍然是 microtask 排程），但它不只是純語法轉換——它改變了程式碼的控制流結構（可以用 try/catch、for 迴圈等同步語法處理非同步邏輯），帶來更好的可讀性跟錯誤堆疊追蹤能力**。「語法糖」這個詞沒有錯，但容易讓人低估這個語法轉換帶來的實際工程效益。

## 3. Promise Chain → async/await 改寫練習（延伸範例）

- [freeCodeCamp — When to Use Async/Await vs Promises in JavaScript](https://www.freecodecamp.org/news/when-to-use-asyncawait-vs-promises-in-javascript/)：<cite index="17-1">用 Promise Chain 處理非同步邏輯常見的除錯困難是：透過層層 .then() 追蹤 stack trace 比較困難，而且容易忍不住把同步邏輯寫進 .then() 區塊裡，導致同步/非同步邏輯混雜、難以閱讀</cite>——這正是你今天示範的「回調地獄」問題的具體化說明。

## 4. 你踩到的坑（忘記 `await`）的延伸驗證

今天你忘記加 `await` 導致印出 `[object Promise]`，這其實可以進一步延伸驗證「async 函式一開始是同步執行」這個細節：

```js
async function testSync() {
  console.log("1: 進入 async function（同步執行）");
  const result = await Promise.resolve("resolved value");
  console.log("3: await 之後才執行到這行", result);
  return result;
}

console.log("start");
testSync();
console.log("2: 這行會比 async function 裡的 await 之後那行先印出");
```

跑跑看，觀察 `"1: 進入 async function"` 是不是在 `console.log("2: ...")` 之前就印出來了——這證明了 async 函式呼叫的當下，函式體會先同步執行到第一個 `await` 為止，不是整個函式都被丟進 microtask queue。

## 5. 自我檢查（面試題自測）

1. Promise 的三種狀態轉換規則是什麼？為什麼「settled 之後不可逆」是個重要特性？
2. `.then()` 的兩個參數分別對應什麼？`.catch()` 跟 `.finally()` 各自的語意是什麼？
3. 為什麼說「async/await 只是語法糖」這個說法不夠精確？更準確的說法應該怎麼講？
4. `async` 函式被呼叫的當下，函式體是同步執行還是直接進入 microtask queue？從哪裡開始才真正暫停？
5. 忘記寫 `await` 會發生什麼事？這個現象怎麼證明 `await` 真的在做「暫停等待」這件事？

---

> 下一份：`month1-week2-day7.md` 對應資源會聚焦 `Promise.all` vs `Promise.race` 的比較，以及非同步 Error Handling 模式——今天用 `try/catch` 處理單一 async 函式的錯誤，明天要延伸到「多個非同步操作、部分失敗」這種更複雜的情境。
