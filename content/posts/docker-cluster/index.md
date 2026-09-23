---
title: "Dockerのクラスタとは？"
url: "/server/docker-cluster/"
date: 2020-03-10
categories: 
  - "docker"
  - "server"
---

Dockerには、Docker Swarmというクラスタ管理機能というツールがある。

このクラスタとはなにか。

クラスタとは、**複数のシステムをまとめて1つのシステムとして振る舞っているもの。**

クラスタ化することをクラスタリングと言われたりもする。

Docker Swarmでは、複数のDockerホストをネットワーク接続しグループ化することで1つのDockerホストのように管理ができる。

これを、Dockerのクラスタ管理という。
