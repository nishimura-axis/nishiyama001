# nishiyama002 セットアップチェックリスト

_作成: 2026-03-05 10:30 by 1号_
_目的: にっしーの起動方式決定後、即座にデプロイできるよう準備_

## 共通（OpenClaw / Codex CLI どちらでも必要）

- [ ] ワークスペース作成: `~/nishiyama002/`
- [ ] SOUL.md 配置（v0.2 → v1.0 確定後）
- [ ] AGENTS.md 配置（v0.2 → v1.0 確定後）
- [ ] TOOLS.md 作成（AXAD固有の設定: DB接続先、APIキー参照先、デプロイ先）
- [ ] USER.md 作成（にっしー情報 + 1号との連携方法）
- [ ] memory/ ディレクトリ作成

## OpenClaw エージェント方式の場合（1号推奨）

- [ ] `openclaw agent create nishiyama002`
- [ ] Telegramチャンネル設定（専用 or 共有グループ）
- [ ] heartbeat設定（保守ルーティン自動化用）
- [ ] 1号→2号の sessions_send 疎通テスト
- [ ] 5号（品質管理）との連携テスト

## Codex CLI 方式の場合

- [ ] codex CLI インストール確認
- [ ] AGENTS.md に起動コマンド記載
- [ ] 1号からの呼び出しスクリプト作成（必要時のみ起動）

## デプロイ後の初期タスク

1. AXADリポジトリのコード理解（README、ディレクトリ構成把握）
2. Phase 1コードのhealth check（テスト実行、lint確認）
3. Phase 2設計書の読み込み（requirements/ 全ファイル）
4. 1号への理解度報告

## 備考

- 技術スタック統合は完了済み（requirements/17, 18参照）
- にっしーの方針確認後、このチェックリストに沿って即実行可能
