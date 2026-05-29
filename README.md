# SDD Spec Kit Test

Next.js + FastAPI + Supabase による仕様駆動開発（Spec-Driven Development）のサンプルプロジェクト。

## 技術スタック

| レイヤー | 技術 |
|---|---|
| フロントエンド | Next.js 15 (TypeScript / Tailwind CSS) |
| バックエンド | Python / FastAPI |
| データベース | Supabase（クラウド） |
| 仮想環境 | Docker / Docker Compose |
| 仕様管理 | spec-kit (`specify` CLI) |
| AI コーディング | Claude Code |

## 前提条件

- Docker / Docker Compose
- Python 3.12+
- Node.js 22+（Next.js 初期化時のみ）
- Git

## セットアップ

### 1. リポジトリのクローン

```bash
git clone <repository-url>
cd sdd-spec-kit-test
```

### 2. 環境変数の設定

```bash
cp .env.example .env
```

`.env` を編集して Supabase の情報を入力します：

```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

Supabase のキーは [Supabase Dashboard](https://supabase.com/dashboard) の **Project Settings > API** から取得できます。

### 3. Next.js アプリの初期化

Docker コンテナ内で `create-next-app` を実行します（ホスト環境に Node.js がなくても OK）：

```bash
docker run --rm -it \
  -v $(pwd)/frontend:/app \
  -w /app \
  node:22-alpine \
  npx create-next-app@latest . \
    --typescript --tailwind --app --src-dir \
    --import-alias "@/*" --yes
```

### 4. Docker Compose で起動

```bash
docker compose up --build
```

| サービス | URL |
|---|---|
| フロントエンド | http://localhost:3000 |
| バックエンド API | http://localhost:8000 |
| Swagger UI | http://localhost:8000/docs |

### 5. spec-kit のインストール

spec-kit はホスト環境にインストールする開発ツールです。

```bash
pip install "git+https://github.com/github/spec-kit.git"
```

インストール確認：

```bash
specify --version
```

### 6. spec-kit の初期化

```bash
specify init .
```

対話式のセットアップが始まり、プロジェクトの仕様ドキュメント（`.speckit/`）が生成されます。

## プロジェクト構成

```
.
├── docker-compose.yml
├── .env.example
├── .gitignore
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── main.py            # FastAPI エントリーポイント
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── src/               # create-next-app 実行後に生成
└── .speckit/              # specify init 実行後に生成
    ├── spec.md            # 仕様書
    ├── plan.md            # 実装計画
    └── tasks/             # タスク一覧
```

## 仕様駆動開発（SDD）ワークフロー

spec-kit + Claude Code を使った開発フローです。

```
仕様を書く (spec.md)
    ↓
実装計画を作る (plan.md)
    ↓
タスクに分解する (tasks/)
    ↓
Claude Code で実装する
```

### 基本コマンド

```bash
# 仕様を確認・更新
specify spec

# 実装計画を生成
specify plan

# タスク一覧を表示
specify tasks

# Claude Code との統合確認
specify integration list
```

## 開発

### バックエンドのみ再起動

```bash
docker compose restart backend
```

### ログの確認

```bash
docker compose logs -f
```

### コンテナ停止

```bash
docker compose down
```
