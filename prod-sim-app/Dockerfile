# Node.js 20 LTS (Debian Bullseye ベース) を使用
# 理由: Next.js開発に必要なNode.jsとnpmが標準で含まれ、軽量で安定している
FROM node:20-bullseye

# 作業ディレクトリを設定
WORKDIR /usr/src/app

# 基本ツールをインストール（git など）
RUN apt-get update && \
    apt-get install -y git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 開発用なので、ソースコードはホストからマウントする前提
# COPY は最小限（package.json など必要最小限のファイルのみ）
# 実際の開発では docker-compose.yml でボリュームマウントを使用

# デフォルトコマンド（docker-compose.yml で上書き可能）
CMD ["tail", "-f", "/dev/null"]
