# 學習資源 — Month 1 / Week 1：JavaScript Runtime

> 對應 `daily-logs/month1-week1-day1.md` 及本週後續主題（Call Stack / Scope Chain / Closure / this / Prototype / Event Loop）。
> 原則：先看一篇「建立心智模型」的文章或影片，再用 Loupe / DevTools 動手驗證，最後用面試題自我檢查。

## 1. Execution Context（今天的核心任務）

**必看（建立心智模型）**
- [MDN — JavaScript execution model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model)：官方參考文件，說明 engine 與 host environment 如何協作執行程式碼，並用具體的 call frame 例子示範 call stack 運作，比詞彙表定義更完整（原先連結的 `/Glossary/Execution_context` 頁面已失效，改用這篇）。
- [javascript.info — Variable scope, closure](https://javascript.inf    o/closure)：雖然標題是 closure，但開頭完整講解了 Lexical Environment 的組成，是 Execution Context 概念的基礎。

**進階/整合視角**
- DEV Community：[How JavaScript Really Executes Code](https://dev.to/malloc72p/how-javascript-really-executes-code-execution-context-and-scope-chain-explained-4c9f) — 把 Execution Context、Scope Chain、`outerEnvironmentReference` 串起來講，<cite index="4-1">解釋了每次函式呼叫都會建立新的執行環境並推入呼叫堆疊，執行完再彈出</cite>，並強調外層環境參照指向函式「宣告時」的位置而非「呼叫時」的位置——這點正好對應你今天筆記裡容易搞混 scope 跟 EC 的地方。
- Medium：[Mastering Execution Context in JavaScript: Q&A Guide](https://medium.com/@aansh0611/mastering-execution-context-in-javascript-the-ultimate-q-a-guide-9f07d6abeecf) — Q&A 形式，適合當作面試題庫，裡面明確點出<cite index="3-1">this 的值是依函式被呼叫的方式決定，而不是定義的位置</cite>，可以直接拿來對照你今天的筆記。

**規格層級（進階，選讀）**
- [ECMA-262 規格 — Execution Contexts](https://tc39.es/ecma262/#sec-execution-contexts)：官方規格用詞（Binding Instantiation、Lexical Environment Creation）跟一般教學文章不完全一樣，等你對概念熟了再回來看能對上號，會有「原來如此」的感覺。

## 2. Call Stack（明天 Day 2 的主題，可提前預習）

- [MDN — Call stack](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)：官方定義 + LIFO（後進先出）行為說明。
- **互動視覺化工具（強烈推薦動手玩）**：[Loupe by Philip Roberts](http://latentflip.com/loupe/) — 貼上你自己的程式碼，即時看到 Call Stack、Web APIs、Callback Queue 的變化過程，比看文字說明直觀十倍。
- 影片：Philip Roberts — *"What the heck is the event loop anyway?"*（JSConf 經典演講）— 雖然主題是 event loop（本週 Day 5 會碰到），但前半段對 Call Stack 的講解非常清楚，可以先看到那段就好。

## 3. Scope Chain / Lexical Environment

- [javascript.info — Variable scope, closure](https://javascript.info/closure)：同一篇文章，這次專注看 Lexical Environment 的圖解部分。
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)：官方對 scope chain 如何支撐 closure 的說明，是你 Day 3 任務的預告讀物。

## 4. `this` Binding

- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)：四種綁定規則（預設綁定、隱式綁定、顯式綁定、new 綁定）+ 箭頭函式的例外，官方文件寫得最完整。
- Kyle Simpson《You Don't Know JS: this & Object Prototypes》（免費線上書，GitHub 開源）：業界公認講 `this` 講得最透徹的資源，適合這週後段（Day 4 call/apply/bind）搭配讀。

## 5. Prototype Chain / `new`（Day 5 主題，可提前預習）

- [MDN — Object prototypes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Object_prototypes)
- [MDN — new operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)：逐步拆解 `new` 做的四件事，適合搭配你自己動手實作一個 `myNew()`。

## 6. 動手驗證工具

| 工具 | 用途 |
|---|---|
| [Loupe](http://latentflip.com/loupe/) | 視覺化 Call Stack + Event Loop |
| Chrome DevTools → Sources → Call Stack 面板 | 打中斷點後直接看真實的 call stack |
| [TC39 Playground](https://tc39.es/ecma262/) + `node --stack-trace-limit` | 進階：想深入看 stack trace 細節時用 |

## 7. 書籍（不用馬上讀完，當作本月的參考書）

- Kyle Simpson — *You Don't Know JS (Yet)* 系列，尤其 **Scope & Closures** 這本，跟你 Week 1 的主題完全對應
- David Flanagan — *JavaScript: The Definitive Guide*（查閱型，不用通讀）

## 8. 自我檢查（面試題自測）

用這幾題檢查今天有沒有真的搞懂，答不出來就代表要回頭補：

1. 執行一段程式碼時，Execution Context 跟 Call Stack 分別扮演什麼角色？兩者的關係是什麼？
2. Global Execution Context 跟 Function Execution Context 建立時分別做了哪些事？
3. `this` 在四種呼叫方式下（一般呼叫、方法呼叫、`call`/`apply`/`bind`、`new`）分別綁定到誰？
4. 為什麼箭頭函式沒有自己的 `this`？它怎麼決定 `this` 的值？
5. Scope 是在程式碼「寫的時候」還是「執行的時候」決定的？這跟 Execution Context 在執行時才建立，有什麼關係？

---

> 之後每天可以照這個格式，在 `docs/learning-resources/` 底下新增對應檔案，例如 `month1-week2-async.md`，逐週累積成一份完整的個人學習資料庫。
