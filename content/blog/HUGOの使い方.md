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
<br>


# 1.HUGOのインストール
　基本的には、HUGOの公式サイトにある[Quick Start](https://gohugo.io/getting-started/quick-start/)に従ってHUGOをインストールしていきます。自分はWindows環境なので、[Chocolatey](https://chocolatey.org/install)というパッケージマネージャを使ってインストールしました。以降はコマンドプロンプト上で、`$ choco install hugo -confirm`を実行するとHUGOがインストールされるはずです。`$ hugo version`でバージョンが確認できたらインストールされています。

    $ hugo version
    hugo v0.81.0-59D15C97 windows/amd64 ....  
<br>
  
# 2.GitHubの準備
　自分のGitHubのページにて、<UserName>.github.ioという名前でリポジトリを作ります。リポジトリ名をこうすることでGitHubさんがPages用のリポジトリだと勝手に判断してくれます。なのでリポジトリ名がページのURL`http://<UserName>.github.io`になります。あとはこのリポジトリをクローンして取り敢えず環境は完成です。

　一般的なのはソースコード用のリポジトリを別で作り、ビルド結果だけをPages用のリポジトリにあげる方法らしいのですが、自分はリポジトリが増えるのが嫌だったので、souceブランチをソースコード用、mainブランチをデプロイ用のブランチにしました。  
<br>

# 3.サイトのひな形を作る
　まずsourceブランチを作り、その下にHUGOのコマンドでファイルを作っていきます。`$ hugo new site <フォルダ名>`でHUGOを使う上で最小限のファイルとフォルダを作成できます。

    $ cd <UserName>.github.io
    $ git branch source
    $ git checkout source
    $ hugo new site blog

自分はsourceブランチ直下で作業したかったので、blogフォルダ内の内容をsourceブランチ直下に移動させました(blogフォルダは削除)。  
<br>

# 4.テーマを導入する
　[HUGO公式のthemeページ](https://themes.gohugo.io/)からテーマを選んで、自分のリポジトリに[サブモジュール](https://qiita.com/sotarok/items/0d525e568a6088f6f6bb)として追加します。例としてこのブログのテーマ([Hugo Future Imperfect Slim](https://themes.gohugo.io/hugo-future-imperfect-slim/))を用います。

    $ git submodule add https://github.com/pacollins/hugo-future-imperfect-slim.git themes/hugo-future-imperfect-slim

サブモジュール化ができたら、themes/hugo-future-imperfect-slim/**exampleSite** 内のファイルやフォルダをまるっとブランチ直下にコピーします。それと同時に、もともとあったconfig.tomlは消しておきます(configファイルが競合するため)。  
<br>


# 5. 試運転してみる
　`$ hugo server`でローカルにサーバーをたてて、[http://localhost:1313](http://localhost:1313/)にアクセスするとサイトの内容を確認できます。ここまでが正しくなされているとテーマのDemoサイトと同じ状態になっているはずです。  
　また、`$ hugo`でサイトのデータをビルドできます(/publicが生成される)。
<br>

# 6.GitHub Actionsの設定
　**GitHub Actions**とは、リポジトリにコミット等があった時にそれを検知して自動的にデプロイとかする機能(?)らしいです(今回の場合)。自分は雰囲気で使っているので詳しくは知りません。  
　sourceブランチの下に、.github/workflows/**main.yml**を作成します。[Reonaさんのブログ](https://reona.dev/posts/20200331)を参考にしてmain.ymlに記載のあるデプロイ先ブランチとかを編集しました。  
  
　ここまでやってきたことをまるっとsourceブランチにコミットすると、Actionsが働いてmainブランチにデプロイされ、ページが公開されるはずです。  
<br>

# 7. 設定を編集する
　config.tomlを見てみると、設定の項目がズラリとならんでいます。ここでサイトを自分用にするための設定をしていきます(サイトの名前、アイコン、URLなど)。  
　ここで、アイコンに設定したい画像等をconfig.tomlから指定するのですが、そういうのは基本 **/static**に保存して、パスを指定するようです。追加のcssで見た目を変えたい時も、/themesの中には書かず、/layouts/partials内とかに書くようです(細かいところはテーマによって変わるのでテーマのDocumentを読んでね)。  
<br>

# 8.記事を追加する
　サイトの枠組みが完成したら、あとはサンプルの記事を削除して、自分の記事を追加していきます。`$ hugo new <フォルダ名>/<ファイル名>`で新しい記事を追加できます。基本的に記事は全てMarkdown形式で書きます。

    例) $ hugo new content/blog/article_2021-01-01.md  
<br>  
<br>  

# 参考サイト
* [Hugo+Github Pagesで新しい個人ウェブサイトを作った](https://dev.to/mshr_h/hugo-github-pages-35me)
* [Hugo + GitHub Pages + GitHub Actions で独自ドメインのウェブサイトを構築する](https://zenn.dev/nikaera/articles/hugo-github-actions-for-github-pages)
* [GitHub Pages × Hugo で技術ブログを始めた](https://reona.dev/posts/20200331)
