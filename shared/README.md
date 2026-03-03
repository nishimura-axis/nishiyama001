# shared/ — 西山 ↔ Claude Code 連携フォルダ

## 構造
- `to-claudecode/` — 西山がClaude Codeに渡したいファイル
- `from-claudecode/` — Claude Codeが西山に渡したいファイル

## 使い方

### 西山 → Claude Code
1. 西山が `to-claudecode/` にファイルを作成・git push
2. にっしーがClaude Codeに以下を送る:
   ```
   ~/nishiyama001/shared/to-claudecode/ に新しいファイルがある。git pullして読んで、指示に従って。
   ```

### Claude Code → 西山
1. Claude Codeが作業完了後、出力を `from-claudecode/` に保存・git push
2. にっしーが西山に「Claude Codeから出力来たよ」と伝える（または西山が定期チェック）

## ルール
- ファイル名は `YYYY-MM-DD_目的.md` 形式
- 各ファイルの冒頭に `## 目的` `## 期待するアクション` を記載
- 処理済みファイルは `done/` サブフォルダに移動
