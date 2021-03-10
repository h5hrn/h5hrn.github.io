---
layout: post
title:  "Jekyll を使った GitHub Pages サイトの設定"
categories: Jekyll
---

### Docker で Jekyll サイトを生成

1. 新しいプロジェクトを以下コマンドで作成する。

    ```sh
    $ docker run --rm --volume="$PWD:/srv/jekyll" -it -p 4000:4000 jekyll/jekyll:latest jekyll new my-pages
    ```

2. `docker-compose.yml` を作成し、ローカル環境で確認しながら編集できるようにする。

    ```yaml
    version: '3.8'
    services:
      app:
        image: jekyll/jekyll:latest
        command: jekyll serve --watch --force_polling
        volumes:
          - .:/srv/jekyll
        ports:
          - 4000:4000
    ```

3. 以下コマンド実行後にブラウザで `localhost:4000` を開くと、リアルタイムで更新した内容を確認できる。

    ```sh
    $ docker-compose up
    ```

### GitHub Pages に公開

1. GitHub で `{username}.github.io` という名前のリポジトリを作成する。
2. ローカルで作成した `my-pages` をコミットし、作成したリポジトリにプッシュする。
3. GitHub リポジトリの `Settings > GitHub Pages` より、公開するブランチなどを設定する。
4. プッシュした内容が `https://{username}.github.io/` に公開される。
