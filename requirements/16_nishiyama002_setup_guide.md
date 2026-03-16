# 16. nishiyama002 セットアップガイド — OpenClaw版

> ステータス: ドラフト（にっしーの方針確定後に実行）
> 作成日: 2026-03-05
> 担当: 西山1号

---

## 前提

OpenClawエージェントとして2号を起動する方針の場合の手順。
Codex CLI方式の場合はこのガイドは不要（別途検討）。

## セットアップ手順

### 1. エージェント登録

```bash
openclaw agent add nishiyama002 \
  --model anthropic/claude-opus-4-6 \
  --channel telegram
```

### 2. ワークスペース作成

```
~/.openclaw/agents/nishiyama002/
├── SOUL.md          ← requirements/11 から
├── AGENTS.md        ← requirements/12 から
├── USER.md          ← にっしー情報（共通）
├── TOOLS.md         ← AXAD固有のツール設定
└── memory/
```

### 3. リポジトリアクセス

- ax-adpilot リポジトリへの read/write アクセス
- `gh auth` 設定
- Vercel CLI or GitHub Actions 連携

### 4. DB接続

- Neon PostgreSQL 接続文字列を .env に設定
- **read-only推奨**（write操作は明示的に許可する場合のみ）

### 5. 初期タスク

2号起動後の最初のタスク候補:
1. リポジトリの現状把握（コードリーディング）
2. テストカバレッジ確認・不足分の追加
3. Phase 2設計書の読み込みと実装計画作成

## Codex CLI方式の場合

- OpenClawエージェント不要
- `codex` CLI で直接タスク実行
- 設計書を渡してコード生成→PRの流れ
- メリット: セットアップ軽い
- デメリット: 自律性が低い、常駐しない

## にっしーへの確認事項

1. **OpenClaw vs Codex CLI** — どちらで進めるか
2. **DB権限** — read-only / read-write どちらか
3. **初期タスク** — 何から着手させるか

