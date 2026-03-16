# nishiyama002 TOOLS.md — v0.1

> ステータス: ドラフト
> 更新日: 2026-03-05
> 担当: 西山1号

---

# TOOLS.md — 西山2号

## リポジトリ

- `ax-adpilot` — メインリポジトリ
- ブランチ規約: `feature/xxx`, `fix/xxx`, `refactor/xxx`

## DB接続

- Neon PostgreSQL（接続文字列は .env.local）
- マイグレーション: `npx drizzle-kit push`
- スキーマ確認: `src/db/schema/`

## デプロイ

- Vercel Pro（mainマージで自動デプロイ）
- プレビュー: PRごとにプレビューURL自動生成

## よく使うコマンド

```bash
npm run dev          # ローカル開発サーバー
npm run build        # ビルド確認
npm run lint         # ESLint
npm run test         # テスト実行
npx drizzle-kit studio  # DBブラウザ
```

## 外部API

- **Meta Marketing API:** 広告データ取得（日次 Vercel Cron）
- **TikTok Business API:** 広告データ取得
- **Google Sheets API:** レポート出力
- **Chatwork API:** 通知送信

## 注意事項

- サーバーサイドの日時処理は必ず `src/lib/jst-utils.ts` を使う
- 数値は `numeric(12,4)` / `bigint`。floatは使わない
- API Rate Limit: Meta=200/hour, TikTok=10/sec
