# 學習資源 — Month 1 / Week 1 / Day 3：Closure

> 對應 `daily-logs/month1-week1-day3.md`。今天的核心任務是三個 closure 應用案例，這份資源清單依 counter → memoize → debounce 的順序整理，並針對你踩到的 `JSON.stringify` cache key 坑額外補了解法參考。

## 1. Closure 核心概念

- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)：官方對 lexical scoping 與 closure 關係最完整的說明，跟你昨天 Scope Chain 主題直接銜接，建議先重看一次這篇的開頭，再往下看應用案例。
- [javascript.info — Variable scope, closure](https://javascript.info/closure)：你這週已經看了兩次，第三次可以專注在文章後段對 `var` 迴圈陷阱的說明，直接對照你今天筆記裡寫的「經典面試坑」。
- [W3Tweaks — JavaScript Closures: Every Use Case Explained](https://www.w3tweaks.com/javascript/javascript-closures-explained/) — 把 closure 比喻成「函式的背包」，並直接點出<cite index="16-1">closure 是函式記住其建立當下所在作用域的變數，即使那個作用域已經執行完畢，函式仍保有存取權</cite>，同時提到了進階替代方案：<cite index="16-1">用 WeakMap 而非一般 Map 做物件當 key 的 memoization，可以在物件沒有其他參照時被垃圾回收自動清掉快取，避免記憶體洩漏</cite>，也提醒<cite index="16-1">如果一定要用 Map，搭配 JSON.stringify 當 cache key 生成方式時要小心：NaN 會變成 null、undefined 會被丟掉、function 會變成 undefined、循環參照會直接拋出例外</cite>——這篇直接點出你今天遇到的 cache key 問題屬於已知的常見陷阱，不是你獨有的踩坑。

## 2. Counter 案例

- 這是最基礎的 closure 範例，多數 closure 教學文章都會用「私有變數」角度切入，例如 DEV Community 的 [JavaScript Closures: Understanding Private Variables, Callbacks, and Memoization](https://dev.to/husayn01/javascript-closures-understanding-private-variables-callbacks-and-memoization-for-efficient-code-2ph4) — 明確定義<cite index="19-1">closure 是一個能存取其父函式變數的函式，即使父函式已經執行完畢返回，只要內層函式是在外層函式裡定義的，它就保有這個存取能力</cite>，可以拿來對照你 `createCounter()` 的實作邏輯。

## 3. Memoize 案例（含你踩到的坑）

- Medium：[Debounce Function in JavaScript — Explained with Closures](https://medium.com/@a1guy/debounce-function-in-javascript-explained-with-closures-real-search-example-a8ae592d1865)：用「背包」比喻示範同一個 `outer()` 被呼叫兩次會產生兩個各自獨立的 closure（`counter1`、`counter2`），彼此的私有狀態互不干擾——這點可以延伸到 memoize：如果你的 `memoize(fn)` 被呼叫多次會不會共用同一個快取？值得補一個測試案例確認行為符合預期。
- **Cache key 問題的延伸閱讀**：你今天記錄「先用簡化版，複雜正規化留到 Month 3」是合理的決策，但如果想先了解正式解法可以看：
  - `WeakMap` 作為 object key 的 memoization 快取（上面 W3Tweaks 那篇有提到）
  - 進階函式庫作法：像 Lodash 的 `_.memoize` 允許自訂 resolver 函式來產生 cache key，這正是你「先簡化、之後再抽象化」這個決策方向的業界標準做法

## 4. Debounce 案例

- [GreatFrontEnd — Debounce (Interview Question)](https://www.greatfrontend.com/questions/javascript/debounce)：面試題形式，清楚定義了 debounce 的核心特性——<cite index="12-1">closure 在任何時刻只保存「沒有 timeout」或「恰好一個代表最新呼叫的 timeout」這個狀態，每次呼叫都不會立即執行原函式，而是重新排程在指定時間後執行</cite>，並提醒了一個你今天雛形版本可以之後補強的方向：<cite index="12-1">回傳的 wrapper 應該用一般 function 而非箭頭函式，因為箭頭函式沒有動態的 this，若用箭頭函式包裝，像 obj.debouncedMethod() 這種呼叫方式就沒辦法在延遲執行時保留正確的 this</cite>——這點你今天雛形版本可能還沒處理到，可以列進之後的技術債。
- DEV Community：[Closures: Debouncing](https://dev.to/bbarbour/closures-debouncing-3ja0)：<cite index="13-1">debounce 會延遲函式處理一段時間，特別適合表單、按鈕、滑鼠事件這類短時間內可能觸發大量輸入的情境</cite>，文章重點放在「找出 closure 藏在哪裡」，適合拿來驗證你自己的實作有沒有真的用對 closure，而不是用其他方式（如全域變數）硬做出類似效果。
- 進階（選讀，Month 3 會更深入）：[Developer Way — How to debounce and throttle in React without losing your mind](https://www.developerway.com/posts/debouncing-in-react)：講在 React 環境下用 `useMemo`/`useRef` 讓 debounce 函式不會每次 re-render 都被重新建立，這是 Month 1 Week 3-4 React 專案初始化、以及 Month 3 Performance 優化時會用到的延伸主題，現在不用深究，但可以先知道這個坑存在。

## 5. `var` 迴圈陷阱（今天筆記提到的經典面試坑）

- javascript.info 的 closure 文章結尾就有這個範例，可以直接動手驗證：
  1. 寫一個 `for (var i = 0; i < 3; i++) { setTimeout(() => console.log(i), 0) }`，觀察輸出是不是三個 `3`
  2. 改成 `let i`，觀察輸出變成 `0 1 2`
  3. 想清楚原因：`var` 是 function-scoped，整個迴圈共用同一個 `i`；`let` 是 block-scoped，每次迭代都會建立一個新的綁定，closure 抓到的是各自獨立的那份

## 6. 自我檢查（面試題自測）

1. Closure 為什麼能讓變數「活得比外層函式的執行還久」？從 Scope Chain 的角度解釋。
2. `for (var i...)` 迴圈裡的 closure 陷阱，本質原因是什麼？`let` 怎麼解決？
3. 如果你的 memoize 函式用物件當參數，`JSON.stringify` 當 cache key 會有哪些已知問題？有什麼替代方案？
4. Debounce 為什麼要用 closure 保存 `timerId`，而不是用一個全域變數？兩者的差異在哪？
5. 承接昨天：Closure 依賴的 Scope Chain 是「定義時」決定的，那 debounce 回傳的函式如果用 `obj.debouncedMethod()` 的方式呼叫，`this` 的值會怎麼決定？（提示：明天的任務主題）

---

> 下一份：`month1-week1-day4.md` 對應資源會聚焦 `this` 綁定規則與 `call`/`apply`/`bind`——今天最後一題的伏筆，正好是明天要解開的內容：Scope Chain（含 closure）在定義時就固定了，但 `this` 完全相反，是由呼叫方式決定的。
