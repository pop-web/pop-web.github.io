---
title: "Windows10 + VirtualBox + VagrantでLAMP開発環境手順（2）"
url: "/server/vagrant/local-development-environment-2/"
date: 2019-05-26
categories: 
  - "vagrant"
---

[Windows10 + VirtualBox + VagrantでLAMP開発環境手順（1）](https://blog.popweb.dev/server/vagrant/local-development-environment/)の続き

vagrantでssh接続できたところから。

```
[vagrant@localhost ~]$
```

これからLAMP環境を構築していく

- **Apache**
- **PHP7**
- **MySQL5.7**

## rootユーザーになる

```
su
```

パスワードは「vagrant」。

以下のように、「#」が先頭につけばOK

```
[root@localhost vagrant]#
```

## SELINUXの無効化

SELINUXはセキュリティの為、デフォルトで有効になっていますが、取り扱いが難しいので、ここではオフにします。

cofigファイルを開いて、編集する。

```
vi /etc/selinux/config
```

以下の箇所を変更

```
SELINUX=enforcing
↓
SELINUX=disabled
```

適用させるために、CentOS7を再起動させます。

```
reboot
```

再度、vagrant sshでssh接続してrootユーザになります。

getenforceと入力して、Disableと出ればSELINUXが無効化されています。

```
[root@localhost vagrant]# getenforce
Disabled
```

## Apacheインストール

```
yum -y install httpd
```

Apacheの起動

```
systemctl start httpd
```

## MySQLのインストール

CentOS7で、デフォルトで入っているMariaDBをまずは削除する

```
yum -y remove mariadb-libs.x86_64
```

リポジトリを追加して、インストールする。

```
yum -y install http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
yum -y install mysql-community-server
```

MySQLの起動

```
systemctl start mysqld
```

## PHPのインストール

まずは、リポジトリを追加する。

```
yum -y install epel-release
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```

PHPをもろもろインストールする。

```
yum -y install --enablerepo=remi,epel,remi-php70 php php-devel php-intl php-mbstring php-pdo php-gd php-mysqlnd
```

## ブラウザで確認

[http://192.168.33.10/](http://192.168.33.10/)へアクセスする。

Apacheのテストページが表示されればOK
