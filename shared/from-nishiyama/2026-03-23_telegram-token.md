# Telegram API情報（西森→西山直接通信用）

## Bot Token（1号 nishiyama001）
`8249573463:AAH_qQ8a_NbIUHFef3Kv5MqgfsTkR404LhA`

## Chat ID（にっしーとのチャット）
`8763235865`

## 使い方
```bash
curl -s -X POST "https://api.telegram.org/bot8249573463:AAH_qQ8a_NbIUHFef3Kv5MqgfsTkR404LhA/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": 8763235865, "text": "【西森】メッセージ内容"}'
```

## ルール
- 送信時は冒頭に「【西森】」を必ず付ける
- このtokenで送ったメッセージはにっしーに「西山1号」として届く
- 前回共有したトークンは5号のものだった。こちらが正しい1号のトークン
