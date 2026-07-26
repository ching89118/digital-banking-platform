# AI-Powered Digital Banking Platform

> 🚧 專案開發中 — Month 1 / Week 2 / Day 7（JS/React 基礎 + 專案初始化階段）

一個模擬真實銀行系統的全端專案，涵蓋交易查詢、身份驗證、即時通知，以及基於 RAG 的 AI 金融助手。這不是一份練習作業，而是依照 [六個月執行手冊](./ROADMAP.md) 逐日建構的完整工程作品，每個功能都對應真實的架構決策與效能數據。

[![Status](https://img.shields.io/badge/status-in%20development-yellow)]()
[![Progress](https://img.shields.io/badge/progress-Day%206%20%2F%20120-blue)]()
[![License](https://img.shields.io/badge/license-MIT-lightgrey)]()

## 專案狀態

| 項目         | 狀態                                 |
| ------------ | ------------------------------------ |
| 目前階段     | Month 1 / Week 2 — Async 與工程能力  |
| 完成任務     | 12 / 240                             |
| 下一個里程碑 | Banking UI Prototype（Month 1 結束） |

## 這個專案要解決什麼

模擬一個具備企業級架構思維的數位銀行平台，重點展示：

- 大資料量（百萬等級）交易查詢的效能優化
- 銀行等級的安全設計（JWT / RBAC / Audit Log）
- 即時通知（WebSocket）
- 用 RAG 讓使用者能用自然語言查詢自己的消費資料

## 技術棧

| 分類     | 技術                                                |
| -------- | --------------------------------------------------- |
| Frontend | React + TypeScript + Next.js（後期導入）            |
| Backend  | Kotlin + Spring Boot                                |
| Database | MySQL（Flyway migration）                           |
| Cache    | Redis（Month 4 後期）                               |
| Infra    | Docker Compose + Nginx + GitHub Actions             |
| AI       | RAG（Embedding + Vector Search）+ AI Chat Assistant |

## 專案結構

```
digital-banking-platform/
├── frontend/           # React + TS 前端（Month 1 起）
├── backend/            # Kotlin + Spring Boot 後端（Month 2 起）
├── docs/               # 技術文件（架構決策、效能報告、安全報告）
├── api/                # OpenAPI 規格
├── docker-compose.yml  # 本地開發環境（Month 2 起）
└── ROADMAP.md          # 240 個微任務完整路線圖
```

> 目前僅有 Git repo 骨架，`frontend/` `backend/` 將依 ROADMAP 逐步建立。

## 開發路線圖

完整的六個月、240 個任務規劃見 [`ROADMAP.md`](./ROADMAP.md)：

| 月份    | 主題                                          | 最終成果                  |
| ------- | --------------------------------------------- | ------------------------- |
| Month 1 | JS/React 基礎 + 專案初始化                    | Banking UI Prototype      |
| Month 2 | Digital Banking Core Platform                 | Transaction Dashboard v1  |
| Month 3 | 大資料交易系統 + Frontend Performance         | 百萬交易查詢優化報告      |
| Month 4 | Security + Realtime + Enterprise Architecture | 銀行等級安全與即時通知    |
| Month 5 | AI Banking Assistant                          | RAG 金融分析助手          |
| Month 6 | Production 化 + 作品集整理                    | Senior Frontend Portfolio |

## 快速開始

> ⚠️ 專案仍在 Day 7，尚無可執行程式碼（React 專案要到 Week 3 Day 11 才會建立）。以下為 Month 1 完成後預計的啟動方式，會隨進度更新。

```bash
# Frontend（Month 1 起可用）
cd frontend
npm install
npm run dev

# 完整環境（Month 2 起可用，需 Docker）
docker compose up
```

## 技術文件

隨開發進度累積於 `docs/`，包含每個重大決策的背景、選項比較與效能數據：

- `docs/architecture.md` — 系統架構圖
- `docs/er-diagram.md` — Database ER Diagram
- `docs/git-commit-convention.md` — Commit 規範

## 開發紀錄

本專案採「每日一任務」方式開發，過程紀錄見 `daily-logs/`，每週驗收見對應 `weekly-review`。

## License

MIT
