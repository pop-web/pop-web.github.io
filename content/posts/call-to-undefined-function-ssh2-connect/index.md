---
title: "Call to undefined function ssh2_connect()のエラー。ssh2インストールするまでの手順"
url: "/programming/php/call-to-undefined-function-ssh2-connect/"
date: 2019-07-19
categories: 
  - "php"
  - "programming"
---

```
Call to undefined function ssh2_connect()
```

PHPでssh2\_connect()を使用したら、上記のエラーがでたのでssh2インストール手順をメモる。

環境としては、VagrantでCentOSを入れて、PHP5.4をインストール。

そもそも、ssh2インストールするには、

**peclコマンドでssh2をインストールする必要がある。**

## **peclのインストール**

peclを導入するために、Pearが必要

```
sudo yum install php54w-pear
```

しかし、上記コマンドでいれようとするとエラー

```
sudo yum install -y --enablerepo=remi,remi-php54 php-pear
```

これで対応。

peclが使用できることを確認します。

```
pecl
```

## **ssh2をインストール**

必要なものをインストール

```
sudo yum -y install libssh2 libssh2-devel
```

これで、やっとssh2をインストールできる。

```
sudo pecl install -f ssh2
```

モジュールが追加されているかを確認

```
ll /usr/lib64/php/modules/ | grep ssh2
-rw-r--r--. 1 root root  273672 Jul  3 05:48 ssh2.so
```

## php.iniへの追記

最後に、php.iniへ「extension=ssh2.so」を追記（最終行でOK）

## 参考サイト

- [https://t0463.blogspot.com/2016/05/centos7ssh2connectwo.html](https://t0463.blogspot.com/2016/05/centos7ssh2connectwo.html)
- [https://oc-technote.com/linuxサーバー/centos7%E3%80%80peclを動作させるための長い道のり/](https://oc-technote.com/linuxサーバー/centos7%E3%80%80peclを動作させるための長い道のり/)
