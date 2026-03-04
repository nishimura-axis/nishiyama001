# Claude Code側のブリッジスキル構築

## 目的
西山（OpenClawエージェント）との双方向ファイル連携を、Claude Codeの標準ワークフローに組み込む。作業結果を西山に共有する時、にっしーがコピペや手動操作なしで済む仕組みを作る。

## 期待するアクション

### 1. CLAUDE.mdにブリッジルールを追加

以下のルールをCLAUDE.md（またはプロジェクトルールファイル）に追記：

```markdown
## 西山ブリッジ（Agent Bridge）

### 共有フォルダ
~/nishiyama001/shared/
├── to-claudecode/    ← 西山からの指示・コンテキスト（読む）
├── from-claudecode/  ← 俺の出力を西山に渡す（書く）

### ルール
1. **作業完了時**: 西山に共有すべき成果物がある場合、from-claudecode/ にmarkdownで保存
2. **ファイル名**: YYYY-MM-DD_目的.md
3. **ファイル構造**: 必ず以下のヘッダーを含める
   - ## 概要 — 何を作った/やったか
   - ## 成果物 — ファイルパス、コード変更、設計判断等
   - ## 西山へのアクション依頼 — 西山に何をしてほしいか（なければ「確認のみ」）
   - ## にっしーの意思決定メモ — 作業中ににっしーが下した判断があれば記録
4. **git push**: ファイル保存後、必ず実行
   ```
   cd ~/nishiyama001 && git add -A && git commit -m "bridge: [内容の要約]" && git push
   ```
5. **セッション開始時**: to-claudecode/ に未読ファイルがないか確認
   ```
   ls ~/nishiyama001/shared/to-claudecode/*.md 2>/dev/null
   ```
   ファイルがあればgit pullして読み、指示に従う
6. **コンテキスト抽出を依頼された時**: 出力をVaultとfrom-claudecodeの両方に保存

### 自動チェック（推奨）
セッション開始時に以下を実行：
```
cd ~/nishiyama001 && git pull --rebase 2>/dev/null; ls shared/to-claudecode/*.md 2>/dev/null || echo "No pending messages from Nishiyama-1"
```

### 共有すべきもの
- 技術的意思決定（なぜこの実装にしたか）
- 設計変更（DB、API、UI構成の変更）
- にっしーが作業中に語った思想・方針
- エラー/問題の発見と解決方法
- にっしーの判断・フィードバック内容

### 共有不要なもの
- 単純なバグ修正の詳細
- ライブラリアップデート等の定型作業
- にっしーが「これは共有しなくていい」と言ったもの
```

### 2. Obsidianにもログを残す

作業ログを以下にも追記（日付別で差分追記しやすい形式）：

```
~/Documents/Obsidian/MyVault/30_Projects/AXIS年商100億構想/NishimuraBrain/
├── work_log_YYYY-MM.md     （月別の作業ログ。日付セクションで追記）
├── technical_decisions.md   （技術判断の累積。新しい判断を都度追記）
├── design_philosophy.md     （設計思想の累積）
└── meta.md                  （この仕組みの説明）
```

work_log のフォーマット：
```markdown
## YYYY-MM-DD

### [作業テーマ]
**やったこと:** ...
**技術判断:** ...（あれば）
**にっしーの発言/判断:** ...（あれば。原文で残す）
**西山への共有:** あり/なし（ありならfrom-claudecodeにも出力済み）
```

### 3. にっしーへの報告テンプレート

作業完了時、にっしーに以下のフォーマットで報告：
```
完了: [タスク名]
西山に共有: [あり → git push済み / なし]
Vaultログ: [更新済み / 該当なし]
```

## コンテキスト
- 西山 = OpenClawで動いてるAIエージェント（設計専任）
- 西山は ~/nishiyama001 リポにアクセスできる
- 西山はObsidian Vaultも読める（書き込みはにっしー許可制）
- にっしーの手間を最小化することが最優先

## 出力先
- CLAUDE.mdまたはプロジェクトルールファイルに追記
- このファイル自体の処理完了後 → from-claudecode/done/ に移動

## 完了後
```
cd ~/nishiyama001 && git add -A && git commit -m "bridge: claude-code side setup complete" && git push
```
