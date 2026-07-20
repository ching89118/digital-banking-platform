# 學習資源 — Month 1 / Week 1 / Day 2：Call Stack & Scope Chain

> 對應 `daily-logs/month1-week1-day2.md`。今天最容易混淆的點是「Call Stack 跟 Scope Chain 是不是同一件事」——你的 daily log 裡已經抓到這個坑，這份資源清單順著同一條主軸整理。

## 1. Call Stack

- [MDN — Call stack](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)：官方定義，用 `greeting()` 呼叫 `sayHi()` 的例子逐步示範 push/pop 過程，跟你今天筆記裡「global → bar → foo 疊三層」的驗證方式幾乎一樣，可以直接對照。
- [互動視覺化：Loupe](http://latentflip.com/loupe/)：你昨天已經用過，今天可以再貼一次 3 層巢狀函式的程式碼，實際看 stack 疊起來又一層層 pop 掉的動畫，比純看文字更容易記住 LIFO 的行為。
- [MDN — JavaScript execution model：Stack and execution contexts](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model#stack_and_execution_contexts)：上次給你的那篇文章裡專門講 stack 的段落，用 `foo`/`bar`/`baz` 的例子逐幀拆解，也有提到 Stack Overflow 的成因（遞迴沒有終止條件）跟今天筆記重點一致。

## 2. Scope Chain / Lexical Scoping

- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)：正式定義了 lexical scoping，並用具體範例圖解「函式存取變數時往外層 scope 查找」的過程，是官方對這個機制最完整的說明。
- [javascript.info — Variable scope, closure](https://javascript.info/closure)：延續你 Day 1 已經看過的這篇，今天重看時可以專注在「Lexical Environment 鏈狀連結」的圖解部分，這正是 Scope Chain 的視覺化呈現。
- Medium：[The Scope Chain, Scope & Lexical Environment](https://medium.com/@razputshivanshu/the-scope-chain-scope-lexical-environment-js-interview-prep-article-5-00adf8da6491) — 明確指出<cite index="19-1">JavaScript 執行時會從區域到全域建立一條 execution context 的鏈，這條鏈就是 scope chain，讓內層函式能存取外層函式或全域變數</cite>，並說明<cite index="19-1">當引擎在目前 execution context 裡找不到變數時，會往上層 parent execution context 查找，直到全域 execution context，若整條 scope chain 都找不到就會拋出 ReferenceError</cite>。這篇的查找流程描述剛好可以驗證你今天實作的 3 層巢狀函式範例。
- Medium（Rahul Kumar）：[Understanding Scope, Lexical Environment, and the Scope Chain](https://rahul319sinha.medium.com/understanding-scope-lexical-environment-and-the-scope-chain-in-javascript-c219cbb73417) — 條列式總結四個重點：<cite index="24-1">Lexical Environment 由函式的區域記憶體加上一個指向父層環境的參照組成；Scope Chain 決定變數解析順序——由內而外、直到全域；內層函式可以存取外層函式的變數，但外層無法反過來存取內層變數；scope 是在函式「定義時」決定的，不是在「呼叫時」決定</cite>。這四點正好是你今天核心任務要驗證的全部內容，可以當作自我檢查表。

## 3. 「函式在 A 定義、在 B 呼叫」的經典驗證題

今天你額外測試了這個情境，這是理解 lexical scoping 最關鍵的一步。可以參考：
- [freeCodeCamp — Lexical Scope in JavaScript](https://www.freecodecamp.org/news/javascript-lexical-scope-tutorial/)：從「lexical」這個字本身的意思切入解釋，適合拿來跟同事/面試官解釋「為什麼叫做詞法作用域」時使用的說法。
- [Dasha AI — What is Scope and Scope Chain in JavaScript?](https://dasha.ai/blog/javascript-scope-and-scope-chain)：特別提到<cite index="21-1">Scope Chain 只會往上層查找，不會橫向查找兄弟作用域（sibling scope），即使兩個函式都是同一個父函式的子作用域，彼此也無法直接存取對方的變數</cite>。這是一個很多人沒測過的邊界案例，建議明天（或這週找個空檔）補一個「兩個 sibling 函式互相存取變數」的測試，會拿到 ReferenceError，正好可以強化今天學到的「Scope Chain 只往外、不橫向」這個結論。

## 4. 動手驗證工具

| 工具 | 用途 |
|---|---|
| [Loupe](http://latentflip.com/loupe/) | 視覺化 Call Stack push/pop |
| Chrome DevTools → Sources → Scope 面板 | 打中斷點後，直接展開 Scope 面板看 Local / Closure / Global 三層變數，比自己想像更直觀 |

## 5. 自我檢查（面試題自測）

1. Call Stack 跟 Scope Chain 分別解決了什麼問題？兩者的建立時機一樣嗎？
2. 為什麼一個函式呼叫堆疊很深，不代表它能存取呼叫鏈上任何一層的區域變數？
3. Sibling 函式（同一個父函式底下的兩個子函式）可以互相存取彼此的變數嗎？為什麼？
4. 「函式在 A 處定義、在 B 處被呼叫」時，它的 Scope Chain 是根據 A 還是 B 決定的？這跟 `this` 的綁定規則（由呼叫方式決定）有什麼不同？

---

> 下一份：`month1-week1-day3.md` 對應的資源清單會聚焦 Closure——今天驗證的 lexical scoping 正是 Closure 能成立的前提，「明日待辦」裡留的那個問題（外層函式執行完了，內層函式為什麼還能存取外層變數）會在那份資源裡有答案。
