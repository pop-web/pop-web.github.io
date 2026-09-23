---
title: "Vagrantでローカルとサーバ側のデータを同期する共有フォルダの設定手順"
url: "/server/vagrant/vagrant-shared-folder/"
date: 2019-06-07
categories: 
  - "vagrant"
---

ローカルの作業フォルダとVirtualBoxで作った開発環境内のフォルダを自動で同期する共有フォルダの設定手順です。

VagrantとVirtualBoxがインストールしている前提です。

## ソフトフェアの更新

```
sudo yum -y update
```

kernelがkernel-headerとバージョンが異なると、うまくいかないので確認しておく。

```
sudo yum list installed | grep kernel
```

再起動しておく。

```
reboot
```

パスワードを聞かれたら、vagrantの初期パスワードは「vagrant」なので、それを入力する。

## プラグイン「vbguest」のインストール

vagrantの作業 ディレクトリに移動して、vbguestをインストールします。

```
vagrant plugin install vagrant-vbguest
```

「Installed the plugin 'vagrant-vbguest (x.xx.x)'!」とでればOK。

Vagrantを起動します。

```
vagrant up
```

ステータスの確認します。

```
vagrant vbguest --status
```

「\[default\] GuestAdditions バージョン running --- OK 」とでればOKですが、 「\[default\] No installation found. 」とでるなら、以下のコマンドを入力

```
vagrant vbguest
```

vbguestは、仮想マシン起動時にGuest Additionsのバージョンを自動でアップデートしてしまいます。

そのため、kernelアップデート時に依存しているパッケージは再度コンパイルが必要となるため弊害があるため、 自動的にGuest Additionsのアップデートを行わない設定にします。

Vagrantfileを以下のように記述します。

```
Vagrant.configure("2") do |config|
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true
```

- 「config.vbguest.auto\_update」は、 agrant up時に自動的にGuest Additionsのアップデートを行わない設定
- 「config.vbguest.no\_remote 」は、isoファイルをリモートからダウンロードしない設定

## ホストマシンと仮想マシンの共有フォルダの設定

Vagrantfileを編集します。

以下のように、記述して共有フォルダの設定をします。

```
config.vm.synced_folder "[ホストOS]", "[仮想マシン]", "[オプション]
```

Windowsの場合、/は\\\\にする必要があります。

```
例）config.vm.synced_folder "C:\\project", "/var/www/html/project "
```

編集したら、再読み込みをします。

```
vagrant reload
vagrant up
```
