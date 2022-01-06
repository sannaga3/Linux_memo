### Docker

Docker Ecosystemと呼ばれる複数のソフトウェアをまとめてDockerと呼ぶ

・メリット

```
OSを含むソフトウェアをパッケージとしてインストールして仮想環境の構築を簡素化することで、すぐに利用できる。
作成した環境を共有できる。
webの３層構造を１つのサーバー上で構築可能(webサーバ、アプリケーションサーバ、DBサーバ)
```

##### Docker image

https://qiita.com/zembutsu/items/24558f9d0d254e33088f

```
コンテナを立ち上げる為のソースコード、ライブラリなどを含んだ読み取り専用のファイル集。
image自体の編集はできず、新たなイメージを作成して保存する。
image layerという階層構造になっており、機能の追加は階層の一番上に積まれる。
```

##### Docker container

```
image を元に実体化したもの。編集が可能。
起動元のOSから独立して仮想環境を立ち上げる。
```

##### ハイパーバイザー

https://www.idcf.jp/words/hypervisor.html

```
仮想マシン (VM) を作成および実行するソフトウェア。
ホストPCで仮想マシンを管理し、CPU、メモリー、ストレージなどを割り当てる。
OSよりも上位の層からプログラムを実行するので、仮想化を実現するためのOSを必要としない。
```

##### 3種類の仮想化型(ホスト型、ハイパーバイザー型、コンテナ型)

https://qiita.com/Pentas/items/5f558deeddcfef814b3e  
https://www.fit-works.co.jp/lp/rampart/contents/virtualization.html  
https://knowledge.sakura.ad.jp/13265/  

```
ホスト型

OS上に仮想ソフトをダウンロードし、仮想化ソフト上に仮想マシンを構築。
任意のOSをインストール可能。
ホストOSに負荷がかかると、仮想マシンのパフォーマンスにも影響する。

Oracle　VirtualBox
VMWare　WorkstationPlayer


ハイパーバイザー型

サーバー上にハイパーバイザーと呼ばれるソフトウェアをインストールし、そのソフトウェア上で仮想マシンを動作させる。
ホストOSの役割をハイパーバイザーが担う為、ホストOSを必要としない。
起動が早く、処理が軽量、使用メモリが少ない。
高度な管理には別途管理ツールの導入が必要。

Microsoft　Hyper-V
VMWare  　 vSphereHypervisor


コンテナ型

ホストOS上から仮想化ソフトウェアとしてコンテナエンジンをインストールし、「コンテナ」という入れ物の中でアプリケーションの実行環境を構築する。
ミドルウェアやOSの情報をコンテナに格納し、OS部分まで共通化することで仮想化ソフトウェアとゲストOSが不要になり、高速での起動や停止が可能。
コンテナ間でOSを統一する為、複数のOSを扱うことはできない。

Docker
```

##### CentOSへのインストール

https://matsuand.github.io/docs.docker.jp.onthefly/engine/install/centos/

```
yum install -y yum-utils                                                                yum-utilsパッケージのインストール
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo   リポジトリファイルのインストール
yum-config-manager --enable docker-ce-nightly                                           最新版（nightly）または テスト版（test）リポジトリの有効化
yum install docker-ce docker-ce-cli containerd.io                                       Docker Engine と containerd の最新版をインストール
systemctl start docker                                                                  Docker起動
systemctl status docker                                                                 Dockerの状態確認  Active: active(running)で起動中

導入後

・イメージ、コンテナの作成削除
docker run hello-world                                                                  テスト用イメージをダウンロードして実行
docker pull centos:6                                                                    centosのインストール　https://hub.docker.com/_/centos
docker images                                                                           centosのイメージが追加されているのを確認
docker create centosのimageID echo 'hello'                                              イメージを元にコンテナを作成
docker ps -a                                                                            作成したコンテナを確認
docker start -a コンテナID                                                               コンテナを開始して結果を表示
docker rm コンテナID                                                                     コンテナの削除
docker rmi イメージID                                                                    イメージの削除(削除するイメージを使用したコンテナが残っているとエラーになる)

・起動コンテナの確認、停止、離脱
docker run -it centos                                                                   -i コンテナ開始後ターミナル入力可能 -t ターミナルのユーザ名表示がコンテナIDに変わり見やすくなる
新しいターミナルを起動し、docker -ps で起動中コンテナを確認すると、STATUSで起動時間が確認できる
ctrl + p => ctrl + q でコンテナを抜け、もう片方でdocker ps で確認すると起動中のままを確認できる
docker attach コンテナID                                                                 コンテナに再接続
docker exec -it コンテナID bash                                                          起動中のコンテナに接続
docker stop コンテナID                                                                   コンテナを停止
docker run -d centos sleep 50                                                           centosのイメージから50秒間停止するコンテナを作成しバックグラウンドで実行
docker kill コンテナID                                                                   stopでは切れないのでkillで強制終了

・コンテナの変更を保存
docker run -it ubuntu                                                                   ubuntuのコンテナ作成
touch sample.txt                                                                        新規ファイル作成
docker commit コンテナID イメージ名(my_ubuntu)                                             コンテナの変更を新たなイメージとして作成
docker run -it my_ubuntu => ls                                                          作成したファイルが保存されているのを確認
```

##### Dockerコマンド

```
docker pull                          Docker Hubからイメージの取得
       create                        コンテナの作成
       start                         コンテナの起動
       run                           pull, create, startを一括で実行
       stop                          コンテナの停止
       rmi                           イメージの削除

       run -it                       コンテナ起動後ターミナルから入力
       exit                          コンテナを停止して抜ける
       ctrl + P → ctrl + Q           コンテナを停止せずに抜ける
       run -d                        コンテナをバックグラウンドで実行
       tag image1 image2             イメージを別名で新たに作成
       docker rmi (-f) image名(imageID)        ローカル上のイメージを削除
       docker commit コンテナID image名(:tag)   コンテナから新しいイメージの作成

       images                        ダウンロードしたイメージを全て確認
       inspect                       イメージの詳細を表示
       ps                            起動中のコンテナを表示
       ps -a                         コンテナを全て表示
       rm コンテナID                  コンテナ削除
       start コンテナID or コンテナ名   コンテナ開始
       stop コンテナID or コンテナ名    コンテナ終了
       kill コンテナID or コンテナ名    コンテナ強制終了

       build -t タグ名                現在のフォルダ上のDockerfileからイメージをビルド
	                             -f でDockerfile以外を指定
       docker system prune           コンテナを全削除
       docker image prune            イメージを全削除
```

