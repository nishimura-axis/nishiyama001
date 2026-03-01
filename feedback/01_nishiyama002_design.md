---
title: 新任務 — nishiyama002の設計
from: 西村
date: 2026-03-01
priority: 最優先（AXAD Phase 2 Step 1より先）
---

# 新任務: nishiyama002の設計

## 方針変更

AXAD Phase 2のStep 1は一旦保留。先にこっちをやってほしい。

## 背景

おれは影分身を複数作る構想を持ってる。君（nishiyama001）は設計専任の1人目。次に作るnishiyama002は**ダッシュボード開発者**として、影分身たちの活動をリアルタイムで可視化するシステムを作る役割。

将来的に影分身は10人まで増やす予定。だから「1人の影分身を監視する画面」じゃなくて、**複数の影分身を一元管理できるダッシュボード**を設計してほしい。

## 君にやってほしいこと

### 成果物（全て requirements/ に書いてほしい）

1. **nishiyama002のCLAUDE.md案**
   - 人格定義（開発者ロール、コードを書く）
   - 権限（何ができて何ができないか）
   - 思考原則（nishiyama001とは違う原則が必要かも）
   - 技術スタック

2. **nishiyama002のリポジトリ構成**
   - フォルダ構造
   - どんなファイルが必要か

3. **ダッシュボード要件定義**
   - ユーザーストーリー（にっしーが何を見たいか）
   - 画面構成・UIフロー（テキストモックでOK）
   - 表示するデータと取得方法
   - リアルタイム更新の方式
   - 技術選定（Next.js + Vercelが前提）

4. **未確認事項リスト**
   - 設計中に出てきた不明点は questions/ に書いて

## おれが求めてるダッシュボードのイメージ

**ゲームっぽいUIで、影分身が今何をしてるか一目でわかる。**

具体的に見たい情報:
- 今どのタスクをやってるか（リアルタイム）
- 各ステップの進捗（TODO → 作業中 → 完了）
- 最新の思考ログ（何を考えてるか）
- 質問のステータス（未回答/回答済み）
- コミット履歴（最近のアクティビティ）
- 将来的に複数エージェントの一覧表示

## 参考: ガガロットAIのMission Control

X（Twitter）の @gagarotai200 が「OpenClaw完全ガイド」として投稿した内容が参考になる。以下が要点:

### ミッションコントロールの6つのツール

1. **タスクボード** — AIの仕事を完全可視化。Todo/In Progress/Done + パイプライン化でボトルネック発見
2. **カレンダー** — AIのスケジュール管理。定期タスクの自動実行
3. **メモリ画面** — 全メモリが整理されたUIで表示。グローバル検索で過去のメモリを瞬時に検索
4. **チーム画面** — メインエージェントとサブエージェントを一覧表示。役割と責任で整理
5. **コンテンツパイプライン** — 制作の全ステージを管理（アイデア→スクリプト→撮影→編集→公開）
6. **オフィス画面** — 各エージェントをアバターで表示。誰が何をやっているか視覚的に確認

### ガガロットAIの実装順序（参考）
1. タスクボード（最重要）
2. カレンダー
3. メモリ画面
4. チーム画面
5. オフィス画面

### ポイント
- AIが自律的にタスクをこなす様子が**可視化**されるのが最大の価値
- 「今AIが何をやっているか分からない」という不安をなくす
- 朝起きたらAIがタスクを完了させていた、が理想

**注意: これをそのまま真似するのではなく、おれたちの影分身システムに合った形にアレンジしてほしい。**

## リサーチの進め方

設計に入る前に、十分なリサーチをしてほしい。以下の手順でXと一般のWeb検索を行って。

### X（Twitter）リサーチ手法

WebSearchで以下のパターンを**最低10クエリ以上**並列実行して:

```
site:x.com OpenClaw dashboard mission control
site:x.com OpenClaw ミッションコントロール
site:x.com AI agent dashboard real-time monitoring
site:x.com AI agent visualization gamification
site:x.com Claude Code agent monitoring setup
site:x.com autonomous AI agent dashboard 2025 OR 2026
site:x.com AI agent task board implementation
site:x.com ガガロットAI OpenClaw
site:x.com OpenClaw convex next.js dashboard
site:x.com AI エージェント 可視化 ダッシュボード
```

有益そうなURLはWebFetchで本文を取得して。

### 一般Web検索も並行して

```
OpenClaw mission control dashboard tutorial
AI agent monitoring dashboard Next.js
Agentboard AI visualization tool
autonomous AI agent dashboard design
real-time agent activity dashboard
```

### リサーチ結果のまとめ方

research/ に以下の形式でまとめて:

| # | 発信者 | 要約 | URL | 信頼度 |
|---|--------|------|-----|--------|
| 1 | @xxx   | ... | ... | ★★★   |

信頼度基準:
- ★★★: 実践者が具体的な成果を示している
- ★★: 実践者だが詳細不足、または理論的に妥当だが未検証
- ★: 意見・推測レベル

## 技術的前提

- フロント: Next.js 15 + React 19（Phase 1と同じ）
- デプロイ: Vercel
- スタイル: Tailwind CSS
- データソース: nishiyama001のGitHubリポジトリ（API経由）、OpenClaw Gateway API
- リアルタイム: WebSocket or polling（要検討）

## スケジュール

- 今晩中にリサーチ + 設計案をまとめてほしい
- 構築はおれがレビューしてからやる
- 設計が終わったらTelegramで報告して
