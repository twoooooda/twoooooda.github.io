+++
author = "twoooooda"
title = "ラズパイに公開鍵認証でssh接続するまで"
date = "2021-04-03"
description = "さっき買ってきたRaspberry piに公開鍵認証でssh接続するまでの方法を備忘録としてまとめます。"
tags = [
    "知見",
    "備忘録",
    "Raspberrypi",
]
categories = [
    "Raspberrypi"
]

[[images]]
src = "img/2021/raspi.jpeg"
+++

# はじめに
　家に余ってたラズパイをテレビの録画サーバーにしようと思い、部屋の奥から引っ張り出してきたのはいいですが、初期設定もsshの接続設定も何もしてなかったので勉強がてらやってみました、例の如くやらないといけないことが複数サイトに渡って散らばっていたので、備忘録として一通りまとめます。なお、ラズパイのOSはインストール済み, 有線LAN接続なのを前提とし、ラズパイからモニターにはなるべく出力せずに設定することを目指します。   
<br>

# ホストのPCからラズパイにssh接続する
　OSをインストールしたmicroSDカードをラズパイに挿す前に, microSDカードの直下にsshという空のファイル(拡張子も無し)を作っておきます。こうすることでラズパイのGUIを触らずにsshをオンにできます。
<br>

　ラズパイに接続して操作するホストのPCのターミナルから、以下のコマンドを叩くだけでssh接続できます(デフォルトユーザーにログインする場合)。
<br>


```
$ ssh pi@raspberrypi.local
```

<br>

# IPアドレスの固定
　インターネットに繋げるたびにIPアドレスが変わると困るので、まずはラズパイのIPアドレスを固定します。ラズパイのIPアドレス固定の前に、ホストのPCのターミナルで以下を実行します。
```
$ ipconfig /all
```
<br>

するとIPの構成が一覧で出てくるので、 **デフォルトゲートウェイとDNSサーバーのIPアドレス**(192.168.x.OOOみたいなやつ)をメモします。
<br>

　以降はssh接続を介してラズパイのコマンドを叩いていきます。まずはラズパイの設定ファイルを開いて編集します。
```
pi@raspberrypi:~$ sudo nano /etc/dhcpcd.conf
```
<br>

nanoはviでもいいです(好み)。開いたら、末尾に以下を追加します。

```
interface eth0(無線LANの場合はここをwlan0にする)
static ip_address=192.168.x.***/24
static routers=192.168.x.OOO
static domain_name_servers=192.168.x.OOO
```
<br>

`staric ip_adress=`にラズパイに割り当てたいIPアドレスを書きます。***の部分は1桁、2桁台は他の危機に割り当てられていることが多いので適当に100～200くらいにするといいらしいです。/24はサブネットマスク長です。
`static routers=`にデフォルトゲートウェイのIPアドレス、`static domain_name_servers=`にDNSサーバーのIPアドレスを書きます。編集できたら、保存してラズパイを再起動すれば変更が適用されているはずです。
<br>
 **[参考記事](https://mugeek.hatenablog.com/entry/2019/05/27/230256)** 
<br>
<br>


# ユーザーの追加, 権限の付与
## 新しいユーザーの追加
　デフォルトユーザーである「pi」とは別のユーザーを作成し、権限をそちらに移行します。sshを介してラズパイのターミナルにて、
```
pi@raspberrypi:~$ sudo adduser |newuser|
```
<br>

|newuser| に任意のユーザー名を指定して実行します。「New password:」と「Retype new password:」へ新規ユーザーのパスワードを指定します。その後くらいにいろいろ設定項目が出てきますが、特に必要がないなら全てEnterで進んでもいいです。
<br>
<br>

## 権限の付与
　「pi」ユーザーの権限を新しいユーザーに追加します。まずは「pi」ユーザーの権限を確認するために以下を実行します。
```
pi@raspberrypi:~$ groups pi
```
<br>

すると「pi」ユーザーに付与されている権限が一覧で表示されるので、usermodコマンドで新しいユーザーに権限を移します。
```
pi@raspberrypi:~$ sudo usermod -G pi,adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,input,netdev,spi,i2c,gpio newuser
```
<br>
<br>

## 「pi」ユーザーのホームフォルダのコピー
　「pi」ユーザーのホームフォルダの内容をnewuserのホームフォルダにコピーします。
```
pi@raspberrypi:~$ sudo cp -r /home/pi/* /home/newuser
```
<br>
<br>

# デフォルトユーザー(pi)の無効化
　ラズパイを起動する度に「pi」ユーザーに勝手にログインされると困るので、「pi」ユーザーへの自動ログインをオフにして、ついでに「pi」ユーザーを無効化します。
```
pi@raspberrypi:~$ sudo nano /etc/lightdm/lightdm.conf
```

/lightdm/lightdm.confの126行目の先頭に#を入れてコメントアウトします(`#autologin-user=pi`)。
<br>
<br>

　次に、newuserでオートログインするようにします。autologin@.serviceを開いて、28行目の「--autologin pi」を「--autologin newuser」へ変更します。
```
pi@raspberrypi:~$ sudo vi /etc/systemd/system/autologin@.service
```
<br>

　最後に「pi」ユーザーを無効にします。ユーザーのアカウントの有効期限を過去の日付にすることで無効化できます。
```
pi@raspberrypi:~$ sudo usermod --expiredate 1 pi
```
<br>

以降は新しく作成したnewuserで作業します。<br>
 **[参考記事](https://monoist.atmarkit.co.jp/mn/articles/1912/11/news022_2.html)** 
<br>
<br>

# 鍵ペアの生成と送信、設定
## 鍵の生成と送信
　公開鍵認証とは、パスワードの代わりに公開鍵(ホスト側のPC)と秘密鍵(ラズパイ)のペアで認証する方法です。まずはホストのPCに鍵ペアを生成するディレクトリ及び鍵ペアを作ります。
```
$ mkdir ~/.ssh/raspberrypi
$ ssh-keygen -t rsa
```
<br>
すると、鍵の生成場所を聞かれるので、先ほど作ったディレクトリを指定します。

```
Enter file in which to save the key (/Users/username/raspberrypi/.ssh/id_rsa):
```
<br>

必要に応じてこの後に聞かれるパスフレーズも入力します。自分は省略しました。これで秘密鍵の`id_rsa`と公開鍵の`id_rsa.pub`が生成されます。これらのうち`id_rsa.pub`をラズパイ側に送ります。

```
$ scp ~/.ssh/raspberrypi/id_rsa.pub newuser@raspberrypi:~
```
<br>

## 公開鍵の設定

　ラズパイにパスワード認証でログインし、鍵を管理する`.ssh`を作ります。
```
newuser@raspberrypi:~$ sudo mkdir ~/.ssh
```
<br>

先ほど送信した`id_rsa.pub`を`authorized_keys`と名前を変更しつつ`.ssh`に移動します。
```
newuser@raspberrypi:~$ sudo mv ~/id_rsa.pub ~/.ssh/authorized_keys
```
<br>

次に`.ssh`、`authorized_keys`のパーミッションを変更します。
```
newuser@raspberrypi:~$ chmod 600 ~/.ssh/authorized_keys
newuser@raspberrypi:~$ chmod 700 ~/.ssh
```
<br>

最後に`ssh_config`を修正します。
```
newuser@raspberrypi:~$ sudo nano /etc/ssh/sshd_config
```
<br>

以下のような記載の行のコメントアウトを外して編集します。
```
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2
```
この時にポート番号の変更や、パスワード認証のオフができます。しかし、**公開鍵認証が上手くいってない状態でパスワード認証をオフにしてしまうと、次回からログインできなくなってしまう**ので慎重に行ってください。最悪OSのインストールし直しになります(私は既に何度かやらかしました)。
<br>
<br>

最後にラズパイを再起動すると、次回から以下のコマンドで公開鍵認証でログインできるはずです。
```
$ ssh -i [秘密鍵ファイル] -p [ポート番号] pi@[Raspberry PiのIPアドレス]
(例)ssh -i .ssh/id_rsa -p 22 pi@192.168.0.100
```

**[参考記事](https://tool-lab.com/raspi-key-authentication-over-ssh/)** 
<br>
<br>

# ssh configの追加
　この方法で接続してもいいんですが、コマンドが長いので短縮します。
<br>

`~/.ssh/config`を作り、以下を追記します。
```
Host [任意のコマンド名]
	HostName [ラズパイのIPアドレス]
	User [ラズパイのユーザーネーム]
	Port [ラズパイのポート番号]
	IdentityFile [秘密鍵ファイル]
```
<br>

今回の例では以下のようになります。
```
Host raspi
	HostName 192.168.1.100
	User newuser
	Port 22
	IdentityFile ~/.ssh/id_rsa
```
<br>

こうすることでssh接続時のコマンドを以下のように大幅に短縮できます。
```
ssh raspi
```
**[参考記事](https://zenn.dev/ryo_kawamata/articles/raspberrypi-auth-setting)** 
<br>
<br>

以上です。ファイル名や環境は各々によって違うのでそこは適宜変更をおねがいします。