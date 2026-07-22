# 學習資源 — Month 1 / Week 1 / Day 4：`this` 綁定 & call/apply/bind

> 對應 `daily-logs/month1-week1-day4.md`。今天最有價值的收穫是自己刻 `myBind()` 時發現的「箭頭函式包裝會讓 `new` 綁定失效」，這份清單特別把這個邊界情況的官方依據放在最前面。

## 1. `this` 綁定規則（四種 + 箭頭函式特例）

- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)：官方文件，完整列出預設/隱式/顯式/`new` 四種綁定規則，也有專門章節講箭頭函式「沒有自己的 `this`，直接沿用外層作用域」，跟你今天筆記的第 5 點完全對應。
- Kyle Simpson《You Don't Know JS: this & Object Prototypes》（免費線上書）：業界公認把 `this` 判斷順序講得最系統化的資源，提供一套「呼叫點分析」的判斷流程，之後遇到複雜的呼叫情境（如 callback 裡的 `this`）可以直接套用。

## 2. call / apply 的差異與 Polyfill

- [GeeksforGeeks — Explain call(), apply(), and bind() methods](https://www.geeksforgeeks.org/javascript/explain-call-apply-and-bind-methods-in-javascript/)：<cite index="7-1">bind() 方法會回傳一個新函式，這個新函式的 this 被永久設定為第一個參數指定的值，額外傳入的參數會在新函式實際執行時被預先帶入，且原函式本身不會被修改</cite>，並附上完整的 `call`/`apply`/`bind` 三個 polyfill 實作，可以直接拿來跟你自己寫的 `myBind()` 對照。
- Medium — [Understanding JS call, apply, bind and their polyfill](https://fejw.medium.com/understanding-js-call-apply-bind-and-their-polyfill-8b593799a79b)：三個方法的 polyfill 各自獨立講解，call 的 polyfill 用 `Object.create` 建立暫時的物件連結來借用 `this`，這個技巧跟你自己想到的做法可以互相對照。

## 3. 自己刻 `myBind()`（今天的核心任務）

- [DEV Community — Creating your own bind() (Polyfill of bind)](https://dev.to/uddeshjain/creating-your-own-bind-polyfill-of-bind-433j)：從最簡單的版本開始，逐步加上「支援額外參數」「支援 partial application」，很適合對照你今天的實作過程（先求對，再補進階功能）。
- [FrontendHire — Polyfill: Function.prototype.bind() Method](https://frontendhire.com/questions/javascript/coding/js-coding-polyfill-function-bind/)：用 TDD（先寫測試再實作）的方式引導寫 `myBind`，並明確提醒<cite index="13-1">實際的原生 bind 方法還有處理 new 運算子相關的額外複雜度，但對大多數實務場景跟面試情境來說，一般簡化版已經足夠</cite>——這句話印證了你今天「先求基本情境正確，new 綁定留待之後完善」的決策是業界普遍做法，不是偷懶。

## 4. 「箭頭函式導致 `new` 失效」這個坑的官方依據（今天最重要的發現）

- [MDN — Function.prototype.bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)：官方文件明確寫出<cite index="16-1">被 bind 過的函式天生就能配合 new 運算子去建構新的實例，此時傳入 bind 的 this 值會被忽略，但預先綁定的參數仍然會被帶入建構呼叫</cite>，並且<cite index="16-1">當 bound function 被當作 instanceof 右側運算元使用時，instanceof 會去讀取內部儲存的目標函式（target function）的 prototype，而不是 bound function 自己的</cite>。這解釋了為什麼原生 `bind()` 能在 `new` 情境下正確運作，而箭頭函式包裝完全做不到——箭頭函式從語言規格上就沒有 `[[Construct]]` 這個內部方法，天生就不能被 `new`。
- 同一頁 MDN 文件也附了警語：<cite index="12-1">下方展示的兩種 bind polyfill 寫法，一種比較小巧高效但不支援 new 運算子；另一種比較笨重但能支援部分 new 運算子的使用情境，官方建議大多數程式碼裡很少真的用 new 呼叫一個 bound function，所以通常選第一種簡化版就好</cite>。這句話直接呼應你今天筆記寫的「這個邊界情況比想像中複雜，先求基本情境正確」的決策——連 MDN 官方都建議大多數情況不用強求完整支援 `new`。

## 5. 動手驗證

用 Chrome DevTools Console 實際驗證今天發現的坑：

```js
// 一般函式包裝：可以被 new
function myBindV1(fn, thisArg) {
  return function (...args) {
    return fn.apply(thisArg, args);
  };
}

// 箭頭函式包裝：無法被 new
function myBindV2(fn, thisArg) {
  return (...args) => fn.apply(thisArg, args);
}

function Person(name) { this.name = name; }

const BoundV1 = myBindV1(Person, null);
const p1 = new BoundV1("Alice"); // 正常運作
console.log(p1 instanceof Person); // true（視實作而定）

const BoundV2 = myBindV2(Person, null);
const p2 = new BoundV2("Bob"); // TypeError: BoundV2 is not a constructor
```

跑一次上面這段，親眼看到 `TypeError`，會比看文章更記得住這個坑。

## 6. 自我檢查（面試題自測）

1. 四種 `this` 綁定規則的優先順序是什麼？如果一個函式同時符合隱式綁定跟顯式綁定的條件，誰贏？
2. 為什麼箭頭函式不能被 `new`？這跟它沒有自己的 `this` 有什麼關係？
3. `call` 跟 `apply` 除了傳參方式，還有其他差異嗎？（提示：想想什麼情境下你會選 `apply` 而不是 `call`，例如參數數量不固定時）
4. 如果 `bind()` 回傳的函式本身沒有實作 `[[Construct]]`，那原生 `bind()` 是怎麼讓被綁定的函式還能用 `new` 呼叫的？（提示：跟目標函式的 prototype chain 有關）
5. Partial application（提前綁定部分參數）跟 currying 有什麼不同？

---

> 下一份：`month1-week1-day5.md` 對應資源會聚焦 Prototype Chain 與 `new` 運作原理——今天發現「箭頭函式不能被 new」這個坑，正是因為不了解 `new` 底層四個步驟，明天會徹底補完這塊。
