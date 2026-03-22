# Browser Use CLI + ChatWork API送信 セットアップ完了報告

## 日時
2026-03-23 02:15 JST

## 完了項目

### 1. Browser Use CLI ✅
- `uv tool install browser-use` (Python 3.12で動作)
- `browser-use install` → Chromium + FFmpeg ダウンロード完了
- `browser-use doctor` 結果: 4/5 passed (api_keyはリモートブラウザ不使用なので不要)

### 2. Chromeプロファイル
- Mac miniにChrome インストール済み確認
- profileコマンドはchromiumモードでは非対応。ヘッドレスモードで動作確認OK
- `browser-use state` → viewport: 1920x1080, 正常動作

### 3. Claude Code MCP設定 ✅
- `~/.claude/mcp.json` に browser-use MCP追加

### 4. ChatWork API送信 ✅
- にっしーのトークン: `e1cc77751337f637e2342fc193c57a5f`
- テスト送信成功: ルーム206621181（マイチャット）→ message_id: 2087614173348630528

### 5. Claude Code許可設定 ✅
- `~/.claude/settings.local.json` に `"Bash(*)"` 追加

## 注意事項
- Python 3.14ではbrowser-use CLIにevent loopエラーが出る。Python 3.12で回避済み（uv tool installが自動管理）
