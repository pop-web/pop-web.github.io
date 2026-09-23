---
title: "【Docker初心者向け】Dockerとは？Nginxサーバ構築するまで"
url: "/server/docker-nginx/"
date: 2020-02-29
categories: 
  - "docker"
  - "server"
---

# Dockerとは

Dockerとは、コンテナ型の仮想環境を作成できるもの。ちなみにGo言語で書かれている

ミドルウェアのイストールや各種設定をコード化して管理しており、共有しやすいという利点がある。

## Dockerイメージとは

コンテナを実行するために必要なファイル。

**レイヤー構造で構成されており読み取り専用となっていおり、レイヤーはコマンドを実行するたびに積み重ねられる。**

Dockerイメージをもとにコンテナを起動すると、コンテナレイヤーというレイヤーができる。コンテナレイヤーは読み書き可能なレイヤーとなっている。

このコンテナレイヤーで過去のレイヤーで追加したファイルを削除することもできるが、**履歴として過去のレイヤーにファイルは残り続ける。**

## Dockerコンテナとは

**DockerコンテナとはDockerイメージを元に作成される仮想環境。**

基本的にDockerコンテナは停止した後も停止ステータスとして残りつづける。

## Docker Hubとは

[Docker Hub](https://hub.docker.com/)とは、Dockerイメージを公開しているレジストリサービス

# Dockerのインストール

[公式サイト](https://www.docker.com/)でDockerをインストールします。

## コンテナの起動してみる

Hello woldというメッセージを表示する簡単なイメージを動かす。

```
docker run hello-world
```

docker run のコマンドは、以下の3つのコマンドを一度に実行するコマンドである。

- docker pull ：イメージの取得
- docker create ：コンテナの作成
- docker start ：コンテナの起動

ローカルPCに常駐しているDockerデーモンがhello-worldというイメージを探しに行く。

ローカルPC上にイメージがない場合、DockerデーモンはDocker Hubにイメージを探しに行く（docker pull）

イメージをダウンロードして作成(docker create)、イメージからコンテナ起動する(docker start)

すでに、ローカルPC上にイメージが存在している場合は、そのイメージからコンテナを起動するので処理は早くなる。

# Dockerイメージの基本操作

ローカルにあるイメージを確認

```
docker images
```

ローカル上にあるイメージにタグ付けし、新しいイメージ名でイメージを作成したい場合

```
docker tag {ローカルにあるイメージ名} {新しいイメージ名}:{タグ名}
```

イメージの詳細情報

```
docker inspect {イメージ名}
```

イメージの削除

```
docker rmi {イメージ名}
```

イメージの強制削除の場合、-fを付ける

```
docker rmi -f {イメージ名}
```

## イメージの作成

イメージを作成するためには、まず作業ディレクトリ内に**Dockerfile**という名のファイルを作成する。

Dockerfileからイメージを作ることをイメージビルドという。

まず、Dockerfileを編集する。

各行へ「命令 引数」といった書式で書いていく。

そして、各命令ごとに新しいレイヤーが追加されていく。

下記の命令ではwhalesayイメージへ新たに3つのレイヤーが追加されたこととなる。

```
FROM docker/whalesay
RUN apt-get -y update && apt-get install -y fortunes
CMD /usr/games/fortune | cowsay
```

各命令について説明する。

### FROM

イメージを作成する際のベースとなるイメージを指定する命令。今回はwhalesayイメージというコンソールでクジラが話しているアスキーアートを表示するイメージ。whalesayイメージは、Linux ディストリビューションのUbuntuがイメージのベースとなっている。

### RUN

イメージビルド時に実行するコマンドを指定する命令。

### CMD

コンテナ起動時に実行するコマンドを指定する命令。

## Dockerfileからイメージビルド

Dockerfileからイメージをビルドするには以下のコマンドを実行。

```
docker build -t test-build .
```

\-tオプションは、ビルドしたイメージに名前を付けるオプション。

最後のピリオドは、ビルドコンテキスト指定でイメージを作成する際のファイルなどの範囲をあわらす。

ピリオドの場合は、カレントディレクトリの意味。

ビルドを実行したら、`docker images`でイメージを確認できる。

キャッシュを読み込みたくない時は、--no-cacheオプションを使用する。

```
docker build --no-cache -t test-build .
```

## イメージからコンテナの実行

イメージからコンテナを実行。

```
docker run test-build
```

実行すると、クジラのアスキーアートの吹き出しに名言が表示されて、実行するたびにその名言が変わるのが確認できる。

# NginxイメージでWebサーバーをたてる

NginxイメージでWebサーバーたてていく。

DcokerHubのメニューの「Explore」から検索に行き、「nginx」と検索してNginxのイメージを見つける。

Nginxイメージの説明の中のExposing external portにある以下のコマンドを参考にする。

```
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

\--nameオプションは起動するコンテナ名に名前をつけるオプション。

**\-dオプションはデタッチモードのことで、コンテナをバックグラウンドで起動するオプション。**

**\-pオプションはホストとコンテナのポートマッピングを設定するオプション。ここでは、8080はホストマシンが外部から受けるポート番号。80はコンテナにマッピングされるポート番号。**

ローカルでコンテナ起動している場合、localhostの8080番ポートにアクセスすればコンテナの80番ポートに転送される。

## Nginxコンテナの起動

以下のコマンドでnginxコンテナの起動。

```
docker run --name test-nginx -d -p 8080:80 nginx
```

ブラウザでlocalhostの8080番ポートにアクセスし確認。

```
http://localhost:8080/
```

Nginxのデフォルトページが確認できればOK。

## コンテナ停止とともにコンテナを削除

**Dockerのコンテナは、コンテナが停止しても停止ステータスとして残り続ける。**

コンテナが停止した時点でコンテナを削除したい場合は、--rmオプションを付ける。

```
docker run --name test-nginx --rm -d nginx
```

# ディレクトリのマウント

ホスト側のディレクトリをコンテナ側でマウントするには、DockerHubのNginxイメージの説明の中のHosting some simple static contentにある以下のコマンドを参考にする。

```
docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
```

\-vオプション部分のコロン区切りになっているところでマウントするティレクトリを指定する。

**はじめのコロンまでがホスト側のディレクトリのパスで次がコンテナ側ディレクトリのパス。**最後のroはReadOnlyの略で、読み取り専用でマウントするオプション指定。

## マウントしてNginxコンテナの起動

以下のコマンドでマウントしてnginxコンテナの起動。

```
docker run --name mount-nginx -v /Users/user/mount-test/html:/usr/share/nginx/html:ro -d -p 8080:80 nginx
```

ホスト側のディレクトリ（/Users/user/mount-test/html）は任意で作成して、index.htmlなどを配置しておく。

ブラウザでlocalhostの8080番ポートにアクセスし確認。

```
http://localhost:8080/index.html
```

index.htmlの内容が表示されればOK。
