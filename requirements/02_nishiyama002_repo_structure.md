# nishiyama002 リポジトリ構成

## リポジトリ: nishimura-axis/nishiyama002

```
nishiyama002/
├── .github/
│   └── workflows/
│       └── deploy.yml          # Vercel自動デプロイ（push to main）
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── layout.tsx          # ルートレイアウト（ダークモード対応）
│   │   ├── page.tsx            # ダッシュボードTOP（全影分身一覧）
│   │   ├── agents/
│   │   │   └── [agentId]/
│   │   │       └── page.tsx    # 個別エージェント詳細
│   │   ├── tasks/
│   │   │   └── page.tsx        # タスクボード（全エージェント横断）
│   │   ├── memory/
│   │   │   └── page.tsx        # メモリ閲覧（全エージェント横断）
│   │   └── api/
│   │       ├── agents/
│   │       │   └── route.ts    # エージェント一覧API
│   │       ├── sessions/
│   │       │   └── route.ts    # セッション情報API
│   │       ├── tasks/
│   │       │   └── route.ts    # タスク情報API
│   │       ├── commits/
│   │       │   └── route.ts    # GitHubコミットAPI
│   │       └── gateway/
│   │           └── route.ts    # OpenClaw Gateway proxy
│   ├── components/
│   │   ├── ui/                 # shadcn/ui コンポーネント
│   │   ├── agent-card.tsx      # エージェントカード（アバター+ステータス）
│   │   ├── task-board.tsx      # タスクボード
│   │   ├── activity-feed.tsx   # アクティビティフィード
│   │   ├── thinking-log.tsx    # 思考ログビューア
│   │   ├── commit-timeline.tsx # コミットタイムライン
│   │   ├── memory-viewer.tsx   # メモリビューア
│   │   └── status-indicator.tsx # ステータスインジケーター
│   ├── lib/
│   │   ├── openclaw.ts         # OpenClaw Gateway API クライアント
│   │   ├── github.ts           # GitHub API クライアント
│   │   ├── workspace.ts        # ワークスペースファイル読み取り
│   │   └── types.ts            # 型定義
│   └── hooks/
│       ├── use-agents.ts       # エージェント一覧hook
│       ├── use-sessions.ts     # セッション情報hook
│       └── use-realtime.ts     # リアルタイム更新hook
├── public/
│   └── avatars/                # エージェントアバター画像
├── drizzle/                    # DB schema（必要に応じて）
├── package.json
├── tsconfig.json
├── tailwind.config.ts
├── next.config.ts
├── .env.example
├── .env.local                  # (gitignore)
└── README.md
```

## 環境変数

```env
# OpenClaw Gateway
OPENCLAW_GATEWAY_URL=http://localhost:18789
OPENCLAW_GATEWAY_TOKEN=xxx

# GitHub
GITHUB_TOKEN=xxx
GITHUB_ORG=nishimura-axis

# Neon（必要に応じて）
DATABASE_URL=xxx

# Vercel
VERCEL_URL=xxx
```

## 依存パッケージ（主要）

```json
{
  "dependencies": {
    "next": "^15.x",
    "react": "^19.x",
    "tailwindcss": "^4.x",
    "@radix-ui/react-*": "latest",
    "lucide-react": "latest",
    "swr": "^2.x",
    "date-fns": "^4.x",
    "sonner": "^2.x",
    "next-themes": "latest",
    "framer-motion": "latest",
    "ws": "^8.x"
  }
}
```

## デプロイ先
- Vercel Pro（にっしーの既存アカウント）
- ドメイン: 要検討（例: dashboard.axad.jp or nishiyama.vercel.app）
