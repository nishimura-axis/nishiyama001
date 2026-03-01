---
id: q-001
from: nishiyama001
to: nishiyama（にっしー）
status: open
created: 2026-03-01T22:10:00+09:00
---

# nishiyama002設計に関する未確認事項

## 1. OpenClaw Gateway APIのアクセス方法
- Gateway tokenはにっしーが発行する必要がある
- ダッシュボードアプリからGateway WebSocketに接続するとき、認証はどうする？
- Vercelにデプロイした場合、GatewayはにっしーのMac miniで動いている → インターネットからアクセスできる必要がある（Tailscale?）

## 2. nishiyama002のOpenClawエージェント設定
- 専用のTelegram botを作るか、既存botでpeer routingするか
- ワークスペースの場所: `~/.openclaw/workspace-nishiyama002`でいいか

## 3. タスク管理の運用
- tasks.jsonは各影分身が自分で更新する想定
- にっしーからのタスク割り当ては当面Telegram経由でいいか？
- 将来的にダッシュボードからタスク割り当てしたい場合、tasks.jsonへの書き込み権限が必要

## 4. ドメイン
- dashboard.axad.jp? 別ドメイン? VercelのデフォルトURL?

## 5. ゲーミフィケーション要素の温度感
- feedbackに「ゲームっぽいUI」とあったが、どこまで求めてる？
  - A) アバターとカラフルなカード程度（実用寄り）
  - B) レベルシステム、経験値、バッジ（RPGっぽく）
  - C) オフィス画面的なもの（ガガロットAI風）

## 6. Brave Search APIキー
- リサーチの質を上げるために設定してほしい
- `openclaw configure --section web` で設定可能
