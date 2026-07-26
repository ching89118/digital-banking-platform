# ROADMAP — 240 個微任務

規則：
- 每天 2 個任務：**暖身任務**（5 或 10 分鐘，概念/筆記/比較表）＋ **核心任務**（30 或 60 分鐘，實作/整合）。
- 每週 5 天 = 10 個任務；每月 4 週 = 40 個任務；6 個月 = **240 個任務**。
- 每個任務結束都要 commit（見 `git-commit-convention.md`），每週五對照 `weekly-review-template.md` 驗收。
- 標記說明：`[5]` `[10]` `[30]` `[60]` = 建議分鐘數；📄 = 產出技術文件。

---

## Month 1 — JavaScript / React 基礎 + 專案初始化

### Week 1（Day 1-5）JavaScript Runtime
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 1 | `[5]` 建立 Git Repository + README Template | `[30]` JavaScript Execution Context 概念 + 撰寫 demo code |
| 2 | `[10]` Call Stack 圖解筆記 | `[30]` Scope Chain 實作範例（3 層巢狀函式） |
| 3 | `[10]` Closure 觀念 flashcard | `[60]` Closure 實作 + 3 個應用案例（counter / memoize / debounce 雛形） |
| 4 | `[5]` `this` 綁定規則速記卡 | `[30]` call/apply/bind 範例 + 自己實作 `myBind()` |
| 5 | `[10]` Prototype Chain + `new` 運作原理筆記 | `[30]` Event Loop / Microtask vs Macrotask 實驗程式（console.log 順序題） |

📄 產出：`JavaScript Runtime Notes.md`

### Week 2（Day 6-10）Async 與工程能力
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 6 | `[5]` Promise 生命週期筆記 | `[30]` Promise Chain 改寫成 async/await 練習 |
| 7 | `[10]` Promise.all vs Promise.race 比較表 | `[30]` 非同步 Error Handling 模式（try/catch/finally、Promise.allSettled） |
| 8 | `[10]` debounce/throttle 概念卡 | `[60]` 實作 `debounce()` + `throttle()` + 單元測試 |
| 9 | `[10]` deepClone 方法比較（JSON / structuredClone / 遞迴） | `[30]` 實作 `deepClone()` + `EventEmitter` class |
| 10 | `[10]` ESM vs CommonJS 筆記 | `[30]` 設定 `package.json` + ESLint + Prettier 規則 |

📄 產出：`Frontend Engineering Setup.md`

### Week 3（Day 11-15）React 專案初始化 I
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 11 | `[10]` Vite + React + TS 建立筆記 | `[30]` 建立專案 + Folder Architecture 設計（feature-based） |
| 12 | `[10]` Component / Props / State 設計原則筆記 | `[30]` 建立 Layout Component（Header / Sidebar / Footer） |
| 13 | `[10]` React Router 設定筆記 | `[30]` 設定多頁面路由（Dashboard / Transactions / Login） |
| 14 | `[10]` Tailwind 安裝設定筆記 | `[30]` 建立 Reusable Component（Button / Card / Table） |
| 15 | `[10]` Git Flow 分支策略筆記 | `[30]` 實作 ErrorBoundary + Environment Config（.env） |

### Week 4（Day 16-20）React 專案初始化 II + Banking UI Prototype
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 16 | `[10]` 銀行 UI Wireframe 草圖 | `[30]` 建立 Dashboard Layout 頁面 |
| 17 | `[10]` Transaction List UI 元件規劃 | `[60]` 實作 Transaction List UI（假資料） |
| 18 | `[10]` Login/Auth UI 規劃 | `[30]` 實作 Login 表單 UI + 表單驗證 |
| 19 | `[10]` Responsive 設計檢查清單 | `[30]` RWD 調整（Mobile/Desktop breakpoints） |
| 20 | `[10]` Month 1 回顧 + 文件大綱 | `[60]` 整合 Prototype + 完成 `Frontend Engineering Setup.md` + Demo |

**Month 1 成果**：`digital-banking-platform/frontend`，可運行的 Banking UI Prototype。

---

## Month 2 — Digital Banking Core Platform

### Week 1（Day 21-25）Backend 基礎
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 21 | `[10]` Kotlin + Spring Boot 分層架構筆記 | `[30]` 建立 Spring Boot 專案（Gradle, Kotlin DSL） |
| 22 | `[10]` Docker Compose 基本語法筆記 | `[30]` 設定 Docker MySQL container |
| 23 | `[10]` DataSource / Connection Pool 筆記 | `[30]` Database Connection 設定 + 測試連線 |
| 24 | `[10]` Flyway Migration 概念筆記 | `[30]` 建立 Flyway Migration 腳本（V1__init.sql） |
| 25 | `[10]` JPA Entity 設計筆記 | `[60]` 建立 `User` / `Transaction` Entity + Table |

### Week 2（Day 26-30）分層架構
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 26 | `[10]` Repository Pattern 筆記 | `[30]` 實作 `UserRepository` / `TransactionRepository` |
| 27 | `[10]` Service Layer 職責邊界筆記 | `[30]` 實作 `UserService` / `TransactionService` |
| 28 | `[10]` Controller + DTO 設計筆記 | `[30]` 實作 Controller + DTO mapping |
| 29 | `[10]` Bean Validation 策略筆記 | `[30]` 加入 Validation 規則（`@Valid`, custom validator） |
| 30 | `[10]` Exception Handling 設計筆記 | `[60]` 實作 `GlobalExceptionHandler` + Swagger（springdoc-openapi） |

📄 產出：`Spring Boot Layer Architecture.md`

### Week 3（Day 31-35）Transaction Module API
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 31 | `[10]` 分頁設計筆記（offset vs cursor） | `[30]` 實作 Transaction API + Pagination |
| 32 | `[10]` 排序/篩選設計筆記 | `[30]` 實作 Sorting + Filtering API |
| 33 | `[10]` 搜尋功能設計筆記 | `[30]` 實作 Search API（關鍵字/日期範圍） |
| 34 | `[10]` Mock Data 策略筆記 | `[60]` 建立 Mock Data 產生器（先產出 1 萬筆交易） |
| 35 | `[10]` API 效能初測筆記 | `[30]` API 測試 + Postman/Bruno 集合整理 |

### Week 4（Day 36-40）Frontend 串接 + Demo
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 36 | `[10]` React Query 概念筆記 | `[30]` 前端串接 Transaction API |
| 37 | `[10]` Loading/Error State Pattern 筆記 | `[30]` 實作 Loading State + Error State |
| 38 | `[10]` Retry 策略筆記 | `[30]` 實作 Retry 機制 + Empty State |
| 39 | `[10]` UI Polish checklist | `[30]` 整理 Transaction Dashboard UI |
| 40 | `[10]` Demo 腳本撰寫 | `[60]` Demo 錄製 + 完成 Architecture 文件 |

**Month 2 成果**：`Transaction Dashboard v1`（React + Kotlin + MySQL 全串通）。

---

## Month 3 — 大資料量與 Performance

### Week 1（Day 41-45）Data Scale
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 41 | `[10]` 大量資料測試計畫筆記 | `[30]` 產生 10 萬筆交易資料腳本 |
| 42 | `[10]` Batch Insert 優化筆記 | `[60]` 產生 50 萬 + 100 萬筆交易資料 |
| 43 | `[10]` 壓測工具筆記（k6 / JMeter） | `[30]` 測試 API 回應速度（建立基準值 baseline） |
| 44 | `[10]` 慢查詢分析方法筆記 | `[30]` 分析並找出慢 SQL |
| 45 | `[10]` Index 設計原則筆記 | `[60]` 設計並建立 Index + 重新測試 |

### Week 2（Day 46-50）SQL 優化 + Benchmark
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 46 | `[10]` EXPLAIN 執行計畫閱讀筆記 | `[30]` 執行 `EXPLAIN` 分析查詢 |
| 47 | `[10]` SQL 優化技巧清單 | `[30]` 改善 SQL 查詢（重寫 / 覆蓋索引） |
| 48 | `[10]` Cursor-based Pagination 概念筆記 | `[30]` 實作 Pagination 最佳化（keyset pagination） |
| 49 | `[10]` Benchmark 腳本設計筆記 | `[30]` 建立 API Benchmark 測試腳本 |
| 50 | `[10]` 效能數據整理方法 | `[60]` 整理 Before/After 效能比較 + 撰寫文件初稿 |

📄 產出：`How I optimize million transaction query.md`

### Week 3（Day 51-55）React Performance 量測
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 51 | `[10]` React Render 機制筆記（reconciliation） | `[30]` 測量元件 Render 時間 |
| 52 | `[10]` React DevTools Profiler 使用筆記 | `[30]` 用 Profiler 找出不必要的 Render |
| 53 | `[10]` React.memo 原理筆記 | `[30]` 套用 `React.memo` 優化 |
| 54 | `[10]` useMemo/useCallback 比較筆記 | `[30]` 套用 `useMemo` / `useCallback` |
| 55 | `[10]` Virtualization 概念筆記 | `[60]` 實作 Virtual List（`react-window` / `tanstack-virtual`） |

### Week 4（Day 56-60）優化 + Report
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 56 | `[10]` Infinite Scroll 設計筆記 | `[30]` 實作 Infinite Scroll |
| 57 | `[10]` Code Splitting / Lazy Loading 筆記 | `[30]` 套用 `React.lazy` + Code Splitting |
| 58 | `[10]` Bundle Analyzer 使用筆記 | `[30]` 分析並縮減 Bundle Size |
| 59 | `[10]` Lighthouse 指標筆記（LCP/TBT/CLS） | `[30]` 跑 Lighthouse 並改善分數 |
| 60 | `[10]` 效能報告大綱 | `[60]` 撰寫 `Transaction Dashboard Performance Report.md`（附 Before/After 數據） |

**Month 3 成果**：百萬交易資料查詢與最佳化的完整效能報告。

---

## Month 4 — Security + Realtime + Enterprise Architecture

### Week 1（Day 61-65）Authentication 基礎
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 61 | `[10]` Spring Security 架構筆記 | `[30]` 整合 Spring Security 基本設定 |
| 62 | `[10]` JWT 結構與簽章機制筆記 | `[30]` 實作 JWT Login API |
| 63 | `[10]` Access Token 設計筆記 | `[30]` 實作 Access Token 簽發與驗證 |
| 64 | `[10]` Refresh Token / Rotation 策略筆記 | `[30]` 實作 Refresh Token + Token Rotation |
| 65 | `[10]` 前端 Route Guard 筆記 | `[60]` 實作前端 Route Guard + 登入流程整合 |

### Week 2（Day 66-70）RBAC
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 66 | `[10]` Permission Model 設計筆記 | `[30]` 設計 Role/Permission 資料模型 |
| 67 | `[10]` RBAC 概念筆記 | `[30]` 實作後端 RBAC 授權邏輯（`@PreAuthorize`） |
| 68 | `[10]` 前端權限控制模式筆記 | `[30]` 實作 Permission-based UI（隱藏/禁用元件） |
| 69 | `[10]` Session 管理筆記 | `[30]` 實作 Session Timeout 機制 |
| 70 | `[10]` 安全整合測試 checklist | `[60]` 整合測試 + 撰寫 `Banking Authentication Design.md` |

### Week 3（Day 71-75）Trusted Device + Audit
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 71 | `[10]` Device Fingerprint 概念筆記 | `[30]` 建立 Device Table |
| 72 | `[10]` 信任裝置流程設計筆記 | `[30]` 實作 Trusted Device Flow |
| 73 | `[10]` Audit Log 設計筆記 | `[30]` 建立 Audit Log Table + 記錄邏輯（AOP） |
| 74 | `[10]` Timeline UI 設計筆記 | `[30]` 實作 Audit Timeline UI |
| 75 | `[10]` OWASP Top 10 對照 checklist | `[60]` 安全性自我檢測 + 文件整理 |

### Week 4（Day 76-80）Realtime
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 76 | `[10]` WebSocket 協定筆記 | `[30]` 建立 WebSocket Server 端（STOMP） |
| 77 | `[10]` 前端 WebSocket 整合筆記 | `[30]` 前端建立 WebSocket 連線 |
| 78 | `[10]` Reconnect / Heartbeat 策略筆記 | `[30]` 實作 Reconnect + Heartbeat 機制 |
| 79 | `[10]` Notification 資料設計筆記 | `[30]` 實作 Notification Center UI |
| 80 | `[10]` Realtime Demo 腳本 | `[60]` 整合 Realtime Notification Demo + 撰寫 `Enterprise Banking Feature.md` |

**Month 4 成果**：銀行等級安全（JWT/RBAC/Audit）與即時通知系統。

---

## Month 5 — AI Banking Assistant

### Week 1（Day 81-85）LLM / RAG 基礎
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 81 | `[10]` LLM 基本概念筆記 | `[30]` Token / Prompt 概念實驗（呼叫 API 測試） |
| 82 | `[10]` Embedding 概念筆記 | `[30]` Embedding API 測試（文字轉向量） |
| 83 | `[10]` Vector Database 比較筆記（pgvector / Pinecone / Qdrant） | `[30]` 選型 + 建立 Vector Store |
| 84 | `[10]` RAG 流程圖繪製 | `[30]` 設計 RAG Pipeline 架構 |
| 85 | `[10]` Chunk Strategy 筆記 | `[60]` 實作文字 Chunking 策略 + Retrieval 雛形 |

### Week 2（Day 86-90）Embedding & Vector Search
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 86 | `[10]` Transaction 資料結構整理筆記 | `[30]` 整理 Transaction 資料供 AI 使用 |
| 87 | `[10]` Embedding Pipeline 設計筆記 | `[30]` 建立 Embedding 產生流程（批次） |
| 88 | `[10]` Vector Search 查詢筆記 | `[30]` 實作 Vector Search API |
| 89 | `[10]` Similarity 門檻調參筆記 | `[30]` 調整 top-k / 相似度門檻 |
| 90 | `[10]` RAG API 整合筆記 | `[60]` 實作 RAG API（檢索 + 組合 Prompt） |

### Week 3（Day 91-95）AI Chat UI
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 91 | `[10]` Chat UI 設計稿 | `[30]` 建立 AI Chat UI 元件 |
| 92 | `[10]` Streaming Response 概念筆記（SSE） | `[30]` 實作 Streaming Response |
| 93 | `[10]` Markdown 渲染筆記 | `[30]` 實作 Markdown Render（AI 訊息顯示） |
| 94 | `[10]` Chat State 管理筆記 | `[30]` 管理對話歷史 State |
| 95 | `[10]` Chat UI 整合測試 checklist | `[60]` 整合 Chat UI + API 串接測試 |

### Week 4（Day 96-100）AI 功能整合
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 96 | `[10]` 消費分析 Prompt 設計筆記 | `[30]` 實作 AI 消費分析功能 |
| 97 | `[10]` 摘要功能 Prompt 設計筆記 | `[30]` 實作 AI 摘要功能 |
| 98 | `[10]` AI 問答情境設計筆記 | `[30]` 實作 AI 問答功能（交易查詢） |
| 99 | `[10]` Prompt Optimization 技巧筆記 | `[30]` 優化 Prompt（few-shot / system prompt） |
| 100 | `[10]` AI 功能整合測試 checklist | `[60]` 整合測試 + 撰寫 `Building RAG Financial Assistant.md` |

**Month 5 成果**：RAG 金融分析助手（可對交易資料問答）。

---

## Month 6 — Production 化 + 面試作品整理

### Week 1（Day 101-105）Docker / CI
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 101 | `[10]` Docker Compose 完整架構筆記 | `[30]` 完整化 Docker Compose（前後端 + DB + Redis） |
| 102 | `[10]` Multi-stage Build 筆記 | `[30]` 撰寫 Frontend Dockerfile |
| 103 | `[10]` Backend Dockerfile 筆記 | `[30]` 撰寫 Backend Dockerfile |
| 104 | `[10]` Nginx 反向代理筆記 | `[30]` 設定 Nginx + Environment Config |
| 105 | `[10]` CI/CD 概念筆記 | `[60]` 建立 GitHub Actions CI Pipeline |

### Week 2（Day 106-110）Testing
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 106 | `[10]` 測試金字塔筆記 | `[30]` 設定 Backend 測試框架（JUnit5 + MockK） |
| 107 | `[10]` 單元測試撰寫筆記 | `[30]` 撰寫 Backend Unit Test（Service 層） |
| 108 | `[10]` 整合測試筆記（Testcontainers） | `[30]` 撰寫 Backend Integration Test |
| 109 | `[10]` 前端測試工具筆記（Vitest/RTL） | `[30]` 撰寫 Frontend Component Test |
| 110 | `[10]` 覆蓋率檢查筆記 | `[60]` 補齊測試覆蓋率 + CI 整合測試 |

### Week 3（Day 111-115）Portfolio 文件/圖表
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 111 | `[10]` README 架構規劃 | `[30]` 整理專案總 README |
| 112 | `[10]` 系統架構圖繪製筆記 | `[30]` 繪製架構圖（Mermaid） |
| 113 | `[10]` ER Diagram 繪製筆記 | `[30]` 繪製 Database ER Diagram |
| 114 | `[10]` Sequence Diagram 筆記 | `[30]` 繪製 API Sequence Diagram |
| 115 | `[10]` 報告彙整清單 | `[60]` 彙整 Performance Report + Security Report + AI Architecture 文件 |

### Week 4（Day 116-120）Demo / 面試準備
| Day | 暖身任務 | 核心任務 |
|---|---|---|
| 116 | `[10]` Demo 腳本撰寫 | `[30]` 錄製完整 Demo 影片 |
| 117 | `[10]` React 面試題複習清單 | `[30]` Mock Interview — React 問題 |
| 118 | `[10]` JS 面試題複習清單 | `[30]` Mock Interview — JavaScript 問題 |
| 119 | `[10]` Architecture 面試題複習清單 | `[30]` Mock Interview — Architecture / 專案問題 |
| 120 | `[10]` 履歷草稿 | `[60]` 用 STAR 法整理履歷 + 最終 GitHub Portfolio 收斂上架 |

**Month 6 成果**：可直接投遞的 Senior Frontend Portfolio（含完整文件、圖表、Demo 影片）。

---

## 任務統計

- 總任務數：**240**（6 個月 × 4 週 × 5 天 × 2 任務）
- `[5]` 任務：1（第一天建 repo）
- `[10]` 任務：119（暖身任務主力）
- `[30]` 任務：92
- `[60]` 任務：28（每週五的整合/收斂任務）
