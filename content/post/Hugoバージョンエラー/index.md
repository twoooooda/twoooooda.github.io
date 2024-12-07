+++
author = "twoooooda"
title = "Hugoで非推奨のプロパティとかを使ってエラーが出たとき"
date = "2024-12-01"
description = "Hugoを使っててつまずいたところの備忘録です。"

tags = [
    "知見",
    "備忘録",
    "web",
    "HUGO"
]

categories = [
    "Web"
]

series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
slug="hugo-komattara"
image = "hugo.png"
+++
この記事は、[某企業アドベントカレンダー2024](https://adventar.org/calendars/10291)の六日目の記事です。
## 経緯
最近久々に記事を書こうとしてHugoを使ったら、以下のようなエラーメッセージが出てうまくビルドができませんでした。
```
$ hugo
WARN  deprecated: resources.ToCSS was deprecated in Hugo v0.128.0 and will be removed in a future release. Use css.Sass instead.
ERROR deprecated: .Site.LastChange was deprecated in Hugo v0.123.0 and will be removed in Hugo 0.140.0. Use .Site.Lastmod instead.
```
要するに、現行のHugoのバージョンでは非推奨、あるいは利用不可なプロパティを使っているせいで怒られています。

## 原因
私の場合は、[ブログのテンプレート](https://github.com/CaiJimmy/hugo-theme-stack)をgitのsubmoduleの形で使っていて、ある程度はテンプレートのリポジトリに依存してしまいます。submoduleの仕様として、submoduleの関係を設定したタイミングのリポジトリの状態を保持してしまうみたいです。このブログを作ったのがもう3年前なので、テンプレートの中身が3年前のバージョンで止まっていたのが原因でした。

## 解決
解決策としては、以下のコマンドでsubmoduleに設定しているリポジトリを最新の状態にすれば解決できました。
```
$ git submodule update --remote
```
内容としてはこれだけなのですが、備忘録として残します。

## 補足
PCを新調したなどで、新しく`git clone`したい場合、以下のコマンドでsubmoduleの情報、submoduleとの関係などを引き継いでcloneできます。
```
git clone --recursive https://github.com/twoooooda/[リポジトリ.git]
```