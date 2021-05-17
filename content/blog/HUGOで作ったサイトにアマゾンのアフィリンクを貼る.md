+++
author = "twoooooda"
title = "HUGOで作ったサイトにアマゾンのアフィリンクを貼る"
date = "2021-04-13"
description = "HUGOで作ったサイトにアマゾンのリンクを貼る方法の備忘録です。"

tags = [
    "知見",
    "備忘録",
    "web",
    "HUGO"
]

categories = [
    "Web"
]

[[images]]
src = "img/2021/hugo.png"
+++

# HUGOの仕様
　静的サイトジェネレータ "HUGO" は、テーマの制作者がGitHubに上げているリポジトリをサブモジュールとして使うことで自分のサイトにテーマを導入します。ゆえに、テーマのレイアウトに関わるコードを直接書き換えることはできません(ローカルファイルは書き換えられますが)。 
<br>
　しかしHUGOの仕様として、`/static`や`/layouts`以下のフォルダやファイルが優先して読み込まれるというものがあります。
<br>
<br>

# ではどうするか
　[当サイトのテーマ](https://themes.gohugo.io/hugo-future-imperfect-slim/)を例にすると、`/themes/hugo-future-imperfect-slim/layouts/partials/site-sidebar.html`を書き換えたい場合、該当のファイルを`/layouts/partials/`にコピー＆ペーストすると、そちらの方が先に読み込まれるので結果的に編集して上書きが可能なわけです。
<br>
　あとはAmazonのアフィリエイトリンクのHTMLコードを生成して、上記のsite-sidebar.htmlに書き込むとちゃんと表示されました。
<br>
<br>

