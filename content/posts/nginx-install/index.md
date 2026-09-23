---
title: "【さくらVPS】Nginxのインストール手順"
url: "/server/linux/nginx-install/"
date: 2020-01-02
categories: 
  - "linux"
---

さくらVPSでWebサーバーであるNginxをインストールする手順を紹介する。

OSのバージョンはCentOS7。

以前の記事で書いた[【さくらVPS】LAMP環境構築手順](https://blog.pop-web.net/programming/vps-lamp/)の[SSHのセキュリティ設定](https://blog.pop-web.net/programming/vps-lamp/#SSH-2)まで終わっている前提で進める。

## 一般ユーザでログインしておく（鍵認証）

```
ssh -p 45454 -i ~/.ssh/id_rsa user@xxx.xxx.xxx.xxx
```

## 80番ポートと443番ポートをあける

ファイアウォールが起動していなければ、起動しておく。

```
sudo systemctl start firewalld
```

http接続に使う80番ポートと、https接続で使う443番ポートがファイアウォールで閉じられいるので、開ける設定をします。

```
sudo firewall-cmd --add-service=http --zone=public --permanent
sudo firewall-cmd --add-service=https --zone=public --permanent
```

firewalldを再起動します。

```
sudo systemctl restart firewalld
```

設定の確認をします。

```
sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services: dhcpv6-client http https ssh-45454
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

services:にhttpとhttps追加されていることを確認。

## Nginxインストール

[Nginx公式サイト](http://nginx.org/en/linux_packages.html#RHEL-CentOS)を参考にインストール。

\[nginx-stable\]版と\[nginx-mainline\]版があるが、\[nginx-mainline\]が推奨されているのでこちらをインストールする。

NginxをインストールするにはNginxのレポジトリファイルを追加する必要がある。レポジトリファイル群は「etc/yum.repos.d/」にある。

まずは、現状のレポジトリを確認

```
ls /etc/yum.repos.d/
CentOS-Base.repo       CentOS-Media.repo    CentOS-fasttrack.repo
CentOS-CR.repo         CentOS-Sources.repo  epel-testing.repo
CentOS-Debuginfo.repo  CentOS-Vault.repo    epel.repo
```

ここにNginxのレポジトリを追加する。

### レポジトリファイルの追加

Nginx用のレポジトリファイルの「nginx.repo」を以下のコマンドで作成・編集していく。

```
sudo vi /etc/yum.repos.d/nginx.repo
```

ファイルの内容は、[Nginx公式のレポジトリ作成ページ](http://nginx.org/en/linux_packages.html#RHEL-CentOS)を参考に作成する。

「baseurl」箇所を環境に合わせて変更してファイルを作成する。

今回はCentOS7なので、以下ようになる。

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```

上記を「nginx.repo」へコピペ して保存

### Nginxパッケージのインストール

Nginxパッケージをインストールする。

```
sudo yum -y install nginx
```

インストールが始まり、最後に「完了しました!」が出ればOK。

### Nginxの起動

以下のコマンドでNginxを起動

```
sudo systemctl start nginx
```

起動しているかステータスを確認

```
sudo systemctl status nginx
```

「Active: active (running)」の表記があれば正常の起動していることが確認できる。

### 自動起動の設定

サーバー再起動時にも、Nginxも起動するように自動起動を設定する。

```
sudo systemctl enable nginx
```

自動起動の設定がなされているかは、以下のコマンドで確認できる。

「enabled」とでれば自動起動設定がされている。

```
sudo systemctl is-enabled nginx
enabled
```

### Nginxのデフォルトページをブラウザで確認

WebブラウザでNginxのデフォルトページを確認します。

さくらのVPSで契約しているIPアドレスをWebブラウザへ直接入力して表示確認。

『Welcome to nginx!』のページが表示されればOK。

次は、[Nginxでのバーチャルホストの設定](https://blog.pop-web.net/programming/nginx-virtualhost/)をやっていきます。
