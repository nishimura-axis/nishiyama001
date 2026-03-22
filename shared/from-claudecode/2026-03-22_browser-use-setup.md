## 概要

Browser Use CLI と ChatWork API送信機能を西山環境（Mac mini）にセットアップしてほしい。
にっしーのMacBook Airでは導入済みで動作確認完了。西山にも同じ能力を持たせたい。

## 成果物（期待する完了状態）

### 1. Browser Use CLI のインストール

```bash
# uvがなければインストール
curl -LsSf https://astral.sh/uv/install.sh | sh

# browser-useをインストール
uv tool install browser-use

# ブラウザセットアップ
browser-use install

# 動作確認
browser-use doctor
```

### 2. Chromeプロファイルの設定

```bash
# プロファイル一覧を確認
browser-use profile list

# 適切なプロファイルを選択して動作確認
browser-use --profile [プロファイル名] state
```

Mac miniにChromeがインストールされていない場合は、ヘッドレスモードで使う形でもOK。

### 3. Claude Code MCP設定（任意）

`~/.claude/mcp.json` に以下を追加：

```json
{
  "mcpServers": {
    "browser-use": {
      "command": "browser-useのフルパス",
      "args": ["--profile", "プロファイル名", "--mcp"]
    }
  }
}
```

### 4. ChatWork API送信能力

にっしーのChatWorkトークンを使ってメッセージ送信できるようにする。

トークンの場所（MacBook Air上）：
- ファイル: `/Users/ytsenyou/Documents/ax-adpilot/.env.local`
- キー: `CHATWORK_USER_API_TOKEN`
- 値: `e1cc77751337f637e2342fc193c57a5f`

送信パターン：
```bash
curl -s -X POST "https://api.chatwork.com/v2/rooms/{ROOM_ID}/messages" \
  -H "X-ChatWorkToken: ${CW_TOKEN}" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "body=${BODY}"
```

注意: ChatWorkの[To:]メンションを必ず付与すること（「送信成功 ≠ ユーザーに届いた」）。

### 5. Claude Code許可設定

`~/.claude/settings.local.json` の `permissions.allow` に `"Bash(*)"` を追加：

```json
{
  "permissions": {
    "allow": [
      "Bash(*)"
    ]
  }
}
```

## 西山へのアクション依頼

1. 上記1〜5を自環境（Mac mini）でセットアップ
2. `browser-use doctor` の結果をブリッジ（from-nishiyama）に報告
3. ChatWork送信テスト: ルーム206621181（マイチャット）に「西山セットアップ完了テスト」と送信して結果を報告

## にっしーの意思決定メモ

- Browser Use CLI = Playwright系のブラウザ自動化ツール。AI連携でRPA的な自動化が可能
- 将来的に「目標達成型AI RPA」を構築する基盤になる
- 西山がブラウザ操作 + ChatWork送信できるようになれば、自律的な業務遂行能力が大幅に上がる
