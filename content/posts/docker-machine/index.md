---
title: "【Docker初心者向け】Docker Machineとは？"
url: "/server/docker-machine/"
date: 2020-03-07
categories: 
  - "docker"
  - "server"
---

Docker MachineとはDocker（Dockerホスト）の実行させる仮想環境を管理するツール。

当初、DockerはLinux環境でしか使えなかった。そのため、MacやWindowsではVirtualBoxで仮想環境を構築しその上にDockerを動作させていた。

しかし、Docker for MacやDocker for Windowsなどが登場して、単にDockerを利用する際には、Docker Machineは使われなくなった。

使われる場合としては、AWSやGCPといったクラウド環境へDocker環境を構築する際にに使われる。

Docker Machineが便利なところは、**コマンドラインからリモートのクラウド環境下でDockerホストを簡単に作成・管理するができること。**

Docker Machineは使うには、事前にVirtualBoxをインストールしておく。

# Docker Machineの基本操作

#### Dockerホストのリスト表示

```
docker-machine ls
```

#### Dockerホストの作成・起動

「--driver」で仮想化のドライバーとしてvirtualboxを使用し名前をdefaultという名のDockerホストを作成する。作成と同時に起動もしてくれる。`--driver virtualbox`は省略可。

```
docker-machine create --driver virtualbox default
```

#### Dockerホストの停止

```
docker-machine stop default
```

#### Dockerホストの起動

```
docker-machine start default
```

#### Dockerホストに接続

作成したDockerホストへ接続する。

まず、接続対象のDockerホストを設定する。`env`コマンドを使う。

```
docker-machine env default
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/hogeyama/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval $(docker-machine env default)
```

いろいろと環境設定がでてくるが、一番下のコマンドを入力すれば自動で設定される。

```
eval $(docker-machine env default)
```

`docker-machine ls`でdockerホストのACTIVEステータスが「\*」になってアクティブになっていること確認。

適当なコンテナを起動する。

```
docker run -d -p 8080:80 nginx
```

`ssh`コマンドでssh接続する。

```
docker-machine ssh default
```

接続できたら、`docker ps -a`などで先程作成したnginxのコンテナがあることが確認できる。

また、DockerホストのIPを確認するには`ip`コマンド確認できる。

```
docker-machine ip default
```

表示されたIPの8080番ポートをつかってブラウザでアクセスすると、nginxデフォルトページが確認できる。

```
http://{IPアドレス}:8080
```

#### Dockerホストの接続対象の解除

接続対象のDockerホストを解除するには、`env`コマンドに`-u`オプションをつける。

```
docker-machine env -u
```

そして、このコマンドで最後に表示されたコマンドを入力することで解除ができる。

```
eval $(docker-machine env -u)
```

`docker-machine ls`でdockerホストのACTIVEステータスが「-」になっていることを確認。
