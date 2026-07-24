# 學習資源 — Month 1 / Week 1 / Day 5：Prototype Chain / `new` / Event Loop

> 對應 `daily-logs/month1-week1-day5.md`，也是 Week 1（JavaScript Runtime）的最後一天。這份資源清單分兩塊：`new` 的底層四步驟、以及 Microtask/Macrotask 的官方權威說明。

## 1. `new` 運算子底層四步驟

- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)：官方指南，<cite index="26-1">明確說明當你執行 const a1 = new A() 時，JavaScript 會在記憶體中建立物件、且在真正執行 A() 函式本體（並把 this 綁定給它）之前，就先把 a1 的內部 [[Prototype]] 設定為 A.prototype</cite>，這證實了你今天筆記寫的步驟順序——prototype 連結是在建構函式本體執行**之前**就設好的，不是執行完才設定。
- Medium — [What is the JavaScript Prototype Chain?](https://microwind.medium.com/what-is-the-javascript-prototype-chain-understand-it-in-one-article-f83805589c22)：<cite index="23-1">用 new Foo() 具體示範四步驟：建立新物件、把新物件的 __proto__ 設為 Foo.prototype、以新物件作為 this 呼叫 Foo 建構函式本體、最後如果建構函式明確回傳一個物件就用那個回傳值，否則回傳一開始建立的新物件</cite>，跟你今天筆記記錄的四步驟完全一致，可以互相對照確認沒記錯。
- Medium — [JavaScript — The prototype chain in depth](https://medium.com/@sagiv.bengiat/javascript-the-prototype-chain-in-depth-347f10288f9f)：額外提到一個實用觀念——<cite index="24-1">命名慣例上，打算被 new 呼叫的函式通常會用大寫開頭命名（如 Player 而非 createPlayer），這是開發者之間用來提示「這是一個建構函式、應該搭配 new 使用」的慣例</cite>，可以順便記下來，之後看到大寫開頭的函式就知道大概率是建構函式。

## 2. 為什麼箭頭函式不能被 `new`（今天回答的昨日伏筆）

- [MDN — Arrow function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：官方文件明確列出箭頭函式「不能作為建構函式使用」，這是語言規格層級的限制，跟你今天筆記寫的「根本沒有 `[[Construct]]` 內部方法」一致。
- 延伸對照（選讀）：[MDN — Function.prototype.bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)（你 Day 4 資源已經看過）——bound function 之所以還能被 `new`，是因為它內部保留了對目標函式的參照，`instanceof` 判斷時讀的是目標函式的 prototype，這跟箭頭函式從一開始就沒有 `[[Construct]]` 是完全不同層級的限制。

## 3. Prototype Chain vs Scope Chain（今天筆記的釐清）

- HackerNoon — [Understanding Prototype Chain And Inheritance in JavaScript](https://hackernoon.com/understanding-prototype-chain-and-inheritance-in-javascript-5c2w31oa)：<cite index="27-1">JavaScript 的繼承機制只有一種構造——物件；每個物件都有一個私有屬性連結到另一個叫做 prototype 的物件，這個 prototype 物件自己也有 prototype，一路串下去直到抵達某個 prototype 為 null 的物件</cite>，這條鏈跟 Scope Chain（查變數用）是兩回事，但都是「找不到就往外/往上查」的機制，這正是你今天筆記裡的釐清重點。

## 4. Event Loop / Microtask vs Macrotask（官方權威資料）

- [MDN — In depth: Microtasks and the JavaScript runtime environment](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)：**今天最重要的一篇**。<cite index="32-1">每個 agent 由一個 event loop 驅動，每次迭代最多執行一個待處理的 task，接著執行所有待處理的 microtask，然後才進行必要的渲染繪製，再進入下一輪迴圈</cite>。關鍵的一段：<cite index="32-1">每當一個 task 結束、且執行堆疊清空時，microtask queue 裡的所有 microtask 會依序被執行；不同之處在於 microtask 的執行會持續到 queue 真的清空為止——即使過程中又有新的 microtask 被排入，換句話說，microtask 可以把新的 microtask 加入佇列，而這些新加入的也會在下一個 task 開始之前、當前這輪事件迴圈結束之前執行完畢</cite>。這段文字直接證實了你今天實驗第 2 題觀察到的行為：巢狀產生的新 `.then()` 確實會在同一輪被清空，不會延後到下一個 macrotask 之後——你的實測結果跟官方描述完全吻合。
- [MDN — Using microtasks in JavaScript with queueMicrotask()](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)：較簡化版的說明，<cite index="31-1">task queue 裡最舊的一個可執行 task 會在事件迴圈的一次迭代中被執行，執行完之後，microtask 會被持續執行直到 microtask queue 清空，然後瀏覽器才可能進行畫面更新，接著才進入事件迴圈的下一輪迭代</cite>，適合當作比 in-depth 那篇更快讀完的版本。
- GreatFrontEnd 面試題庫 — [Explain the concept of a microtask queue](https://github.com/greatfrontend/top-javascript-interview-questions/blob/main/questions/explain-the-concept-of-a-microtask-queue/en-US.mdx)：<cite index="36-1">microtask queue 會在目前執行中的 script 結束之後、且在 macrotask queue 裡任何其他 task 之前被處理，這代表 microtask 的優先權高於 macrotask</cite>，並列出 `Promise.resolve().then()`、`queueMicrotask()`、`MutationObserver` 都是產生 microtask 的來源——你今天只測了 Promise，之後有機會可以順手也測一下 `queueMicrotask()` 的行為。

## 5. 你今天發現的「microtask 餓死 macrotask」現象的延伸閱讀

- 2026 年的實務文章 — [JavaScript Event Loop Explained (With Examples) 2026](https://crosscheck.cloud/blogs/javascript-event-loop-explained/)：<cite index="35-1">JS Event Loop 是讓單執行緒的 JavaScript 能處理非同步工作而不阻塞的執行機制，它會依照嚴格且可重複的順序，在呼叫堆疊、Web API、macrotask queue、microtask queue 這四個角色之間搬移 callback</cite>，文章用實際會印出結果的範例逐步拆解，適合拿來驗證你自己寫的三題順序題有沒有推論正確。

## 6. 動手驗證（延伸練習，選做）

今天的第 2 題已經驗證了「巢狀 microtask 會在同一輪被清空」，可以再加一題把這個現象推到極端，實際感受「microtask 餓死 macrotask」：

```js
console.log("start");

setTimeout(() => console.log("macrotask: setTimeout"), 0);

let count = 0;
function recursiveMicrotask() {
  count++;
  if (count < 5) {
    Promise.resolve().then(recursiveMicrotask);
    console.log("microtask #" + count);
  }
}
Promise.resolve().then(recursiveMicrotask);

console.log("end");
```

跑跑看，觀察 `"macrotask: setTimeout"` 是不是要等所有 `"microtask #N"` 都印完才出現。如果把 `count < 5` 改成沒有上限的遞迴，就會實際重現「畫面卡住不渲染」的情境（不建議真的跑沒有終止條件的版本，會讓分頁卡死）。

## 7. Week 1 收尾：自我檢查（面試題自測）

用這幾題檢查這週七大主題有沒有真的串起來，答不出來的回頭補進 `JavaScript Runtime Notes.md`：

1. Execution Context、Call Stack、Scope Chain 這三者的建立時機分別是什麼？彼此的關係是什麼？
2. Closure 為什麼能讓變數活得比外層函式的執行還久？從 Lexical Environment 的角度解釋。
3. `this` 的四種綁定規則的優先順序是什麼？箭頭函式為什麼是特例？
4. `new` 底層四步驟是什麼？為什麼箭頭函式無法被 `new`？
5. Microtask 跟 macrotask 的優先順序規則是什麼？為什麼即使 `setTimeout(fn, 0)` 延遲是 0，還是會排在所有 microtask 之後？
6. 如果要你用一張圖，把這七個主題的關聯畫出來，你會怎麼畫？（這題留給週五驗收時，直接產出一張圖放進 `JavaScript Runtime Notes.md`）

---

> Week 1 到此結束。下週進入 Week 2：Async 與工程能力，第一天主題是 Promise 生命週期，會直接延續今天 Event Loop 的基礎繼續往下深挖。
