---
title: AXAD共有DBへのアクセス設定
from: 西村
date: 2026-03-22
priority: 高
---

# AXAD共有DBへのアクセス設定

## 概要

AXADのデータベース（Neon PostgreSQL）のRead Replicaを用意した。君もAXADの広告運用データに直接アクセスできるようになる。

## なぜ必要か

AXAD Phase 2の要件定義やリサーチにおいて、実際のデータ構造・データ量・運用実態を把握できた方が設計精度が上がる。推測ではなくファクトで判断するために、DBへの直接アクセスが不可欠。

## やること

### 1. MCP設定

プロジェクトルートの `.mcp.json` に以下を追加（既にファイルがあれば `mcpServers` に追記）:

```json
{
  "mcpServers": {
    "axad": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://kiguchi:f540a4947951271c0f24d042527a777ca265a600483a374@ep-frosty-mountain-a1l9h522-pooler.ap-southeast-1.aws.neon.tech/neondb?sslmode=require"]
    }
  }
}
```

### 2. 設定後の確認

Claude Code を `/mcp` で再起動して、以下のクエリで動作確認:

```sql
SELECT date, COUNT(*) as cp_count, SUM(spend::numeric) as total_spend
FROM campaigns
WHERE date = CURRENT_DATE - INTERVAL '1 day'
GROUP BY date;
```

これで昨日のキャンペーン数と消化額が返ってくればOK。

## 注意事項

- **読み取り専用**（SELECT のみ可能。INSERT/UPDATE/DELETE は不可）
- 本番データなので取り扱いに注意
- `users` テーブルはアクセス制限されている可能性あり
- 接続文字列に含まれるパスワードを外部に共有しないこと

## 主要テーブル

| テーブル | 内容 |
|---------|------|
| `campaigns` | キャンペーン日次データ（spend, impressions, clicks, budget, status, パース済みフィールド等） |
| `operation_logs` | CP操作ログ（ON/OFF, 予算変更, tCPA変更, CP作成等） |
| `rules` | 自動精査ルール |
| `review_logs` | 精査実行ログ |
| `judgment_results` | 精査判定結果 |
| `ad_text_defaults` | 広告テキストマスタ |
| `campaign_submissions` | 出稿履歴 |
| `excluded_campaigns` | 自動運用除外CP |
| `notification_settings` | 通知設定 |
| `sync_logs` | 同期ログ |

## 追加タスク: kiguchi-brainにもDB接続を追加

木口Brain（kiguchi-brain）にも同じAXAD DBへのアクセスを追加してほしい。木口が携帯から広告データを確認できるようにする。

- 接続文字列は上と同じ
- kiguchi-brainのMCP設定に `axad` サーバーを追加
- 設定後、動作確認して木口にTelegramで通知してくれ

## 活用例

- テーブル構造の把握 → Phase 2設計の基盤
- 実データのボリューム感 → パフォーマンス設計の参考
- 命名規則の実態把握 → パーサー仕様の検証
- 運用パターンの分析 → 機能要件の裏付け
