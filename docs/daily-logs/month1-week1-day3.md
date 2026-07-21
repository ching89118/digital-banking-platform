# Month 1 / Week 1 / Day 3 — 2026-07-22

## 今日任務

- 暖身任務 [10min]：Closure 觀念 flashcard
- 核心任務 [60min]：Closure 實作 + 3 個應用案例（counter / memoize / debounce 雛形）

## 今日目標

回答昨天留下的問題——外層函式執行完畢後，內層函式為什麼還能繼續存取外層變數，搞懂 Closure 就是「函式 + 它建立當下所綁定的 Scope Chain」這個組合，並用 3 個實際案例證明這個組合真的能讓變數「活得比外層函式的執行還久」。

## 完成定義

- [x] Closure flashcard 完成，能用一句話講清楚 Closure 的定義
- [x] `createCounter()`、`memoize(fn)`、debounce 雛形三個函式可執行，並各自有測試案例驗證
- [x] 已 commit
- [x] 筆記寫入 `JavaScript Runtime Notes.md`

## 執行紀錄

### 暖身任務

- 花費時間：10 分鐘
- 筆記重點：
  1. Closure = 函式 + 它建立當下所綁定的詞法環境（Lexical Environment）。這個環境在外層函式執行完後，理論上該被回收，但只要還有內層函式參照著它，垃圾回收機制就不會清掉，變數因此「活」得比外層函式的執行還久。
  2. 回答昨天的問題：Scope Chain 在函式「定義時」就決定了，這個決定本身不會因為外層函式執行完畢而失效——閉包只是把這個已經決定好的 Scope Chain 保留下來，讓內層函式繼續能用。
  3. 常見誤區：Closure 抓住的是變數的「參照」，不是當下的「值」。在 `for (var i ...)` 這種舊寫法裡，所有 closure 共享同一個 `i`，迴圈結束後全部指向同一個最終值，這是經典面試坑。

### 核心任務

- 花費時間：55 分鐘
- 實作摘要：
  1. `createCounter()`：回傳一個內部函式，每次呼叫都會遞增並回傳外層作用域裡的 `count` 變數，驗證外層變數狀態能跨呼叫存活。
  2. `memoize(fn)`：用 closure 保存一個快取物件，重複呼叫相同參數時直接回傳快取結果，不重新執行 `fn`。
  3. debounce 雛形：用 closure 保存 `timerId`，每次呼叫都清掉前一個計時器並重新設定，只有最後一次呼叫的計時器會真正執行。
- 遇到的問題：`memoize` 的 cache key 用 `JSON.stringify(args)` 在物件參數的屬性順序不同時會產生不同字串，導致明明是同樣的物件卻沒命中快取。
- 解法：先寫一個只支援基本型別參數（number/string）的簡化版，並在筆記裡明確記下這個限制，複雜的 cache key 正規化留到 Month 3 效能優化時再處理，不在今天卡關。

## Commit

```
feat(js-runtime): implement closure examples (counter, memoize, debounce)
```

## 今天學到的三件事

1. Closure 保留的是變數的「參照」而不是「值的快照」，這也是 `for (var i...)` 迴圈裡最常出錯的原因。
2. Closure 之所以成立，是因為 Scope Chain 在函式定義時就已經決定好了，只是被閉包「保留」下來，不會因為外層函式執行完就失效——這正好接上昨天 Scope Chain 是 lexical（定義時決定）而非呼叫時決定的結論。
3. Memoize 的 cache key 設計本身就是一個工程取捨題：越通用的 key 產生方式（如 deep serialize）成本越高，簡單場景用簡化版反而更務實。

## 面試可能會問的一個問題

Q: 什麼是 Closure？請舉一個實際應用場景，並說明它為什麼能運作。
A: Closure 是函式建立時，連同它當下所在的詞法作用域一起被「記住」的機制；即使外層函式已經執行完畢，只要內層函式還存在，這個作用域鏈就不會被回收。實務上我用它做過 debounce（用 closure 保存 timer id，讓它跨多次呼叫存活）跟簡單的 memoization cache（用 closure 保存一個跨呼叫共享的快取物件）。它能運作的關鍵在於：JavaScript 的 Scope Chain 是在函式「定義時」就決定的，不會因為外層函式執行結束而失效，closure 只是把這條已經決定好的鏈保留下來繼續使用。

## 明日待辦

- `this` 綁定規則速記卡（明天暖身任務）
- 核心任務會做 call/apply/bind 範例 + 自己實作 `myBind()`；今天已經確認「函式定義時的 Scope Chain 不受呼叫方式影響」，明天要對照的是 `this` 剛好相反——`this` 是由「怎麼被呼叫」決定，不是「在哪裡定義」，這個對比會是很好的釐清素材。
