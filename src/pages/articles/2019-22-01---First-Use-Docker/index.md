# Dockerを使ってみた

## Dockerってなんぞや

Dockerとは環境をコンテナ化する技術らしい。

もともとインフラエンジニアをやっていて、VMwareでがっつり仮想環境を構築したりしていたのでここらへんは得意だったはずなんだけど、さっぱりわからん。。

仮想環境とコンテナ環境って何が違うのか。

実は今までも何回か「Docker とは」で調べてQiitaを読んだりしていたのですが、やっぱりわからない！

ということで、触ってみることにしました。

## Dockerで何をするつもりか

きっかけとなった理由は2つ。

1つはVagrant環境がなんかおかしいから。

何がおかしいかというと、朝出社してPCを立ち上げるとVagrantのCPUがマックスになっていてファンがうるさいんだよね。

で、仕方なくVagrant reloadをするってことを続けてたんだけど流石になんとかしたいということでDockerにしようと思ったわけ。

何が原因なのかはここ1ヶ月くらいずっと調べてたけどさっぱりわからないし、家のPCは平気だからバージョンなのかな〜とも思ってる。

それで、2つ目の理由は、家と会社で同じ環境を作りたかったから。

会社の開発環境を持ち出しちゃダメだよって言われるとそうなのだが、私が作りたいのはブログ環境。

このブログはGatsby.jsというライブラリを使って作成しているのですが、ReactとかGatsbyはほとんど触ったことがないので、環境を持っていない。だから今回新たに環境を作るということでDockerにチャレンジしてみたくなった。

## Dockerのインストール

キャプチャとかコードとかは書きません。多分他のサイトを見た方がわかりやすいからね。

初心者がどうやって始めてどこに苦しみ、理解するのかということだけ書く。

まずはmacにDockerをインストールするところから。

プログラミング言語と違ってアプリをインストールするんだね。Linuxだとyumでインストールするらしい（軽くやったことある）

インストールが終わって、ターミナルからDockerがインストールされていることを確認。

docker -vとdocker-compose -vでバージョンが表示されればOK。

Dockerのコマンドを知っていたわけではないけど、とりあえず-vしておけばバージョンが見れるはずだからやってみたらできた。

## dockerfile作成

dockerfileってのをどっかに作るよ。どっかーにね

作る場所は特に決まってないけどそこを指定したり、後々起点になると思うから/docker/webディレクトリの中に作っておいた。

dockerfileの書き方は難しいから説明は省くけど、中身を見てイメージが作られるみたい。

例えばRUN yum -y install httpdとか。

これでapacheのイメージができあがり。

## イメージってなんぞや

正直dockerfileとイメージの部分が一番難しかった。

イメージっていうのはコンテナを作るためのレシピで、そのイメージを作るのがdockerfile。

2度手間じゃない？って思うかもしれないけど、dockerfileは複数のイメージを一気に作り出すもの。

そして、イメージはコンテナから作り出すこともできる。

なので、まずは基本的な環境イメージをdockerfileから作って、そのイメージでコンテナを作る。

そのコンテナを自分好みの環境にしたらイメージとして保存する。ということが可能になる。

## Docker Hubというサービス

しかもそのイメージはdocker hubというgithub的なサイトにリポジトリとして登録することができる。

docker hub を使えば他の人と環境を共有することもできるし、環境を過去のものに戻すこともできる。

実際docker hubがあれば必要なイメージをダウンロードすることができるのでわざわざ自分でdockerfileを作成する必要はないような気がする。

## Docker Composeについて

macにDockerをインストールすると、Docker Composeというソフトもインストールされる。

Docker Composeとはイメージを管理するためのもので、イメージを一気にコンテナにすることができる。

Dockerのみでイメージを管理しようとすると、例えばPHP環境が欲しいというときにApacheとMySQLのイメージでコンテナを作る。

しかし、実はあと1つ別のコンテナも作る必要があった場合忘れてしまう場合があるだろう。

Docker Composeにまとめておけば、環境ごとに必要なイメージをまとめて管理することができるので環境に漏れがなくなる。

使わなくてもいいが、あると便利だよってソフト。

## Dockerを使う流れのまとめ

* dockerfileを作る、もしくはDocker Hubから必要なイメージを探す。

* dockerfileからイメージを作る、もしくはDocker Hubからイメージをプルしてくる。

* docker-composeに必要な環境を書く。

* docker-composeでイメージから一気にコンテナを作る。

といった流れ。

そこからは通常通りの開発ができるはず。

例えばLaravelプロジェクトを作って、DB情報を入れて、マイグレートをする。みたいな感じ。

DBのユーザやパスワードはdocker-composeで管理できるし、ポートも管理できるからApacheのポート80で書いておけばlocalhostですぐにローカル環境をブラウザ表示できる。

## 苦しんだ点

コンテナという環境の理解に苦しんだのは間違いないんだが、その中でも特にコンテナ間の連携に戸惑った。

例えばApacheやMySQLの設定ファイルはどこにあるのか。

ホストとコンテナのプロジェクトルートはどこにあるのか。どのように連携させているのか。

こういったコンテナの連携の部分はまだ微妙なところがあるので、これから使っていくうちに理解していきたい。

## あとやること

まだ家のPCに環境を立ててないからそっちでイメージをプルしてみようと思う。

あと聞いた話によると、DockerとCIを使ったテストが便利らしいのでそれも試してみようかなと。

あくまでのインフラは時間をかける部分ではないので、シンプルに作ったり壊したりして理解していくのが良い気がする。