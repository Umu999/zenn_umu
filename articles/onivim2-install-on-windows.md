---
title: "Onivim 2 インストールチャレンジ（Windows）"
emoji: "👹"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Onivim"]
published: false
---

# 概要

Onivim 2というエディタを知ったので、Windowsにインストールしてみた。
Onivim 2を知らない人も多いだろうから、まずはその説明を行う。
インストール方法だけ知りたい人は[こちらを参照](#windowsでのインストール)。

# Oni/OniVimとは

VSCodeやAtomと同じ、Electronで作られたNeoVimベースのGUIエディタ。
エディタ部分だけでなく、ファイルツリーやコマンドの呼び出し方などもVimと同じようなキーバインドでできる。
もともとはOniVimと呼ばれていたのが、ある時点でただOniというようになったらしい。
今は開発を停止しており、Onivim 2が後継として開発されている。
# Onivim2の特徴

OniはReact + TypeScriptで開発されていたようだが、Onivim 2は Revery + Reasonで開発されている。

## Reason

https://reasonml.github.io/docs/en/what-and-why

ReasonはOCamlベースのAltJSで、HaskellインスパイアのElmやPureScriptに対してOCamlインスパイアなのがReasonという感じ（詳しくは知らない）。
Reasonで書いたコードはJavaScriptにトランスパイルしてブラウザで動かすこともできるし、ネイティブコードにコンパイルして単体で実行させることもできる、らしい。

## Revery

https://github.com/revery-ui/revery

ReveryはElectronインスパイアのGUIフレームワークで、Reasonで書かれている。
Reason + Reveryを使って書かれたGUIアプリはネイティブコードにコンパイルされるので、Electronより速い、らしい。

## Vimをモダンエディタで

いきなり技術の話から入ってしまったが、Onivim 2が目指すところはOniと同じ、Vimの思想をモダンエディタで実現することだろう。
Vimと同じモーダル（モードを切り替えて使う）テキストエディタであり、キーボードだけを使って操作できる。それゆえ生産性を最大限に高められる。
まあ、ここでVimの良さを語るのははOnivim 2 を使ってみようなどという奇特な諸兄には釈迦に説法だろう。
藪蛇でマサカリを投げられるのも怖いのでこの辺にしておく。
とにかく、VSCodeっぽいVim――あるいはVimっぽいVSCode――ということだ。
そして、Electronではパフォーマンスに問題があるためReason + Reveryに移行したということだろう。
英断だと思うし、自分も見習いたい（ほんまか）。

## すべてがキーボードで操作可能

Vimの良さをこれ以上語らないと言ったばかりだが、ここだけは説明させてもらいたい。
ガチのVimmer諸兄はご存じないかもしれないが、自分のようななんちゃってVimmer(あるいはVimキーバインドラバーとでも呼ぶべきか)なら、VSCodeにVimキーバインド拡張を入れてVimライクに使えることを知っているだろう。
ではそれでVimと同じ体験ができるかというと、そうではない。
ファイルツリーでの選択の移動など、Vimとは異なる操作（おおむねマウス操作）を強いられるところは多い。もちろんキーボードショートカットをきっちり設定すればできるのだが、それらも他の操作とぶつからないように設定するのも大変なのだ。
VSCodeにおいては「キーボードのみで操作可能」というのは「できなくはない」というだけであって、そんなことをやろうとする変態はそれなりに労苦を強いられることになるのだ、残念ながら。

それに対してOnivim 2では、「すべての要素がキーボードで操作可能」である。
それを実現するのが「[Sneak mode](#スニークモード)(以下スニークモード)」で、`Ctrl + g`を押すだけで画面上のすべての要素にショートカットキーが割り振られる。
「エディタ(の選択)」と「エディタを閉じるボタン」に別々にショートカットキーが割り振られるのだから徹底している。
これだけでもVSCodeから乗り換えたくなったなんちゃってVimmerは多いのではないだろうか。
Vimに限界を感じつつも、「VSCodeはチョットなァ～～」なんて考えているVimmerにもおすすめだ。

## VSCodeの拡張機能が（一部）使える

なんと、OniVim 2 ではVSCodeの拡張機能を使うことができる。
とはいえ、すべての拡張機能を使えるわけではない。
VSCodeの拡張機能が配布されている VSCodeマーケットプレイスは実はMicrosoft製品だけに仕様が許されている。
VSCode互換のエディタはVSCodiumやEclipse Theiaなどがあるが、それらはMicrosoft製品ではないので、VSCodeマーケットプレイスで配布されている拡張は使えないのだ。

そうした問題意識から、[Open VSX Registry](https://open-vsx.org)では、こうしたVSCode互換エディタで使用できる拡張機能を配布している。
Onivim 2で使用できるのもこうした拡張機能だ。
つまり、「Open VSXで配布されている、VSCode互換の拡張機能」だけを使用できる。
ちょっとがっかりしたか？ 私もした。
でもブラウザのタブを閉じる前に、[Open VSX Registry](https://open-vsx.org)でいくつかお気に入りの拡張を検索してみるといい。
**意外とある。**
なんと、Microsoft製のPython拡張もある。**Microsoft製なのに！？**
[GitHub](https://github.com/microsoft/vscode-python/blob/main/LICENSE)を見れば分かるが、VSCodeのPython拡張はMITライセンスなのだ。ありがとう、Microsoft。

残念ながら、VSCodeの非常に強力な拡張機能であるRemote development(SSH/WSL/Container)は[オープンソースではない](https://code.visualstudio.com/docs/remote/faq#_why-arent-the-remote-development-extensions-or-their-components-open-source)ため、Onivim 2での利用は絶望的だろう、今のところ。Microsoftに期待したい（お願いします）。

## Vim/NeoVimの設定は（最終的には）ロードできるようになる

今はまだできない。
[FAQ](https://onivim.github.io/docs/other/faq#can-i-use-my-initvim-or-vimrc-with-onivim-2)によると、ベータ版のマイルストーンに `init.vim`と`.vimrc`の使用がスケジュールされているらしい。
Vimmer諸兄は首を洗って待っていてほしい。
# Windowsでのインストール

ようやくインストールの話だ。お待たせした。いや待っていないかもしれないが。
しかし手順に入るその前に、単にインストールするだけでわざわざ説明がいるのか、という点を説明させてほしい。すまない。

## インストールバイナリの配布とアーリーアクセス



## ソースからビルド

# Onivim 2を使ってみる

## ターミナルの起動

## ファイルツリーの利用

## スニークモード

## 使ってみた感想
