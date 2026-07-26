# Month 1 / Week 1 驗收 — 2026-07-24（JavaScript Runtime）

## 本週任務完成度

- 應完成任務數：10（Day 1-5，每天暖身 + 核心各 1 個）
- 實際完成任務數：10
- 未完成任務與原因：無，但 Day 3 跟 Day 5 的核心任務都超出原訂範圍：
  - Day 3 額外多做了一題 `inBetween(a, b)` closure 應用練習（filter 搭配 closure 產生客製化判斷函式），是計畫外的第 4 個 closure 案例
  - Day 5 把 Event Loop 實驗程式從「1 題基本順序題」擴充成「3 題遞增難度」，多驗證了巢狀 microtask 跟 run-to-completion 兩個延伸情境

## 產出檢查

- [x] `JavaScript Runtime Notes.md` 已產出，涵蓋 Execution Context、Call Stack、Scope Chain、Closure、this/call/apply/bind、Prototype/new、Event Loop 七大主題
- [x] 程式碼可從乾淨環境跑起來（目前都是可獨立貼進 Chrome DevTools Console 執行的 demo，未依賴專案環境）
- [x] 本週所有 commit 訊息符合 `git-commit-convention.md` 規範（`feat(js-runtime): ...` 格式）
- [x] 至少一項本週成果可以「口頭講 60 秒」給別人聽懂

## Demo-able 檢查（能不能給人看）

> 我可以打開 Chrome DevTools Console，現場示範：① 用 Loupe 視覺化一段 3 層巢狀函式呼叫的 Call Stack push/pop 過程；② 貼上 `myBind()` 的兩個版本（一般函式 vs 箭頭函式包裝），現場觸發 `new BoundV2()` 拋出 `TypeError`，解釋箭頭函式為什麼沒有 `[[Construct]]`；③ 貼上 microtask/macrotask 順序題，讓對方先猜輸出順序，再實際執行驗證 microtask 永遠優先於 macrotask。

## 技術債 / 待補清單

- [ ] `memoize(fn)` 的 cache key 目前只支援基本型別參數（`JSON.stringify` 版本），物件參數的正規化留到 Month 3 效能優化時處理
- [ ] `myBind()` 的 `new` 綁定邊界情況（用 `this instanceof boundFn` 簡易判斷）只做了基本版本，沒有處理原生 `bind()` 完整規格的所有邊界（如多層繼承的 prototype chain）
- [ ] debounce 雛形版本尚未驗證 `obj.debouncedMethod()` 這種依賴動態 `this` 的呼叫方式（Day 3 資源清單裡有提到這個技術債，還沒回頭補測試）
- [ ] Week 1 筆記目前分散在各天的 daily-log 裡，`JavaScript Runtime Notes.md` 需要重新整理成一份可獨立閱讀（不依賴 daily-log 上下文）的技術文件

## 本週最大的收穫（技術面）

Scope Chain（含 Closure）是「定義時」決定、`this` 是「呼叫時」決定——這個對比貫穿了整週：Day 2 確認 Scope Chain 由詞法位置決定，Day 3 用 Closure 證明這個決定會被保留下來跨呼叫存活，Day 4 再用 `this` 的動態綁定跟它對照，Day 5 用「箭頭函式沒有 `[[Construct]]`」把兩條線收在一起解釋——七個看似獨立的主題，其實是同一組底層機制（Execution Context 的組成）從不同切面展開。

## 本週最大的收穫（工程習慣/方法論）

「先求基本情境正確，複雜邊界情況記錄下來但不強求當下做完」這個決策模式在這週出現了兩次（Day 3 的 memoize cache key、Day 4 的 `myBind()` new 綁定），後來查資料證實連 MDN 官方都建議 polyfill 大多數情況不用強求完整支援邊界情況——這說明「先讓核心情境正確、明確記錄限制範圍」是業界普遍認可的務實做法，不是偷懶。

## 下週風險評估

- 下週（Week 2：Async 與工程能力）第一天就是 Promise 生命週期，Day 6 已經提前完成並驗證兩者排程行為跟 Week 1 的 Event Loop 規則一致，銜接順暢，沒有風險
- 需要提前預習的概念：`Promise.allSettled`（Day 7 會用到，跟單純 `try/catch` 在「多個非同步操作、部分失敗」情境下的處理方式比較）
