# nishiyama002 起動方式比較 — v0.1

> 作成日: 2026-03-05 12:30
> 担当: 西山1号
> 目的: にっしーの起動方式判断を支援する比較資料

---

## 選択肢

| 項目 | OpenClaw（推奨） | Codex CLI直接 |
|------|-----------------|--------------|
| セットアップ | `openclaw agent create` で統合管理 | 個別にCodex CLI設定 |
| タスク受信 | sessions_send で1号から直接指示 | 手動 or スクリプト経由 |
| メモリ共有 | workspace内のmemory/を共有可能 | 別途同期が必要 |
| 監視 | ダッシュボードで状態確認可 | pm2 or 独自監視 |
| モデル切替 | session_statusでオンザフライ変更 | 環境変数変更→再起動 |
| 1号との連携 | ネイティブ（sessions_send/subagents） | webhook or ファイル経由 |
| コスト管理 | OpenClaw側で一元管理 | 個別トラッキング |

## 1号の推奨: OpenClaw

理由:
1. 1号→2号のタスク指示がsessions_sendで完結する（設計→実装のフロー自動化）
2. ダッシュボードで2号の稼働状態が見える（Phase 3-1で実装済み）
3. memory/の共有で設計⇔実装のコンテキストが途切れない
4. 3号（開発）と同じインフラなので運用が統一される

## にっしーへの確認事項

1. OpenClaw経由でOKか？ → OKなら `openclaw agent create nishiyama002` で進める
2. 2号のデフォルトモデルは？（候補: claude-sonnet-4-20250514 推奨 — コード生成のコスパ）
3. AXAD repoへのアクセス方法（git clone先、ブランチ戦略）
