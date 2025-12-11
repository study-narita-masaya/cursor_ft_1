# 開発ガイド

## プロジェクト構成

- **ホスト OS**: Windows
- **コンテナ**: Ubuntu (Debian Bullseye) ベース
- **フレームワーク**: Next.js 16 (App Router) + TypeScript
- **スタイリング**: Tailwind CSS

## 開発環境の起動

### 1. コンテナの起動

```powershell
cd prod-sim-app
docker compose up -d
```

### 2. コンテナのターミナルに入る

コンテナ内でコマンドを実行するために、まずターミナルに入ります：

```powershell
docker compose exec web bash
```

これでコンテナ内の `/usr/src/app` ディレクトリでシェルが開きます。
コンテナ内で作業を終える場合は `exit` と入力して抜けられます。

### 3. 開発サーバーの起動

コンテナ内で開発サーバーを起動します：

```powershell
docker compose exec web bash -c "cd /usr/src/app && npm run dev"
```

または、バックグラウンドで起動：

```powershell
docker exec -d prod-sim-app-web bash -c "cd /usr/src/app && npm run dev"
```

### 4. ブラウザで確認

Windows側のブラウザから `http://localhost:3000` にアクセスしてください。

## 開発方法の選択肢

### 方法1: ホスト側でファイル編集 + コンテナで実行（推奨）

**メリット:**
- Cursorの機能をフルに活用できる
- ファイル編集が高速
- Git操作が簡単
- ホスト側のエディタ拡張機能が使える

**デメリット:**
- コンテナ内でコマンド実行する際は `docker compose exec` が必要

**使い方:**
1. Cursorで `prod-sim-app/` ディレクトリを開く
2. ホスト側でファイルを編集（`src/app/page.tsx` など）
3. 変更は自動的にコンテナに反映され、Next.jsのホットリロードで更新される
4. パッケージのインストールなどは `docker compose exec web npm install <package>` で実行

### 方法2: Dev Container / Remote 開発

**メリット:**
- コンテナ内で直接開発できる
- ターミナル操作が簡単
- 環境の一貫性が高い

**デメリット:**
- CursorのDev Container機能の設定が必要
- 初期セットアップがやや複雑

**設定方法:**
1. `.devcontainer/devcontainer.json` を作成
2. Cursorで「Reopen in Container」を選択
3. コンテナ内で直接開発

## よく使うコマンド

### コンテナのターミナルに入る

コンテナ内で対話的にコマンドを実行する場合：

```powershell
docker compose exec web bash
```

または、コンテナ名を直接指定する場合：

```powershell
docker exec -it prod-sim-app-web bash
```

コンテナ内の作業ディレクトリ `/usr/src/app` でシェルが開きます。
作業を終える場合は `exit` と入力して抜けられます。

### パッケージのインストール

```powershell
docker compose exec web npm install <package-name>
```

### ビルド

```powershell
docker compose exec web npm run build
```

### ログの確認

```powershell
docker logs prod-sim-app-web
```

### コンテナの停止

```powershell
docker compose down
```

### コンテナの再ビルド

```powershell
docker compose build --no-cache
docker compose up -d
```

## プロジェクト構造

```
prod-sim-app/
├── Dockerfile              # コンテナイメージ定義
├── docker-compose.yml      # Docker Compose設定
├── package.json            # Node.js依存関係
├── tsconfig.json           # TypeScript設定
├── next.config.ts          # Next.js設定
├── src/
│   └── app/                # App Router（ページとレイアウト）
│       ├── layout.tsx      # ルートレイアウト
│       ├── page.tsx        # ホームページ
│       └── globals.css     # グローバルスタイル
└── public/                 # 静的ファイル
```

## 次のステップ

生産シミュレーション結果の可視化アプリの開発を開始する準備が整いました。

1. **グラフ表示**: Chart.js や Recharts などのライブラリを追加
2. **ガントチャート**: react-gantt-chart や dhtmlx-gantt などを検討
3. **テーブル表示**: TanStack Table (React Table) などを検討

必要なパッケージは `docker compose exec web npm install <package>` でインストールしてください。
