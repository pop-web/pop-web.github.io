---
title: "Nuxt.js（SSRモード）をDocker Composeでdeployする"
url: "/programming/vue-js/nuxt-js-ssr-docker-compose-deploy/"
date: 2020-04-03
categories: 
  - "vue-js"
  - "programming"
---

Nuxtプロジェクト（SSRモード）をDocker Composeを使って起動させる方法を説明する。

今回は、Nuxtプロジェクト直下に`Dockerfile`と`docker-compose.yml`を置く。

まずは、Dockerfileから。

## Dockerfile

```
FROM node:12.16.1-alpine

EXPOSE 3000
ENV NUXT_HOST=0.0.0.0

RUN mkdir -p /nuxt-app
WORKDIR /nuxt-app

RUN apk update && \
    apk upgrade && \
    apk add git && \
    apk add curl

COPY package*.json ./

RUN npm install
```

必要はないが、gitやcurlもとりあえずインストールする。

Dockerは命令文を実行するたびにイメージレイヤーを作成してしまうため、RUN命令は、&&でつなげた方が良い。

さらに、`\` を使って複数行にして見やすくした方が良い。

Dockerコンテナ内部に「nuxt-app」ディレクトリを作成してそこでNuxtプロジェクトを動かしていく。

まず、ローカルにある`package*.json`をコンテナにコピーして、`npm install`必要なパッケージをインストールする。

次に、`docker-compose.yml`を作成する。

## docker-compose.yml

```
version: "3"
services:
  frontend:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    volumes:
      - /nuxt-app/node_modules
      - .:/nuxt-app
    command: ["npm","run","dev"]
    environment:
      - NODE_ENV=development
```

Dockerコンテナを起動した際に、`npm run dev`でNuxtアプリを起動するようにする。

## .dockerignore

イメージに含めたくないファイルは`.dockerignore`に書く。

ビルドには不必要なファイルを除外することで、ビルド時間の短縮になる。

```
.git
node_modules
.nuxt
```

## Dockerコンテナの起動

最後に、Dockerコンテナをビルドして起動する。

```
docker-compose up -d --build
```

コンテナが起動したら、ブラウザで`localhost:3000`にアクセス。

これで、Nuxtプロジェクトが動作していればOK。
