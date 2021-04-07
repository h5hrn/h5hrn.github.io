---
layout: post
title:  "WSL2 で React プロジェクトの実行に時間がかかる問題"
categories: Windows
---

### 事象

- Windows 10 の WSL2 上で React のプロジェクトを動かそうと `npm start` を実行した。
- 軽量なサンプルアプリなのに立ち上がるまでに1、2分かかる。

### 原因

- Windows 10 の D ドライブにプロジェクトファイルを配置していた。
  - `/mnt/d/project/sample-app`
- WSL2 から Windows ファイルシステムへのアクセスは速度が遅いらしいので、実行に時間がかかったと思われる。

### 対策

- Linux ファイルシステム上にプロジェクトファイルを配置する。
- Windows ファイルシステム上の `\\wsl$` 配下が WSL2 の Linux ファイルシステムと対応している。
  - Windows: `\\wsl$Ubuntu\home\username\project\sample-app`
  - WSL2: `~/project/sample-app`
