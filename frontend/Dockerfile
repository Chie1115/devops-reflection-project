# --- ステージ1: ビルド環境 ---
# Node.js 20 の軽量な公式イメージをベースにする
FROM node:20-alpine AS builder

# 作業ディレクトリを設定する
WORKDIR /app

# 最初に package.json と package-lock.json をコピーする
# (これにより、依存関係のキャッシュが効きやすくなる)
COPY package*.json ./

# 依存関係をインストールする
RUN npm ci

# プロジェクトの全てのファイルをコピーする
COPY . .

# アプリケーションをビルドする
RUN npm run build


# --- ステージ2: 本番環境 ---
# Webサーバー(nginx)の軽量な公式イメージをベースにする
FROM nginx:alpine

# ビルドステージから、ビルドされた静的ファイル(/app/dist)を、
# nginxが公開するフォルダ(/usr/share/nginx/html)にコピーする
COPY --from=builder /app/dist /usr/share/nginx/html

# コンテナが80番ポートを公開することを知らせる
EXPOSE 80

# nginxサーバーを起動する
CMD ["nginx", "-g", "daemon off;"]

