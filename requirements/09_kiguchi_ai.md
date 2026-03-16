# 木口AI化 — 業務ログ自動蓄積 → 思考回路再現システム

> ステータス: 設計中（v0.3）
> 作成日: 2026-03-04
> 担当: 西山1号
> ソース: Vault 西村AI化構想/影分身/木口AI化.md + にっしーヒアリング（3/4）

---

## 本当のゴール

**木口の思考回路を言語化し、再現性を持たせること。**

ログ蓄積はそのための手段。木口は限りなくノンストレスで、これまでと同じ生活をするだけ。自動でログが集計される。

## 設計原則

1. **木口の負荷ゼロ** — Plaudを装着する以外、何もしない
2. **全会話を対象** — 口頭もオンラインも漏れなく
3. **にっしーの環境で先に構築** → 動作確認 → 木口に展開

---

## データ収集パイプライン

### 1. 対面・口頭会話 → Plaud NotePin → Zapier → Obsidian

```
Plaud NotePin（装着するだけ）
  → 録音完了 → Plaudアプリ内で自動文字起こし・話者分離
  → Zapier トリガー発火（文字起こし完了時）
  → Zapier アクション: Obsidianフォルダにファイル保存
```

#### Plaud × Zapier 接続手順
1. Plaudアプリ → Explore → Integrations → Zapier → 「Go to Zapier」
2. Zapierにサインイン
3. Plaudアカウントと連携（Allow access）
4. Zap作成:
   - **トリガー**: Plaud — 文字起こし完了（transcription summary completed）
   - **アクション**: ファイル作成（Google Drive → Obsidian同期 or 直接ファイル書き込み）
5. テスト実行 → 問題なければPublish

#### Plaud トリガーイベント（利用可能）
- 文字起こし完了（transcription summary）
- 再文字起こし完了（re-transcription）
- 再要約完了（re-summary）

#### 前提条件
- Plaud Proプラン（月1,400円）
- Zapierアカウント（Freeプラン以上）

### 2. オンラインMTG → tl;dv → API → Obsidian

```
Googleカレンダー連携
  → tl;dv bot が自動参加
  → 録画 → 文字起こし → AI要約
  → API / Webhook で取得
  → Obsidian 保存（+ ChatWork投稿も可能）
```

#### tl;dv 環境
- **プラン**: Business（にっしーアカウントでアップグレード済み）
- **月額**: 6,980円
- **利用可能機能**: Webhook + APIキー
- **実装方法**: APIでMTGデータ取得 → 整形 → Obsidian保存

---

## Obsidian保存フォーマット

### フォルダ構成（案）
```
MyVault/
  30_Projects/
    木口ログ/
      対面/
        2026-03-04_〇〇との打ち合わせ.md
      オンライン/
        2026-03-04_週次定例.md
```

### ファイルテンプレート
```markdown
---
date: 2026-03-04
type: 対面 | オンライン
source: plaud | tldv
participants: [木口, ○○]
duration: 30min
---

## 要約
（AI生成サマリー）

## 判断・意思決定
（抽出された判断ポイント）

## タスク・TODO
（会話から抽出されたアクション）

## 全文
（文字起こし全文）
```

---

## Phase構成

### Phase 1: パイプライン構築（本PJ）

| # | 内容 | 担当 | 期限 |
|---|------|------|------|
| 1-1 | Plaud Pro契約 | にっしー | 3/5 |
| 1-2 | Plaud → Zapier連携設定 | にっしー + 西山 | 3/5 |
| 1-3 | Zapier → Obsidian保存アクション構築 | 西山（設計）→ 3号（実装） | 3/7 |
| 1-4 | tl;dv API → Obsidian保存スクリプト構築 | 西山（設計）→ 3号（実装） | 3/7 |
| 1-5 | テスト運用（にっしーの会話で検証） | にっしー | 3/7-10 |
| 1-6 | 木口環境に展開 | にっしー | 3/10 |
| 1-7 | 木口運用開始 | 木口 | 3/10 |

### Phase 2: 思考回路再現（構想）

蓄積されたログから：
1. **判断パターン抽出** — 繰り返し出現する判断基準をAIで分析
2. **FAQ自動生成** — よく聞かれる質問と木口の回答パターン構造化
3. **戦略壁打ちAI** — 木口の思考回路を再現したAIアシスタント
4. **タスク自動分配** — 会話から抽出されたTODOをChatWork通知
5. **週次サマリー** — 1週間の活動・判断の自動レポート

---

## コスト

| 項目 | 月額 |
|------|------|
| Plaud Pro | 1,400円 |
| tl;dv Business | 6,980円 |
| Zapier | 既存アカウント利用（Free以上） |
| **合計** | **8,380円/月** |

※ 木口展開時は木口用に別途Plaud Pro契約が必要

---

## 実装上の検討事項

### Zapier → Obsidian の接続方法
ObsidianにはネイティブAPIがないため、以下のいずれかで対応：
1. **Google Drive / Dropbox 経由** — Zapier → Google Drive にファイル作成 → Obsidian同期プラグインで自動取り込み
2. **Webhook → スクリプト** — Zapier → Webhook → Mac mini上のスクリプトが受信 → Vaultフォルダに直接ファイル書き込み
3. **Notion経由** — Zapier → Notion → Obsidian同期（間接的すぎるため非推奨）

**推奨: 方法2（Webhook → スクリプト）** — 最もシンプルで遅延なし。Mac miniが常時稼働してるため成立。

### tl;dv API の実装方針
- APIキーでMTG一覧取得 → 未処理分を検出 → 文字起こし・要約取得 → Obsidian保存
- cronで定期実行（15分ごと or MTG終了検知）
- Mac mini上にスクリプト配置

---

## 未確認事項

- [ ] Zapier → Obsidian の具体的な接続方法をにっしーと決定
- [ ] tl;dv APIドキュメントの調査・エンドポイント確認
- [ ] Obsidian保存先のフォルダ構成確定（にっしー確認）
- [ ] 木口用Obsidian Vault は新規 or 既存Vault内
- [ ] 録音許可の確認（外部MTGの場合）

---

## 次のアクション

1. にっしーレビュー（この設計書）
2. tl;dv APIドキュメント調査
3. Zapier → Obsidian接続方法の決定
4. 3号に実装指示

---

## tl;dv API 調査メモ (2026-03-04 20:45)

**結論: 公開APIドキュメントが見当たらない**

調査した URL:
- `https://tldv.io/api-docs` → 404
- `https://docs.tldv.io` → DNS解決不可
- `https://tldv.io/integrations/api` → 404
- `https://tldv.io/app/api-key` → ログイン必要（中身取得不可）

**推測:**
- BusinessプランのAPIはアプリ内（Settings → API）で提供されている可能性が高い
- 公開ドキュメントではなく、ログイン後のダッシュボードにSwagger/OpenAPI仕様がある形式か

**にっしーへの依頼:**
- tl;dvにログインして Settings → API or Integrations → API を確認
- APIキー発行画面があればキーを発行
- APIドキュメント（エンドポイント一覧）のスクショ or URL を共有してほしい

## tl;dv 追加調査 (2026-03-05 08:00)

**Zapier連携ページ:** `tldv.io/integrations/zapier/` → 404（ページ削除済み）
**連携一覧ページ:** 5,000+アプリ連携を謳うがZapier専用ページなし
**対応確認済みの連携先:** Salesforce, Notion, ClickUp, HubSpot, Google Meet等（ネイティブ連携中心）

**仮説更新:**
- tl;dvはZapier連携を廃止 or ネイティブ連携に移行した可能性あり
- Zapier経由ではなく、tl;dvネイティブの「Webhook」or「API」で直接Obsidianに流す設計に変更すべきかもしれない
- にっしーにtl;dvダッシュボードで確認してほしい項目を追加:
  1. Settings → Integrations に Zapier があるか
  2. Settings → API / Webhooks があるか
  3. あればスクショ共有
