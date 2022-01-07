+++
author = "twoooooda"
title = "AutoHotKeyを導入してみた"
date = "2022-01-04"
description = "キーボードについて調べていくうちに、AutoHotKeyというものに行きつきました。"
tags = [
    "日記",
    "知見",
    "備忘録",
    "プログラミング"
]
categories = [
    "diary"
]

series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
image = ""
slug="Introduce AutoHotKey"
+++

## 導入を考えたきっかけ
　[以前の記事](https://twoooooda.github.io/p/%E7%A7%81%E3%81%AE%E8%87%AA%E4%BD%9C%E3%82%AD%E3%83%BC%E3%83%9C%E3%83%BC%E3%83%89%E5%A5%AE%E9%97%98%E8%A8%98/)でも述べた通り、自作キーボードに対応しているファームウェアがマジのゴミで、キー割り当てを変えられなかったので、いろいろ調べているうちにAutoHotKeyというものを見つけました。  

## AutoHotKeyとは？
　独自のプログラミング言語を用いて、キーボードのキー操作によるかなり柔軟なショートカットの作成や、普段あまり使わないキーを全く別のキーとして割り当てたり、メディアコントロールやPCのシステム操作を割り当てたりできます。[参考](https://fuchiaz.com/usage-autohotkey/)


## 導入方法
　基本的に[このサイト](https://fuchiaz.com/auto-hot-key/#AutoHotkey)に従ってインストールして、拡張子を`.ahk`としたテキストファイルを作り、そこに任意のショートカットやキーの割り当てを記述していきます。記法や文法、決まりごとは[日本語のリファレンス(?)](https://so-zou.jp/software/tool/system/auto-hot-key/introduction/)があるので、そちらを参照してください。  

　このAutoHotKeyを使う場合、拡張子が`.ahk`のファイル、あるいは`.ahk`のファイルから作成した`.exe`の実行ファイルを起動し、タスクトレイで常駐させておかなければなりません。なので、PC起動時に自動で該当のファイルが起動するようにしておくと便利です。私は[こちらのサイト](https://kiryusblog.com/autohotkey-autorun/)を参考に設定しました。


## 実際に使う
　私がこのAutoHotKeyでやりたかったのは、"**キーボード右上のPageUp、PageDownのキーをメディアの再生一時停止、次の曲ボタンへ割り当て**" です。実際のコードはごく簡単なもので、以下のように書いたら思った通りに動いてくれました。

```
#InstallKeybdHook
#UseHook

PgUp::Media_Play_Pause
PgDn::Media_Next
return
```  

また、便利だと聞いたので[こちらのサイト](https://www.karakaram.com/alt-ime-on-off/)を参考に、左右Altの空打ちで日本語入力と英字入力を切り変えられるようにしました。私は起動しないといけない実行ファイルが増えると嫌なので、実際に使うときはPageUp/Downキーをメディアコントロールに割り当てるコードと、`alt-ime-ahk.ahk`の内容を一つのファイルに統合しています(ファイル名を`general.ahk`としています)。

## 導入してみて
　導入してしばらく使ってみましたが、**とてもとても便利です。** タスクトレイに入るアイコンが一つ増えるのが最初は少し抵抗がありましたが。それを補って余りある恩恵を受けています。今後新しくショートカットが欲しくなったり、割り当てを変えたい時が来れば今使っている`general.ahk`ファイルに追記すればいいだけなので、使い方を多少覚えていればこれからもっと気軽に便利にしていくことができると思います。