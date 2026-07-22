# Month 1 / Week 1 / Day 4 — 2026-07-23

## 今日任務

- 暖身任務 [5min]：`this` 綁定規則速記卡
- 核心任務 [30min]：call/apply/bind 範例 + 自己實作 `myBind()`

## 今日目標

驗證昨天留下的對比：Scope Chain（含 Closure）是函式「定義時」就決定、不受呼叫方式影響；`this` 則完全相反，是由「怎麼被呼叫」決定，跟函式寫在哪裡無關。用 call/apply/bind 三個方法實際操控 `this`，並自己刻一個 `myBind()` 證明真的理解 bind 的底層行為。

## 完成定義

- [x] `this` 綁定規則速記卡完成，四種綁定規則（預設/隱式/顯式/new）各有一句話說明 + 一個例子
- [x] call/apply/bind 範例可執行，能證明三者都能改變 `this` 但傳參方式不同
- [x] `myBind()` 實作完成，行為與原生 `Function.prototype.bind` 一致（含之後可以再 `new` 出來的邊界情況，先記錄但不強求今天做完）
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`

## 執行紀錄

### 暖身任務

- 花費時間：5 分鐘
- 筆記重點（四種 `this` 綁定規則）：
  1. **預設綁定**：一般函式呼叫（非嚴格模式下 `this` 指向全域物件，嚴格模式下是 `undefined`）。
  2. **隱式綁定**：以 `obj.method()` 形式呼叫，`this` 指向呼叫時前面的物件 `obj`。
  3. **顯式綁定**：用 `call`/`apply`/`bind` 明確指定 `this` 要綁定的對象。
  4. **`new` 綁定**：用 `new Fn()` 呼叫建構函式，`this` 指向新建立的物件。
  5. 箭頭函式是特例，它沒有自己的 `this`，會直接沿用外層（詞法上）作用域的 `this`——這點跟 Scope Chain 的行為反而是一致的，因為箭頭函式的 `this` 本質上是「借用」外層的，不是自己重新綁定。

### 核心任務

- 花費時間：30 分鐘
- 實作摘要：
  1. 寫了同一個函式 `introduce()`，分別用一般呼叫、`obj.introduce()`、`introduce.call(obj)`、`introduce.apply(obj)`、`introduce.bind(obj)()` 五種方式呼叫，逐一印出 `this` 指向誰，驗證四種綁定規則。
  2. 確認 `call` 跟 `apply` 的差異只在傳參方式：`call(thisArg, arg1, arg2, ...)` 逐一列出參數；`apply(thisArg, [arg1, arg2, ...])` 用陣列包起來傳。
  3. 實作 `myBind()`：核心邏輯是回傳一個新函式，這個新函式內部呼叫原函式的 `apply`，把 `this` 固定綁定成 `myBind` 呼叫時傳入的 `thisArg`，並支援 partial application（提前綁定部分參數，之後呼叫時可以再補剩下的參數）。
- 遇到的問題：一開始 `myBind()` 用箭頭函式包裝回傳的新函式，結果原生 `bind()` 支援的「用 `new` 呼叫 bind 後的函式」這個邊界情況完全壞掉——因為箭頭函式沒有自己的 `this`，`new` 也無法正確綁定新物件。
- 解法：改用一般 `function` 宣告回傳的包裝函式，並在內部判斷「這次呼叫是不是透過 `new`」（可以用 `this instanceof boundFn` 簡易判斷），如果是就不強制綁定 `thisArg`、改用新建立的物件當 `this`。這個邊界情況比想像中複雜，今天先讓基本情境（一般呼叫 + partial application）正確，`new` 綁定的完整處理先記錄下來但不追求今天做到完美。

## Commit

```
feat(js-runtime): implement this-binding demo (call/apply/bind) and myBind()
```

## 今天學到的三件事

1. `this` 是「呼叫時」動態決定的，跟函式「定義在哪裡」完全無關——這跟昨天確認的 Scope Chain（定義時決定、不受呼叫方式影響）正好是兩種相反的機制，同一份程式碼裡，變數查找是靜態的，`this` 卻是動態的。
2. 箭頭函式的 `this` 其實不是「沒有規則」，而是直接繼承外層作用域的 `this`——這讓箭頭函式的 `this` 行為看起來反而更像 Scope Chain（沿著詞法位置找），跟一般函式的動態綁定不同。
3. 自己刻 `myBind()` 才發現：回傳的包裝函式如果用箭頭函式寫，會直接喪失「被 `new` 呼叫」的能力，因為箭頭函式沒有自己的 `this` 可以被 `new` 綁定新物件。這是原生 `bind()` 規格裡容易被忽略但很關鍵的一個邊界情況。

## 面試可能會問的一個問題

Q: `call`、`apply`、`bind` 有什麼差異？`bind` 內部大致是怎麼實作的？
A: 三者都能明確指定函式執行時的 `this`；差別在於 `call`/`apply` 會立即執行函式，只是傳參方式不同（`call` 逐一列出參數、`apply` 用陣列包參數），而 `bind` 不會立即執行，是回傳一個新函式，這個新函式內部固定了 `this` 綁定（也可以固定部分參數，也就是 partial application），要等之後被呼叫時才真正執行。實作上 `bind` 的核心就是回傳一個 wrapper function，內部用 `apply` 呼叫原函式並帶入固定的 `thisArg`；但要注意這個 wrapper 不能用箭頭函式，否則會喪失原生 `bind()` 支援的「用 `new` 呼叫已綁定函式」這個邊界能力。

## 明日待辦

- Prototype Chain + `new` 運作原理筆記（明天暖身任務）
- 核心任務會做 Event Loop / Microtask vs Macrotask 實驗程式；今天實作 `myBind()` 時已經先摸到「`new` 呼叫時 `this` 怎麼被綁定」這件事的邊界，明天可以順勢把 `new` 底層四個步驟（建立新物件 → 設定 prototype 鏈 → 綁定 this 執行建構函式 → 回傳）徹底搞懂，剛好補完今天沒深究完的部分。
