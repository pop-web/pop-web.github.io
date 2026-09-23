---
title: "SSH接続でWARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!が出たときの対処法"
url: "/server/linux/warning-remote-host-identification-has-changed/"
date: 2019-06-14
categories: 
  - "linux"
---

SSH接続しようとしたら、「WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!」っていうのが出た。

## エラーの原因

サーバのOSを再インストールしたときなどによく出る。

SSH接続するときに安全に接続するため、接続先のサーバー情報をクラインと側に保存する。その保存情報が以前接続した情報と一致しない場合は接続エラーになる。

## 解決方法

保存される情報は、「known\_hosts」に保存されるので、この中の接続したいサーバ情報を削除してあげればいい。

削除するコマンドは以下

```
ssh-keygen -R ホスト名
```
