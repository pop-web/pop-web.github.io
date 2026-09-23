---
title: "Laravelプロジェクトをインストールエラー（zip、unzip、gitがないため）"
url: "/programming/laravel/laravel-install-error/"
date: 2019-05-27
categories: 
  - "laravel"
---

vagrantのCentOS7環境で、Laravelを「cmposer create-project …」でインストールしようとしたら以下のエラーがでた。

```
# composer create-project --prefer-dist laravel/laravel /var/www/html/laravelapp
Installing laravel/laravel (v5.5.28)
    Failed to download laravel/laravel from dist: The zip extension and unzip command are both missing, skipping.
Your command-line PHP is using multiple ini files. Run `php --ini` to show them.
    Now trying to download from source
  - Installing laravel/laravel (v5.5.28): Cloning f4cba4f2b2

  [RuntimeException]
  Failed to clone https://github.com/laravel/laravel.git, git was not found, check that it is installed and in your P
  ATH env.

  sh: git: command not found

create-project [-s|--stability STABILITY] [--prefer-source] [--prefer-dist] [--repository REPOSITORY] [--repository-url REPOSITORY-URL] [--dev] [--no-dev] [--no-custom-installers] [--no-scripts] [--no-progress] [--no-secure-http] [--keep-vcs] [--remove-vcs] [--no-install] [--ignore-platform-reqs] [--] [<package>] [<directory>] [<version>]
```

エラーを見ると、**zipとunzipがない。そして、gitもない。**と言っています。

なので、以下でzip, unzip, gitインストールします。

```
# yum -y install zip unzip git
```

これで再度コマンドを入力すれば、無事にLaravelプロジェクトがインストールされます。
