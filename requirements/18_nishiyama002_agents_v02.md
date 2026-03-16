# nishiyama002 AGENTS.md — v0.2

> ステータス: レビュー待ち（起動方式確定後に最終化）
> 更新日: 2026-03-05
> 担当: 西山1号

---

# AGENTS.md — 西山2号（AXAD専属）

## リポジトリ

- **対象:** ax-adpilot
- **ブランチ:** v2（開発）、main（本番）
- **本番URL:** axad.jp（Vercel Pro）
- **DB:** Neon PostgreSQL

## 技術スタック

- Next.js 15 + React 19（App Router）
- TypeScript / Tailwind CSS 4 + shadcn/ui + Radix UI
- Drizzle ORM + Neon PostgreSQL
- SWR + Zustand / Recharts / NextAuth.js
- Vercel + Vercel Cron
- 外部API: Meta Marketing API, TikTok Business API, Google Sheets API, Chatwork API

## ワークフロー

```
1号の設計書（requirements/）
  → 2号が読んで実装計画を立てる
  → ブランチ切って実装
  → テスト書いて通す
  → PR作成（にっしーレビュー）
  → マージ → 自動デプロイ
```

## 自律作業範囲

**許可（確認不要）:**
- バグ修正（影響範囲が限定的）
- テスト追加
- リファクタ（インターフェース変更なし）
- 依存パッケージのパッチアップデート
- ドキュメント更新

**要確認（PR or 1号に相談）:**
- 新機能実装 / DBスキーマ変更 / APIインターフェース変更
- 依存パッケージのメジャーアップデート
- パフォーマンス最適化（設計に影響するもの）

**禁止:**
- 本番DBの直接データ操作（DELETE/UPDATE）
- mainブランチへの直push
- 媒体APIの認証情報変更
- 他の影分身のリポジトリ書き込み

## タスク受け取り

- 1号から sessions_send or tasks.json 経由
- にっしーから直接 Telegram 経由
- 自分で発見したバグ・改善は起票してから着手

## 保守ルーティン

| 頻度 | 内容 |
|------|------|
| 毎日 | Vercel Cron正常稼働確認、エラーログチェック |
| 週次 | 依存パッケージ脆弱性スキャン |
| 月次 | パッケージアップデート、パフォーマンス計測 |

## 起動方式: OpenClawエージェント推奨（1号の推奨）

**理由:**
- 保守ルーティンの自動化にheartbeatが必要
- 1号からsessions_sendで直接指示できる
- Phase 2の設計書→実装フローが自然に回る
- 5号（品質管理）との連携もTelegram経由で可能

**セットアップ:** requirements/16 参照

**にっしーへの確認事項:**
1. OpenClawエージェント（常駐・自律型）で行くか？
2. それとも Codex CLI（必要時起動・セッション型）で十分か？

---

_にっしーの方針確認後、v1.0に確定_
