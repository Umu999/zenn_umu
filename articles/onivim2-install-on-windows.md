---
title: "VSCodeライクなVim。Onivim 2でなんちゃってVimmerを超えていけ"
emoji: "👹"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Onivim", "Vim"]
published: false
---

# はじめに

Atomが死に、VSCodeがモダンエディタ戦争を制して一年[^1]。
VSCodeでVimキーバインドを使っている**なんちゃってVimmer**も多いだろう。かくいう私もその一人である。
VSCodeの美しいGUIと強力な拡張機能に日々助けられてはいるものの、あともう少し、ほんの少しだけ、マウスを使う機会を減らしたい。
そんな気持ちを捨てられずにいる。

[Onivim 2](https://v2.onivim.io/#about)はそんな諸兄のためのエディタだ。
VSCodeのような美しいGUIと、VSCode互換の拡張機能を持つ。
そして、ゴリゴリに練り上げられた`.vimrc`や`init.vim`をロードすることができる――将来的には[^2]。
本当に欲しかったものがそこにある。それがOnivim 2なのだ。

## ライセンスについて

読み進める前に、[ライセンス](https://github.com/onivim/oni2#licensed)について確認しておこう。
Onivim 2はオープンソースではあるが、無償での利用は非商用と教育目的に限られている。
商用目的――つまり、業務でのプログラミングその他の営利目的の作業――で使うためには、ライセンスの購入が必要だ。
記事執筆時点(2021年1月25日)では40ドルだが、これは開発が進むごとに価格が上がっていくそうだ。

業務で使えないならやっぱりVSCodeでいいや、そう思った方もいるだろう。
だが、Onivim 2は"Time delay"デュアルライセンスを採用している。
`master`ブランチにマージされてから18か月後に、すべてのコミットはMITライセンスとなる[^3]。
MITライセンスとなったコードは[別のリポジトリ](https://github.com/onivim/oni2-mit)で管理されている。
つまり、１年半待ちさえすれば商用でもこの素晴らしいエディタを使うことができる、というわけだ。
その時間とライセンスフィーを天秤にかけて、どちらを選ぶかを決めればいい。

:::message alert
この記事を読んでOnivim 2を**タダで**使おうと思ったあなた、会社の敷地内ではVSCodeを使うように。
リモートワークでも駄目だ。いい年をして屁理屈をこねるな。
間違っても私がそそのかしたなどと口を滑らせないでくれ。
:::

分かったら次に進もう。

## 本記事の構成

まず、Onivim 2の説明を簡単にしてからインストール方法を説明する。
インストール方法だけ知りたい人は[こちらを参照](#windowsでのインストール)。

[^1]: [2019年10月ごろにAtomの更新停止が話題になった。](https://bookyakuno.com/atom-editor-end-of-development/)。最近は少しコミットが増えている。
[^2]: [beta版で手に入る](https://v2.onivim.io/#timeline)らしい。
[^3]: ちなみに、Outrun Labs――Onivim 2の開発会社――の外部からのコントリビューションは、18か月を待たずにMITライセンスとなるらしい。頑張ろう！（？）

# Oni/OniVimとは

Onivim 2の前身として、Oni/OniVimというエディタが開発されていた。
VSCodeやAtomと同じ、Electronで作られたNeoVimベースのGUIエディタだ。
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

いきなり技術の話から入ってしまったが、Onivim 2が目指すところはOniと同じ、Vimの思想をモダンエディタで実現することだ。
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

現在のところ、Onivim2はアーリーアクセスに参加してnightly development buildをダウンロードするか、ソースコードからビルドするしかない[らしい](https://onivim.github.io/docs/other/faq#where-can-i-download-a-build)。
アーリーアクセスに参加するためには[40ドルの支払いが必要](https://v2.onivim.io/early-access-portal)だ。
コミュニティを支援するために、Onivim2が気に入った人はいっちょ参加してみてはどうだろうか。
しかしまずは使ってみないとお話にならない。
ということでソースからビルドしてみよう。

なお、タイトルにもあるが、ここではWindows上でインストールを行う。
筆者はMacを使っていないし、Linuxだったら何の問題もなくビルドできるだろう。
Windowsでのビルドは――多くの人が**やりたくない**と思うだろう。そこにこの記事の価値がある(ドヤッ)。

## ソースからビルド

### 依存関係のインストール

ビルド手順は[ここ](https://onivim.github.io/docs/for-developers/building)に書かれている。
ターミナルにはGit-bashを使う。
管理者権限で開いておこう。ReasonのパッケージマネージャであるEsyの実行に必要なのだ。


Git, Nodeをインストールしたら、Esyをインストールする。

```bash
$ npm install -g esy@latest
```

次に、Windows用のビルドツールを入れる。

```bash
$ npm install -g windows-build-tools
```

Linuxの場合はさらに、`revery`の依存関係のインストールが必要なのだが、Windowsでは準備はこれでおしまいだ。

次にリポジトリをクローンする。

```bash
$ git clone https://github.com/onivim/oni2
$ cd oni2
```

`master`ブランチだとやや不安なので、最新のタグを使おう。

```bash
$ git tag
0.5.0
v0.5.1
v0.5.2

$ git checkout v0.5.2
```

`oni2`ディレクトリ配下で、nodeの依存関係をインストールする。
Nodeバイナリをoni2プロジェクトが提供しているものを使うために、`npm install`を使わないらしい。

```bash
$ npm install -g node-gyp
$ node install-node-deps.js
```

ちなみに、筆者の環境だとビルドしたOnivim 2でターミナルがうまく動かなかったが、この手順からやり直すとうまくいった。
もし同じ状況になったら試していただきたい。
[参考](https://github.com/onivim/oni2/issues/1648)

### フロントエンドのビルド

ここからはEsyを使って依存関係のインストールやビルドを行っていく。

```bash
$ esy install

$ esy bootstrap

$ esy build
```

たまに、`esy bootstrap`のところで以下のようなエラーが出ることがある。

```bash
info building @opam/luv@github:bryphe/luv:luv.opam#8e9f2b0@d41d8cd9: done
error: build failed with exit code: 1
  build log:
    # esy-build-package: building: esy-cmake@0.3.5
    # esy-build-package: pwd: C:\Users\user\.esy\3\b\esy_cmake-0.3.5-5b613289
    # esy-build-package: running: "bash" "-c" "./build-windows.sh"
    C:\Users\user\AppData\Local\Temp\__esy-bash__377572354__1__.sh: 行 3: 対応する `"' を探索中に予期しないファイル終了 (EOF) です
    C:\Users\user\AppData\Local\Temp\__esy-bash__377572354__1__.sh: 行 4: 構文エラー: 予期しないファイル終了 (EOF) です
    error: command failed: "bash" "-c" "./build-windows.sh" (exited with 2)
    esy-build-package: exiting with errors above...

  building esy-cmake@0.3.5
esy: exiting due to errors above
```

原因は不明だが、筆者の環境ではもう一度実行したら成功した(原因を追いかける気力はなかった)。

# Onivim 2を使ってみる

## ターミナルの起動

## ファイルツリーの利用

## スニークモード

## 使ってみた感想
