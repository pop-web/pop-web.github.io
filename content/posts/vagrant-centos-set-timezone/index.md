---
title: "Vagrant、CentOS7環境でタイムゾーンを日本時間するコマンド"
url: "/server/linux/vagrant-centos-set-timezone/"
date: 2019-05-28
categories: 
  - "linux"
---

VagrantでCentOS7をインストールすると、デフォルトでは時間が日本時間になってないのでタイムゾーンの設定をする。

以下、日本時間に設定するコマンドです。

```
timedatectl set-timezone Asia/Tokyo
```

dateコマンドで確認します。

```
$ date
Tue May 28 09:04:09 JST 2019
```

ちゃんと、現在の時刻になっていることが確認できました。
