# リサーチサマリー

## 調査日: 2026-03-01

### 制約
- Brave Search API未設定のためX（Twitter）検索不可
- web_fetchでURL直接取得のみ実施
- ガガロットAI（@gagarotai200）のX投稿は取得不可（X側のブロック）

---

## 1. OpenClaw マルチエージェント構成

| # | ソース | 要約 | 信頼度 |
|---|--------|------|--------|
| 1 | OpenClaw公式docs | マルチエージェントは`agents.list[]`で定義。各agentは独立したworkspace・agentDir・sessionsを持つ。bindingsでチャネル→エージェントのルーティング | ★★★ |
| 2 | OpenClaw公式docs | ワークスペースはAGENTS.md/SOUL.md/USER.md/IDENTITY.md等の標準ファイル構成。Git private repoでバックアップ推奨 | ★★★ |
| 3 | OpenClaw公式docs | セッション管理はGatewayが中心。sessions.jsonにメタデータ、.jsonlにトランスクリプト。トークン数・コストもGateway経由 | ★★★ |
| 4 | OpenClaw公式docs | Control UIはVite+Litベース。WebSocketでGateway通信。セッション一覧・Cron管理・チャット等が既存 | ★★★ |
| 5 | OpenClaw公式docs | Usage trackingはプロバイダーのAPI報告値。/statusでトークン+推定コスト表示 | ★★★ |

## 2. ガガロットAI Mission Control（feedbackからの二次情報）

| # | ソース | 要約 | 信頼度 |
|---|--------|------|--------|
| 6 | にっしーfeedback引用 | 6ツール: タスクボード、カレンダー、メモリ画面、チーム画面、コンテンツパイプライン、オフィス画面 | ★★ |
| 7 | にっしーfeedback引用 | 実装優先: タスクボード → カレンダー → メモリ画面 → チーム画面 → オフィス画面 | ★★ |
| 8 | にっしーfeedback引用 | 最大の価値は「AIが今何をしてるか分からない」不安の解消 | ★★★ |

## 3. 技術的知見

| # | ソース | 要約 | 信頼度 |
|---|--------|------|--------|
| 9 | OpenClaw docs | Cron jobsはGateway内蔵。`~/.openclaw/cron/jobs.json`に永続化 | ★★★ |
| 10 | OpenClaw docs | per-agent sandbox + tool制限が可能 | ★★★ |

---

## 設計への示唆

### データソース戦略
影分身の活動データは以下から取得可能:
1. **OpenClaw Gateway API（WebSocket）** — セッション一覧、トークン数、コスト
2. **Git リポジトリ（GitHub API）** — コミット履歴、ファイル変更
3. **ワークスペースファイル** — memory/*.md、タスクファイル
4. **Cron jobs** — スケジュール済みタスク

### OpenClaw Control UIとの差別化
- **影分身特化** — 複数エージェントの「今何してるか」に特化
- **ゲーミフィケーション** — ビジュアル重視、アバター、進捗バー
- **にっしー専用** — 1人のオーナーが10人の影分身を一元管理

### nishiyama002の役割明確化
- OpenClawのマルチエージェント機能をフル活用
- 独自のNext.jsアプリをVercelにデプロイ
- GatewayのWebSocket APIでリアルタイムデータ取得
- GitHubのAPIでコミット・ファイル変更を追跡
