<hr>

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
ctrl + p => ctrl + q でコンテナを抜け、もう片方でdocker ps で確認すると起動中の状態を確認できる
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

##### Dockerfile

イメージとコンテナの構築方法を定義する設定ファイル

```
FROM        ベースイメージの指定
RUN         イメージからコンテナを作成し実行する
COPY        ローカルからファイルをコンテナにコピー
WORKDIR     コンテナでの作業用ディレクトリの指定
ENTRYPOINT  コンテナ起動時の実行コマンド指定(デフォルトは bin/bash/sh -c(後続のshellを実行))
CMD         コンテナ作成時にエントリポイントに渡す引数、コマンドの指定
```

##### Pythonのflaskを使う

```
①pythonのインストール

vim Dockerfile
docker build -t flask_app .     Dockerfileを元にイメージの作成

----------- 内容 ------------------
FROM ubuntu:bionic

RUN apt-get update -y && apt-get install -y python3-pip python-dev
----------------------------------

* 上記でcentOS8(virtualBox)の時刻がズレていてインストールできないエラー
E: Release file for http://security.ubuntu.com/ubuntu/dists/bionic-security/InRelease is not valid yet (invalid for another 3d 9h 16min 1s). Updates for this repository will not be applied.

対処法
https://server-network-note.net/2021/02/centos8-set-ntp/

docker run -it flask_app
python３                        pythonの起動を確認したら ctrl + d で抜ける


②flaskのインストール

vim requirements.txt

----------- 内容 ------------------------
Flask==1.1.2        flaskのバージョンを記載
----------------------------------------


vim Dockerfile

----------- Dockerfile 追記 -------------
COPY ./requirements.txt requirements.txt    カレントディレクトリのファイルをコンテナにコピー

RUN pip3 install -r requirements.txt        requirements.txtの内容をコンテナにインストール
----------------------------------------

docker run -it flask_app
from flask import Flask      flaskをpythonで使えるようにimport


③flaskアプリの設定

vim app.py                   flaskアプリ作成

----------- app.py ---------------------
from flask import Flask

app = Flask(__name__)

@app.route('/hello')

def hello_world():
    return 'Hello World'

if __name__ == '__main__' :
    app.run(host='0.0.0.0', port=5000)
----------------------------------------


④アプリの起動とポートの共有

vim Dockerfile

----------- Dockerfile 追記--------------
COPY . .                   作業ファイルを全てコンテナにコピー

ENTRYPOINT ["python3"]

CMD ["app.py"]             ENTRYPOINTの引数 => python3 app.pyでファイルを実行してアプリ起動
----------------------------------------


docker build -t flask_app .
docker run -it アプリのイメージID
docker run -d -p 5000:5000 flask_app    アプリをバックグラウンドで起動し、コンテナのポートをホスト側(VirtualBoxのCentOS)で接続可能にする。
* runコマンドのオプション  https://docs.docker.jp/engine/reference/commandline/run.html

VirtualBoxのCentOSの設定 => ネットワーク => ポートフォワーディング にTCP 5000ポートを追加(ホストポートとゲストポート)

http://localhost:5000/hello に接続すると「hello world」が表示される
参考  https://qiita.com/KMim/items/0b84a32d2d8f0399b391


④イメージ内のファイル整理

Dockerfileやアプリのファイルがルートディレクトリに配置されている為、appディレクトリを作成し移動する。
docker stop コンテナID
vim Dockerfile

----------- Dockerfile 追記--------------
WORKDIR /app               この行を追記   デフォルトのコピー先ディレクトリを指定

COPY . .
----------------------------------------

docker build -t flask_app .
docker run -d -p 5000:5000 flask_app
docker exec -it コンテナID bash   とすると作業ディレクトリが /appになっている。cd .. でルートディレクトリに戻るとわかりやすい。


⑤ローカルの変更を簡単に反映する方法

（コンテナの停止） => ローカルの変更 => ビルド（イメージの作成） => コンテナの作成・起動
上記の一連の流れを短縮する

vim app.py

----------- app.py 変更 -----------------
def hello_world():
    return 'Hello'        ここを変更  "Hello World" => "Hello"
----------------------------------------

docker stop コンテナID                                   変更したいコンテナが起動していたら停止しておく
docker run -d -p 5000:5000 -v $(pwd):/app flask_app     現在のディレクトリをコンテナのディレクトリと紐付けることができる（$pwdをflask_appの /appに紐付けて変更を反映する）
```

<hr>

### virshコマンド

https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/virtualization_host_configuration_and_guest_installation_guide/sect-virtualization_host_configuration_and_guest_installation_guide-host_installation-installing_kvm_packages_on_an_existing_red_hat_enterprise_linux_system

仮想マシンとハイパーバイザのリソースを管理する為のコマンド

```
virt-manager   仮想マシン管理ソフト。ライブラリ libvirt-clientを使用してGUIで利用する
virt-install   仮想マシン作成
libvirt        libvirtd デーモンを提供することで、仮想化機構との連携を行うライブラリ。
```

```
yum install virt-manager
yum install libvirt virt-install

virth list         ドメイン一覧表示
virsh start VM     仮想マシン起動
virsh console VM   virshコマンドでVMに接続
virsh shutdown VM  仮想マシンの停止
virsh destroy VM   仮想マシンの削除
virsh dominfo VM   仮想マシンのドメインに関する情報表示
virsh domid VM     仮想マシンのドメインID表示
virsh dommstate VM 仮想マシンの実行状態確認
```

<hr>
