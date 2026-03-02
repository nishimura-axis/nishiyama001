# 明日のセットアップコマンド一覧

## 前提
- Bot token × 3 が手元にある状態で実行
- いがちのAnthropic APIキーが手元にある
- いがちのGitHub PATが手元にある
- いがちのVaultリポジトリURLがわかっている

---

## 1. macOSユーザー作成（にっしーがGUIで実行）

```
システム設定 → ユーザとグループ → ユーザを追加
  フルネーム: igachi
  アカウント名: igachi
  種類: 標準
  パスワード: 仮で設定（後でいがちが変更）
```

---

## 2. igachiユーザーの環境構築

### igachiユーザーにログイン切替してターミナルを開く

```bash
# Homebrew（Apple Silicon）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# Node.js
brew install node

# OpenClaw
npm install -g openclaw

# OpenClaw初期設定
openclaw onboard
```

### Vaultをclone（GitHub PAT使用）

```bash
mkdir -p ~/Documents/Obsidian
cd ~/Documents/Obsidian
git clone https://<GITHUB_PAT>@github.com/<IGACHIのユーザー名>/<Vaultリポジトリ名>.git MyVault

cd MyVault
git config user.name "igachi-bot"
git config user.email "igachi-bot@axis.co.jp"
```

### OpenClawワークスペース設定

```bash
# ワークスペースをVaultに設定（openclaw.jsonを編集）
# → agents.defaults.workspace を ~/Documents/Obsidian/MyVault に変更
```

### AGENTS.md / SOUL.md 配置

```bash
# nishiyama001/setup/ から igachi用ファイルをコピー
cp /Users/nishiyama/.openclaw/workspace/setup/igachi-agents.md ~/Documents/Obsidian/MyVault/.openclaw/workspace/AGENTS.md
cp /Users/nishiyama/.openclaw/workspace/setup/igachi-soul.md ~/Documents/Obsidian/MyVault/.openclaw/workspace/SOUL.md
```

※ パスはOpenClawのワークスペース設定次第で調整

### Telegram bot設定

```bash
# openclaw.jsonの channels.telegram に追加:
# {
#   "enabled": true,
#   "botToken": "<いがち用BOT_TOKEN>",
#   "dmPolicy": "pairing",
#   "groupPolicy": "allowlist",
#   "streaming": "off"
# }
```

### Anthropic APIキー設定

```bash
# openclaw.jsonの auth.profiles に追加:
# {
#   "anthropic:default": {
#     "provider": "anthropic",
#     "mode": "token"
#   }
# }
# 環境変数 or openclaw auth でAPIキーを設定
```

### Gateway起動

```bash
openclaw gateway start
```

---

## 3. にっしー専用bot設定（nishiyamaユーザーで実行）

```bash
# 新しいエージェント追加
openclaw agents add nishimura-brain

# ワークスペースをにっしーのVaultに設定
# agents.nishimura-brain.workspace = ~/Documents/Obsidian/MyVault

# Telegram bot設定
# channels.telegram にbinding追加

# AGENTS.md / SOUL.md 配置
# setup/nishimura-brain-agents.md → nishimura-brain のワークスペースへ
```

---

## 4. 3号（nishiyama003）設定（nishiyamaユーザーで実行）

```bash
# リポジトリ作成
mkdir -p ~/nishiyama003
cd ~/nishiyama003
git init
# GitHub上にリポジトリ作成してremote追加

# エージェント追加
openclaw agents add nishiyama003

# ワークスペース設定
# agents.nishiyama003.workspace = ~/nishiyama003

# Telegram bot設定
# channels.telegram にbinding追加

# AGENTS.md / SOUL.md / tasks.json 配置
# requirements/05_nishiyama003_dashboard.md をコピー
```

---

## 5. Gateway再起動（全エージェント反映）

```bash
openclaw gateway restart
```

---

## 6. 動作確認

1. いがち: Botにテキスト送信 → 返答確認
2. いがち: 音声送信 → 文字起こし + 返答確認
3. いがち: 「PJ進捗を教えて」→ Vault参照確認
4. にっしー: 専用Botにテキスト送信 → 返答確認
5. 3号: 起動メッセージ確認

---

## 注意事項

- igachiユーザーのOpenClawは**独立したGatewayインスタンス**として動く（ポート番号を分ける必要あり）
- にっしー側のGatewayとigachi側のGatewayは別プロセス
- 実際のopenclaw.jsonの構成はOpenClawのマルチエージェント仕様に合わせて調整が必要
- 当日、不明点が出たらOpenClaw公式ドキュメントを参照
