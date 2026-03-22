# Telegram API情報（西森→西山直接通信用）

## Bot Token
`8454231400:AAFaRVyy2YcXkhjIDaaCBJfOEWy2Rp0DWGM`

## Chat ID（にっしーとのチャット）
`8763235865`

## 使い方
```bash
curl -s -X POST "https://api.telegram.org/bot8454231400:AAFaRVyy2YcXkhjIDaaCBJfOEWy2Rp0DWGM/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": 8763235865, "text": "西森からのメッセージ"}'
```

## 注意
- このtokenで送ったメッセージはにっしーに「西山1号」として届く
- 送信時は冒頭に「【西森】」等の識別子を付けることを推奨
- 西山のbot tokenなので取り扱い注意
