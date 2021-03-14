+++
author = "twoooooda"
title = "静的サイトジェネレータ”HUGO”で個人ブログをつくり、GitHub Pagesで公開する"
date = "2021-03-07"
description = "当ブログにも使われている便利ツールHUGOの使い方を、詰まった点も含めて簡単にまとめてみました。"
tags = [
    "知見",
    "web",
    "hugo"
]
categories = [
    "HUGO",
    "Web"
]

[[images]]
src = "img/2021/hugo.png"
+++

# はじめに
　自分は最近、当ブログを静的サイトジェネレータであるHUGOを用いて作成しました。HUGO自体はわかってしまえばごく簡単に導入、使用できるんですが、いかんせん自分がweb系、GitHubに関して無知であったためハマりポイントをことごとく踏み抜いていき、結局公開までかなり時間がかかってしまいました。なので覚えているうちに使い方や参考サイトをまとめようと思います。

　ちなみにデプロイ方法等は我流でやっていますが、いろんな方法があるみたいなのであくまで一例としてお読み頂けたら光栄です。


# 1.HUGOのインストール
　基本的には、HUGOの公式サイトにある[Quick Start](https://gohugo.io/getting-started/quick-start/)に従ってHUGOをインストールしていきます。自分はWindows環境なので、[Chocolatey](https://chocolatey.org/install)というパッケージマネージャを使ってインストールしました。以降はコマンドプロンプト上で、`choco install hugo -confirm`を実行するとHUGOがインストールされるはずです。`hugo version`でバージョンが確認できたらインストールされています。

    $ hugo version
    hugo v0.81.0-59D15C97 windows/amd64 ....

# 2.GitHubの準備
　自分のGitHubのページにて、<UserName>.github.ioという名前でリポジトリを作ります。リポジトリ名をこうすることでGItHubさんがPages用のリポジトリだと勝手に判断してくれます。なのでリポジトリ名がページのURL`http://<UserName>.github.io`になります。あとはこのリポジトリをクローンして取り敢えず環境は完成です。ただ、一般的なのはソースコード用のリポジトリを別で作り、ビルド結果だけをPages用のリポジトリ方法っぽいのですが、自分はリポジトリが増えるのが嫌だったので、souceブランチをソースコード用、mainブランチをデプロイ用のブランチにしました。

# 3.サイトのひな形を作る
　2. で述べた通り、





# 参考サイト
* [Hugo+Github Pagesで新しい個人ウェブサイトを作った](https://dev.to/mshr_h/hugo-github-pages-35me)
* [Hugo + GitHub Pages + GitHub Actions で独自ドメインのウェブサイトを構築する](https://zenn.dev/nikaera/articles/hugo-github-actions-for-github-pages)
