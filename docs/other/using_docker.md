# 概要

## 本

- [Docker | Adrian Mouat, Sky株式会社 玉川 竜](https://amzn.to/2PZ7xAy)
- 参考
  - [using-docker](https://github.com/using-docker)

## かかった時間

- x 時間

## 読む前の状態

- xxx

## 感想

- xxx

# 読書メモ

## 1: コンテナとはなにか、そしてなぜ注目されているのか

- コンテナ
  - コンテナは、アプリケーションを依存対象とともにカプセル化したもの
- コンテナと VM の違い
  - 仮想化にはホスト型とハイパーバイザ型がある
    - ホスト型
      - ホスト型は OS 上に土台となるソフトウェアをインストールし、そのソフトウェア上で仮想マシンを稼働させる方式
      - ハードウェアにアクセスするにはホスト OS を経由しないといけないため、余計なオーバーヘッドがかかる
    - ハイパーバイザ型
      - サーバーへ直接ハイパーバイザをインストールし、仮想マシンを稼働させる方式
      - こちらの場合は（ホスト OS を介さず）ハードウェアを直接操作可能
    - 参考: [ホスト型とハイパーバイザー型の違いは何？VMware vSphere Hypervisor の概要 | Think IT（シンクイット）](https://thinkit.co.jp/story/2012/10/17/3722)
    - 上記の通り、それぞれの VM には OS、実行するアプリケーション、必要な支援ライブラリの完全なコピーが必要となる
  - コンテナの場合
    - VM の場合とは異なり、ホストのカーネルは実行されるコンテナと共有される
    - また、アプリケーション間で共通のライブラリを使う場合は、冗長なコピーを持つのではなく、共有できる
    - コンテナエンジンは、ハイパーバイザ上の VM と同じようなやり方で、コンテナの起動や終了を受け持つ
    - コンテナ内で動作しているプロセスは、ホスト上で直接動作しているプロセスと同等であり、ハイパーバイザの実行に伴うオーバーヘッドがない
  - VM が目的としているのは、異なる環境を完全にエミュレートすること。コンテナが目的としているのは、アプリケーションをポータブルにし、単体で動作できるようにすること
- Docker とコンテナ
  - Docker はポータブルなイメージと、ユーザーフレンドリーなインターフェイスを中心とする様々な方法で既存のコンテナ技術をラップして拡張し、コンテナの生成と配布のための完全なソリューションを生み出している
  - Docker というプラットフォームには、2 つの構成要素がある
    - Docker エンジン
      - コンテナの生成と実行を受け持つ
      - 動作中のコンテナへの高速で便利なインターフェイスを提供する
    - Docker Hub
      - コンテナを配布するためのクラウドサービス
- Docker のプラットフォームは 64bit Linux のみ
  - Docker を利用するコンピュータでは、64bit の Linux ディストリビューションを動作されなければならず、コンテナもすべて 64bit Linux でなければならない
  - そのため、Windows もしくは Mac OS のユーザーは、VM 内で Docker を動作させることになる

## 2: インストール

- Docker for Mac/Windows
  - この本が書かれた時点では Docker Toolbox をインストールし、Virtual Box 上で Linux のホストを動作させるやり方だった。現時点では、Docker for Mac/Windows を利用して、それぞれのプラットフォーム固有の仮想化の機能を使うことで、ほぼ Linux と同様の感覚で Docker を使えるようになる
- Docker for Mac
  - Docker for Mac は Mac OS Yosemite に組み込まれたネイティブの仮想化機構である Hypervisor Framework を利用し、Docker エンジンの動作環境を提供する
  - ユーザーからは、仮想環境のホストの存在をほぼ意識することなく、Docker を Mac OS 上で利用できる

## 3: はじめの一歩

- 初めてのイメージ実行
  - `docker run debian echo "Hello World"` を実行する
  - 呼び出したコマンドが `docker run`
  - `debian` が使いたいイメージの名前
  - イメージがダウロードされると、Docker はそのイメージを実行中のコンテナに変え、その中で指定されたコマンドを実行する
  - [run — Docker-docs-ja 17.06.Beta ドキュメント](http://docs.docker.jp/engine/reference/commandline/run.html)
- コンテナ内のシェルを使う
  - `docker run -i -t debian /bin/bash`
  - `-i` と `-t` で Docker に対して tty 付きのインタラクティブセッションを要求する
  - シェルを終了するとコンテナも停止する
    - コンテナが動作するのは、そのコンテナのメインプロセスが動作している間だけ
- コンテナの一括削除
  - `docker rm $(docker ps -aq)`
- コンテナのファイル変更や設定を、新しいイメージに収容する
  - `docker commit cowsay test/cowsayimage`
  - 上記だと cowsay がコンテナの名前、test が保存するリポジトリの名前、cowsayimage がイメージの名前
  - [commit — Docker-docs-ja 17.06.Beta ドキュメント](http://docs.docker.jp/engine/reference/commandline/commit.html)
- Dockerfile からのイメージの構築
  - FROM 命令で使用するベースイメージを指定
  - RUN コマンドはイメージ内で実行するシェルのコマンドを指定
  - Dockerfile と同じディレクトリから `docker build` コマンドを実行すればイメージを構築できる
  - [build — Docker-docs-ja 17.06.Beta ドキュメント](http://docs.docker.jp/engine/reference/commandline/build.html)
  - 普通に docker build するとエラーが出るので `FROM debian/eol:wheezy` に変更した
    - [sources list in wheezy should be switched to archive · Issue #65 · debuerreotype/docker-debian-artifacts](https://github.com/debuerreotype/docker-debian-artifacts/issues/65)
  - ENTRYPOINT
    - ENTRYPOINT にはコンテナが実行するファイルを指定する
    - [ENTRYPOINT - Dockerfile リファレンス](http://docs.docker.jp/engine/reference/builder.html#entrypoint)
  - COPY
    - [COPY - Dockerfile リファレンス](http://docs.docker.jp/engine/reference/builder.html#copy)
  - LABEL
    - MAINTAINER は deprecated になったので LABEL を使う
    - [LABEL - Dockerfile リファレンス](http://docs.docker.jp/engine/reference/builder.html#label)
- Union File System(UFS)
  - union file system は、複数のファイルシステムをオーバーレイして、単一のファイルシステムとしてユーザーに見せてくれる
  - Docker のイメージは、複数のレイヤから構成される。イメージの各レイヤは、リードオンリーのファイルシステム
- レジストリ、リポジトリ、イメージ、タグ
  - レジストリ
    - イメージのホスティングと配布を受け持つサービス。デフォルトのレジストリは Docker Hub
  - リポジトリ
    - 関連するイメージの集合（自分の場合は ymeguro リポジトリを持つ）
  - タグ
    - リポジトリ内のイメージに与えられる、アルファベット及び数値からなる識別子
- イメージを再構築して、Docker Hub にアップロード
  - [ymeguro/cowsay - Docker Hub](https://hub.docker.com/r/ymeguro/cowsay)
- イメージの名前空間
  - push された Docker のイメージが属する名前空間には 3 つの種類がある
    - user
      - `ymeguro/cowsay` のように、文字列と / でプレフィックスされた名前は "user" の名前空間に属する
      - ユーザーによってアップロードされたイメージが属する
    - root
      - プレフィックスや / を持たない debian や ubuntu といった名前は "root" の名前空間に属する
      - この名前空間は Docker Inc. が管理しており、公式イメージのために予約されている
    - サードパーティのレジストリがホストしているイメージ(Docker Hub がホストしているものではない)
      - ホスト名もしくは IP がプレフィックスになっているイメージ
      - 例えば localhost:5000/wordpress はローカルのレジストリでホストされている WordPress のイメージ
  - ボリューム
    - データを永続化し、コンテナとホスト、あるいは他のコンテナとの間で簡単に共有するために、ボリュームを利用する
    - ボリュームは通常の union file system の一部ではなく、ホストに直接マウントされるファイルもしくはディレクトリ
      - つまり、ボリュームは他のコンテナと共有でき、すべての変更は直接ホストのファイルシステムに対して行われる
    - ディレクトリをボリュームとして宣言する方法は 2 つある
      - Dockerfile の中で VOLUME 命令を使う方法
      - docker run で -v フラグを指定する方法
    - デフォルトでは、指定したディレクトリやファイルは、ホスト上の Docker をインストールしたディレクトリ(通常は /var/lib/docker/)にマウントされる
