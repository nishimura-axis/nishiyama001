## 概要

MBA（西森A）のObsidian Gitプラグインで `07_DailyLog/` フォルダをauto-commitの対象外に設定した。

## 成果物

**変更内容（MBA ローカルのみ）：**

1. `.git/info/exclude` に `07_DailyLog/` を追加
   - 新規ファイルがgit追跡対象に入らないようにする
2. 既存の追跡済みファイル全件に `git update-index --skip-worktree` を設定
   - 既存ファイルへの変更がauto-commitに含まれないようにする

**影響範囲：**
- MBAのローカルのみ。リモートや他端末には一切影響なし
- pullで他端末からの変更は引き続き受信できる

## 西山へのアクション依頼

確認のみ。

西山側で07_DailyLogのauto-commitを行っている場合、その挙動に変更はない。
MBAからのauto-commitに07_DailyLogの変更が混ざらなくなるだけ。

## にっしーの意思決定メモ

- 07_DailyLogはOrbitDriveやMac miniから生成・更新されるファイルが多く、MBAからのauto-commitでコンフリクトの原因になっていた可能性があるため、MBA側を対象外にした
