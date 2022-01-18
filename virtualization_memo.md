<hr>

### Docker(101)

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
       cat /etc/os-release           ホストのOS情報を表示(コンテナ内)

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

virth list         ドメインID、ドメイン名、実行状態の一覧表示
virsh start VM     仮想マシン起動
virsh console VM   virshコマンドでVMに接続
virsh shutdown VM  仮想マシンの停止
virsh destroy VM   仮想マシンの削除
virsh dominfo VM   仮想マシンのドメイン、CPU,
メモリなどに関する情報表示
virsh domid VM     仮想マシンのドメインID表示
virsh domstate VM  仮想マシンの実行状態確認(シャットオフ、実行中など)
```

<hr>

### ブートプロセスとsystemd

##### ブートプロセス

コンピュータを操作できるようにシステムを順次起動していく流れ  

https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-ja-4/s1-boot-init-shutdown-process.html  

https://log-bennkyou.com/175/  

https://kimamani89.com/2020/05/01/%E3%80%90linux%E3%80%91%E3%83%96%E3%83%BC%E3%83%88%E3%83%AD%E3%83%BC%E3%83%80%E3%83%BC%E3%81%8B%E3%82%89%E3%82%AB%E3%83%BC%E3%83%8D%E3%83%AB%E3%81%BE%E3%81%A7%E3%81%AE%E8%B5%B7%E5%8B%95%E3%83%97/  

##### BIOS

マザーボードのROMに組み込まれたプログラム。BIOSによってOSが読み込まれ、メモリにロードされてPCは起動する。

マザーボード上のコントローラ（CPU、メモリコントローラ、外部キャッシュメモリ、割り込みコントローラ、ビデオ・コントローラ、リアルタイム・クロック、パラレル/シリアル・ポート、ディスク・コントローラ、バスコントローラ、キーボード コントローラなど）にアクセスし、使用可能な状態を確認する。

https://www.pc-koubou.jp/magazine/1257




##### Linuxのブートプロセス

```
①電源ON

②BIOS/UEFIが起動   OSを起動するためのプログラムを実行する。
BIOSの場合...ROM
・メモリのチェック
・ハードウェアの設定の読み込み（自己診断と初期化）
・起動デバイスのチェック
・起動デバイスのブートローダーの実行

UEFIの違い

・GUIでのセットアップ
・起動時間と再開時間の短縮
・1MBのメモリ制約開放 => セキュリティなどを含めたシステムの機能強化が可能
など

③ブートローダの起動
OSをメモリにロードする。

④カーネルの起動
メモリの初期化を行った後、初期RAMディスク(initrd)をメモリに展開し、仮の必要最低限なルートファイルシステムをマウントする。

https://keichi.dev/post/linux-boot/
https://www.konosumi.net/entry/2020/09/27/223948

⑤カーネルがinitプロセス(最初のプロセス PID：1)を起動する。initはシステム上のすべてのプロセスの親にあたるプロセスであり、/etc/initディレクトリからジョブ設定を読み取る。

 https://docs.oracle.com/cd/E39368_01/admin/ol_about_bootconf.html

ジョブ...initが読み込む一連の命令のこと。命令にはプログラム（バイナリファイルまたはシェルスクリプト）とイベントが含まれる。イベントが実行されることで対応するプログラムが起動する。

イベント...アプリケーションやUSBデバイスなどの接続、起動状態のこと。

 https://mag.osdn.jp/08/02/18/0145226#div-gpt-osdn_mag_rec-article-middle

「/etc/inittab」ファイル
 initプロセスが最初に参照するファイルで、記述されたプロセスを順に起動する。デフォルトのランレベルの指定、デバイスなどの初期化、initの起動、ブート時の処理、ランレベルごとのrcスクリプトの実行などが記載されている。

 https://linuc.org/study/knowledge/504/

initプロセスの種類3つ
・SysVinit
  カーネルを読み込んだ後のプロセス実行部分を指す。initプロセスの開始 => /etc/inittabファイルを読み込み、シェルによるシステムの初期化 => ランレベルを元にrcスクリプトの実行(rcスクリプトは各ランレベルごとに作成され、実行する内容が記述されている)

  https://qiita.com/miyuki_samitani/items/559fb28faa3888bce526
  https://users.miraclelinux.com/technet/document/linux/training/1_1_3.html

以下2つはSysVinitの改良番
・Upstart

  SysVinitは直列起動だが、並列起動により起動時間が短縮された。イベントをトリガーとしてジョブを実行し、プロセスを起動させる。
  pid = 1 の初期プロセスの為のみに存在し、以降の設定は別ファイルに記述し、並列処理で起動する。

  https://qiita.com/miyuki_samitani/items/ff81846f44c083564dbc
  https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/migration_planning_guide/ch04s02s03

・systemd
  現在主流のinitプロセス。Upstartと同じく各プロセスをユニットとして並列起動する。
  より無駄を減らして高速起動を可能。システム管理の共通化。親子関係にあるプロセスの起動・停止制御など。

  https://tech.pjin.jp/blog/2020/11/11/linux-system-boot-2
  https://kcfran.com/2021/03/10/linux-systemd-1/

initプロセスのランレベル
  0     システム停止
  1     シングルユーザーモード  OSへのログインはrootのみ
  2,3   マルチユーザーモード    root以外のユーザもログイン可
  4     なし
  5     GUIモード
  6     システム再起動
```

##### systemctlコマンド

systemdで制御しているサービスを管理するコマンド

```
systemctl start サービス名       サービス起動
systemctl stop サービス名        サービス停止
systemctl restart サービス名     サービス再起動
systemctl reboot サービス名      サービス再起動
systemctl poweroff サービス名    シャットダウン
systemctl getdefault サービス名  デフォルトのブートターゲットを表示
systemctl setdefault サービス名  デフォルトのブートターゲットを変更
systemctl isolate サービス名     ブートターゲットを変更

* ブートターゲット = ランレベル
  0     poweroff.target    システム停止
  1     rescure.target     シングルユーザーモード  OSへのログインはrootのみ
  2,3   multi-user.target  マルチユーザーモード    root以外のユーザもログイン可
  4     なし
  5     graphical.target   GUIモード
  6     reboot.target      システム再起動

デフォルトのランレベル変更2つ
systemctl set-default name.target
default.targetファイルの作成 => シンボリックリンクを作成

wall    ログイン中のユーザ全員のターミナルにメッセージを表示させる(緊急のサーバメンテナンスなどでシステムを停止する場合など、至急の通知に利用する)
https://horus531.hatenadiary.org/entry/20110110/1294647041

```

##### demsg関連コマンド

システム起動時のカーネルが出力するメッセージを表示するコマンド

```
dmesg | less              システム起動時のカーネルが出力するメッセージを表示

runlevel                  ランレベル表示

init ランレベル(数値)  ランレベル変更(初期値は3)     init 1 にするとrootユーザー以外ログイン不可になる。 init 6をするとPCの再起動。

・ランレベル変更方法１
systemctl list-unit-files --type=service       起動できるサービス一覧を表示。STATEが起動状態を表す。
https://www.suzu6.net/posts/303-systemctl-list/

systemctl status サービス名                      サービスの状態表示  =>  systemctl start 起動

systemctl get-default                          3の場合 => multi-user target

systemctl set-default rescue.target            defaultの設定を1(rescue.target)に変更

・ランレベル変更方法2
ls -l /etc/systemd/system/ でもデフォルトのランレベルを確認できる =>  default.target -> /usr/lib/systemd/system/multi-user.target   デフォルトターゲットがマルチユーザーをシンボリックリンクに登録している
  ↓
rm /etc/systemd/system/default.target
  ↓
ls /usr/lib/systemd/system/multi*   =>  /usr/lib/systemd/system/multi-user.targetの存在を確認
  ↓
ln -s /usr/lib/systemd/system/multi-user.target /etc/systemd/system/default.target   <=  /etc/systemd/system/のdefault.targetという名前で /usr/lib/systemd/system/multi-user.gargetのシンボリックリンクを作成(ln -s リンク元 登録名)
  ↓
systemctl reboot で再起動してrunlevelを確認

wall 'メッセージ'     ターミナルを複数開き、違うユーザでログインして左記コマンドを打つと、全ターミナルにメッセージが表示される
```

<hr>

### Linuxの操作

Linuxのインストール設定でできること

```
OSで使用する言語
キーボードの言語
インストール先ハードディスク
タイムゾーン
ネットワークホスト名
ユーザ情報、rootユーザ情報
インストールするソフトウェアパッケージ
```

##### Linux操作のコマンド

rootユーザで実行するコマンド。ローカル環境以外で使うことはまずない。

```
shutdown 時間     now、+1(1分後)、17:18など。shutdownは停止のみで電源はオフにならない
shutdown ＋20 "System Maintenance Start"         シャットダウンと時刻の通達
shutdown -h +1   1分後に電源オフ
shutdown -H +1   1分後にシャットダウンするが電源オフにはしない

shutdown -r now  今すぐシャットダウン後に再起動
shutdown -c "Schedule changed to tommorrow"     予定時刻のシャットダウンをキャンセル。Ctrl+CでもOK
shutdown -k now  シャットダウンは行わず、ログイン中のユーザーへメッセージを送る(事前連絡に利用する)

シャットダウンの予定が入ると、その予定をキャンセルしないと新たなシャットダウン予定は予約できない。
シャットダウンした後は Ctrl + C でシャットダウンをキャンセルできる
https://eng-entrance.com/linux-command-shutdown

reboot          OS再起動
poweroff        OSを停止して電源を切る
halt            OSをシャットダウンできる状態にする(プロセスは停止するが電源は落とさない
halt -p         OSを停止して電源を切る。poweroffやshutdown -h と同じ

SSH系ファイル
~/.ssh/known_hosts      初めて接続するサーバーの場合、サーバー情報が保存される
~/.ssh/authorized_keys  公開鍵認証でクライアントの公開鍵を登録
~/.ssh/id_rsa           秘密鍵
~/.ssh/id_rsa.pub       公開鍵
```

##### dockerでsystemdを使ってみる

centOSでは初期設定でsystemdがオフになっている為、下記URLの Dockerfile for systemd base image のコードをコピーする

https://hub.docker.com/_/centos

```
su -
cd
mkdir ssh-centos
cd ssh-centos
vim Docekerfile

----------- Dockerfile 記述 -----------------
FROM centos:8
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
----------- app.py 変更 -----------------

systemctl start docker
docker build -t ssh-centos .
docker images                                      sh-centosのイメージがあることを確認
docker run -d --privileged ssh-centos /sbin/init (ポートに接続できなかった為、エラー対処欄のコマンドを入力すること)   デフォルトでオフ(unprivileged)になっているsystemd(systemctlコマンド)を--privilegeオプションで利用可能にする。
https://genchan.net/it/virtualization/docker/18388/

docker ps                                          コンテナの起動を確認
docker exec -it コンテナID bash                      作業コンテナへアクセス
yum install openssh-server -y                      サーバ機能を提供する openssh-server パッケージをインストール(外部からssh接続が可能になる)
https://nwengblog.com/rhel-ssh/

systemctl start sshd                               サービスの起動
systemctl status sshd                              ステータスがactiveであることを確認
passwd                                             rootユーザで接続するためのパスワードを設定(必要であれば yum install -y passwd)
docker inspect コンテナID                           コンテナ情報に表示されるIPアドレスを使ってssh接続を行う。"IPAddress": "172.17.0.2",

ssh root@172.17.0.2  => 失敗  ssh: connect to host 172.17.0.2 port 22: Connection refusedのエラーが発生（エラー対処へ）
ssh root@172.17.0.2  => 成功  passwdで設定したパスワードを入力するとログインできる

パスワードを用いたログイン完了

----------- エラー対処 -----------------
yum -y install lsof                                lsofパッケージのインストール
lsof -i -P                                         22番ポートの状態確認。接続側のポートでsystemctl start sshd をしてしまったと推測。
docker exec -it コンテナID bashでコンテナに入った後systemctl start sshdが通らず、
別ターミナルを立ち上げて先のコマンドを実行したのが間違い。コンテナ内でsystemctl start sshdを実行できる方法を探すべきだった。

docker run -itd --privileged -p 2222:22 --name ssh-centos --hostname centos8 centos:latest /sbin/init
上記コマンドでポート指定して起動することで、systemctl start sshd が通った。
https://genchan.net/it/virtualization/docker/18314/

↓
次のエラー。コンテナ内でpasswdコマンドが無い。
ls /bin | grep passwd でコンテナ内の/bin /sbinの登録コマンドにpasswdが無いのを確認。
yum install -y passwd でpasswdコマンドをインストール。/bin にコマンドが追加された。
https://stackoverflow.com/questions/60900968/bin-sh-passwd-command-not-found

↓
次のエラー。ターミナルを新たに起動してVirtualBoxのcentOS8に接続できない。
kex_exchange_identification: Connection closed by remote host
接続側のターミナルでsystemctl stop sshdをしてしまっていたのでsystemctl start sshdで接続できた。
--------------------------------------


鍵を用いたログイン方法
docker exec -it コンテナID bash でコンテナに入った状態からスタート

vi /etc/ssh/sshd_config  (コンテナ側)   => コマンドモードで /Pass として PasswordAuthentication の設定を探して yesから no に変更し、コメントアウトを解除。
systemctl restart sshd  (コンテナ側)
ssh root@172.17.0.2  (接続側) で root@172.17.0.2: Permission denied になるのを確認。(パスワードでの接続をnoにした為)
パスワード接続ができないのを確かめたら設定を一度元に戻す(公開鍵の登録にパスワードが必要)

ssh-keygen  (接続側)        ~/.ssh/  ディレクトリに公開鍵(id_rsa.pub)と秘密鍵(id_rsa)が作成される
cat ~/.ssh/id_rsa.pub  (接続側)                             公開鍵の確認
ssh-copy-id -i ~/.ssh/id_rsa.pub root@172.17.0.2 (接続側)   コンテナ側に公開鍵を登録
ls ~/.ssh/ (コンテナ側)                                      authorized_keys(コピーした公開鍵)の確認
ssh root@172.17.0.2    パスワード入力を求められず、公開鍵認証で接続できる
```

<hr>

##### プロセスの生成、監視、終了

```
コマンド &           バックグラウンドプロセスとしてコマンドを実行(バックグラウンドとして起動することでターミナル入力できる状態を保つ)
jobs                バックグラウンドのジョブを確認できる
bg                  一時停止中のジョブをバックグラウンドで再開
fg                  バックグラウンドで実行中のジョブをフォアグラウンドへ移行
kill %n             n番目のジョブを削除
kill −19 %n         n番目のジョブを停止

top                 実行中プロセスのCPU使用率などを表示  =>  k => pid ＋ enter でプロセス削除
ps                  実行中のプロセスを表示
pstree              親プロセスと子プロセスの派生を階層構造で表示。  yum install psmisc でインストール
pstree  -p          階層構造の表示にプロセスIDを追加表示
pstree  プロセスID    対象プロセスより下位の階層構造を表示
uptime              システム稼働時間を表示。 現在時刻、システム稼働時間、ログイン中のユーザ数、ロードアベレージ(過去１分、5分、15分のシステム負荷)

pgrep               プロセス名を指定してプロセスIDを検索   pgrep sleep =>  sleepコマンドを使っているプロセスを検索 => pkill コマンド名

kill                プロセスにシグナルを送信
kill -1             再起動
kill -2             割り込み(ctrl + c)
kill -9             強制終了
kill -15            終了
kill -19            停止
pkill               プロセス名を指定してシグナル送信
killall             コマンド名でプロセスにシグナルを送る。  killall vi => viコマンドを使っているプロセスを全て停止
                    1 再起動   6 中断   9 強制終了   15	終了   17 停止   18 再開
                    https://uxmilk.jp/52198
nohup コマンド &     ターミナルを閉じたりログアウトしてもコマンドの処理を継続することができる。処理結果を $(HOME/nohup.out) に出力する
                    https://atmarkit.itmedia.co.jp/ait/articles/1708/24/news022.html

tmux                ターミナル上に複数のターミナルを立ち上げる    yum install tmux
tmux ls             立ち上げたセッション一覧表示
tmux new-session    セッションを新たに立ち上げる
tumx new-session -s name      nameという名前のセッション立ち上げ    tumx new -s name でもok
tmux kill-session -s name     セッション名を指定して削除
tmux attach -t name           セッション名を指定して再接続(リモートなどで同一OSにログインして作業している場合、他の人のターミナルを表示しながら作業することも可能)
tmux rename-session -t name   セッション名を変更    tmux rename -t s1 s2(s1からs2へセッション名変更)

セッション立ち上げ後のコマンド
ctrl + d or exit    ウィンドウ、セッションの終了
ctrl + b -> s       セッション一覧表示
ctrl + b -> c       新規ウィンドウ作成
ctrl + b -> &       セッション破棄
ctrl + b -> "       上下に分割
ctrl + b -> %       左右に分割
ctrl + b -> %       左右に分割
ctrl + b -> 十字キー ウィンドウ内の移動
ctrl + b -> d       セッションを一時的に中断してメインに戻る
ctrl + b -> w       セッション一覧から表示するセッションを選択
ctrl + b -> t       ターミナル一面に現在時刻表示
```

##### 実際にプロセスを操作してみる

```
sleep 100
ctrl + Z       プロセスを停止
jobs           プロセス一覧を表示(コマンド実行したターミナルのみ)
sleep 200
ctrl + Z
jobs           停止プロセスが増えている
fg %1          1つ目のプロセスをフォアグラウンドに移行
ctrl + Z
bg %2          2つ目のプロセスをバックグラウンドで実行
jobs           2つ目のプロセスが実行中の表示になる
kill %1        1つ目のプロセスを終了

mkdir sanmple
vi sample.sh   中身に1秒ずつ数字を数えるコードを記述
chmod 755 sample.sh
./sample.sh &  バックグラウンドでプログラムを実行
jobs           ./sample.sh & が実行中プロセスとして表示される   出力ファイルを設定しておき、tale -F ファイル名 で実行内容を確認できる
ctrl + c
kill -19 %プロセス番号    ./sample.sh & のプロセスを停止       tale -F ファイル名で再度確認すると、値の更新が停止しているのを確認できる
bg %プロセス番号          プロセスをバックグラウンドで再開
```

<hr>

### Linuxのデスクトップ環境

##### X Window System

X.Org Foundationが開発している、LinuxをGUIで利用する為のクライアントサーバー型システム。  
サーバーをXサーバーといい、クライアントをXクライアントクライアントという。Xプロトコルを使い、入出力、Xサーバー、Xクライアントを仲介している。  
＊ X11 Xプロトコルのバージョン11

https://uhoho.hatenablog.jp/entry/2020/12/24/125056  
https://qiita.com/kakkie/items/c6ccce13ce0beaefaad1

```
/etc/X11/xrog.conf  Xサーバー用の設定ファイル。複数のセクションの集まりで構成され、各セクションはSection "<section-name>"行で始まり、EndSection行で終了する。

https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-ja-4/s1-x-server-configuration.html

/etc/X11/xrog.conf     Xのユーザー固有の設定ファイルを格納するディレクトリ
/etc/ssh/sshd_config   ssh接続を待ち受けるプロセスの設定ファイル

X11Forwarding yes      X11の転送許可
X11DisplayOffset 10    Xサーバのディスプレイ番号
X11UseLocalhost        X11にローカルサーバのみ許可
~/.xsession-errors     Xのエラーログファイル

startx                 CLIモードでXを起動
xhost                  Xサーバへの接続を許可
DISPLAY=<IPアドレス>:<ディスプレイ番号>.<スクリーン番号>     Xサーバのディスプレイ表示先を指定
xdpyinfo               ディスプレイ情報を表示
```

##### ディスプレイマネージャ

LinuxをGUI環境で利用可能にする

https://eng-entrance.com/linux-displaymanager  
https://log-bennkyou.com/561/

ディスプレイマネージャの変更方法

https://blue-red.ddo.jp/~ao/wiki/wiki.cgi?page=Linux%A4%CE%A5%ED%A5%B0%A5%A4%A5%F3%B2%E8%CC%CC%A4%F2%CA%D1%B9%B9%A4%B9%A4%EB

```
systemctl set-default graphical.target  ディスプレイマネージャの有効化(centOS)

・XDM
XDMCPプロトコルを使用するXのデフォルトのディスプレイマネージャ。
設定ファイル  /etc/X11/xdm/xdm-config
Xresources	ログイン画面のデザインを設定
Xaccess     ホストからXDMへのアクセス許可設定
Xsetup_0	ログイン画面表示前に実行されるスクリプト
Xsession	ログイン後に実行されるスクリプト

・KDM
KDE標準のディスプレイマネージャ。KDE環境とあわせて利用し、ウィンドウマネージャなども起動できる。
設定ファイルは /etc/X11/kdm/ にある

・GDM
GNOME標準のディスプレイマネージャ。CentOS7のデフォルト。
設定ファイルは /etc/X11/gdm/  にある

・Xfce
豪華な見た目と簡単な使用感を保ちながら、軽量・高速なデスクトップ環境を提供する。

・LightDM
Ubuntu標準のディスプレイマネージャ。デスクトップ環境に依存しない。Greeterで設定を行う。
設定ファイルは /etc/lightdm/ にある

VNC(Virtual Network Computing)    ネットワークを介して別のホストを遠隔操作するソフトウェア
RDP(Remote Desktop Protocol)      デスクトップコンピューターを遠隔操作する技術

クラウドコンピューティング            クラウドに保存されているファイルやアプリケーション、特にクラウドサーバーにアクセスする
リモートデスクトップ                 物理的なデスクトップコンピューターにアクセスするため、そのデスクトップに保存されたファイルとアプリケーションが利用できる
https://www.cloudflare.com/ja-jp/learning/access-management/what-is-the-remote-desktop-protocol/
```

<hr>

##### X11のインストール

以下2つをインストールして使うが、mac非対応
xming fonts、xming setup

代わりにXQuartzでやってみる

https://www.xquartz.org/ からdmgファイルをダウンロードしてインストール  
https://itcweb.cc.affrc.go.jp/affrit/documents/guide/x-window/x-win-mac  

```
su -
yum install xterm* xorg* xauth  (macで行う場合？は不要)

vi /etc/ssh/sshd_config        X11が使えるようにsshdの接続設定を変更

--------- sshd_config -----------
X11Forwarding yes           追記
X11DisplayOffset 10         追記
--------------------------------

sshd -t         テストモード。設定ファイルや鍵の正当性をチェック
https://nxmnpg.lemoda.net/ja/8/sshd

systemctl restart sshd.service      ssh接続の変更を反映

ip a                                ifconfigに変わって推奨されるコマンド。(ifconfigはyum install net-toolsでインストール)
https://qiita.com/_dakc_/items/4eefa443306860bdcfde

startxで xauth:  file /root/.serverauth.1717 does not exist のエラーを発見

yum remove xterm* xorg* xauth       下のコマンドで干渉するといけないので削除しておく

yum groupinstall "X Window system"   コマンドが古く対応していないと返ってきた
https://eng-entrance.com/linux-gui-x11
↓
yum -y group install "Server with GUI"   もっと多くのパッケージをインストール必要があった「yum install xterm* xorg* xauth」では足りなすぎた
*依存関係やパッケージ見つからないエラーが多数表示された為、指示通り依存関係の更新を行う --allowerasing オプションで実行した。

macのターミナルでxtermを起動
startx                       VirtualBoxで起動したcentOSの画面がGUIに変わっている


・以下は全てメモ
--------------------上手くいかなかったやり方--------------------
https://hana-shin.hatenablog.com/entry/2021/12/23/194837#3-%E3%82%B5%E3%83%BC%E3%83%90%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%A4%89%E6%9B%B4

vi /etc/ssh/sshd_config で追記   Port 22222
semanage port -a -t ssh_port_t -p tcp 22222
* semanageコマンドはSELinuxの管理で使用する。
* SELinux(Security-Enhanced Linux) Linux のセキュリティをより強固なものにするため、米国国家安全保障局(NSA)によって開発され、Fedora, Red Hat Enterprise Linux, CentOS では標準で有効化されている。
https://www.tohoho-web.com/ex/selinux.html

lsof -i4:22222 -a -P              sshdのポート番号を確認
firewall-cmd --permanent --add-port=22222/tcp   ポートの開放
firewall-cmd --reload             パーマネントルールをランタイムルールに展開。
firewall-cmd --list-ports         22222番ポートへのアクセス許可の確認
firewall-cmd --remove-port=22222/tcp  ポートを閉める

firewall-cmdについて  ポートの開閉など
https://qiita.com/hana_shin/items/bd9ba363ba06882e1fab
------------------------------------------------------------


--------- _(centOSでアンダーバーが打てなくなった場合) ------------
macを再起動したらcentOSのキーボード入力が変更されていた
https://detail.chiebukuro.yahoo.co.jp/qa/question_detail/q11247807971

sudo localectl status          キーボードの入力がJpになっているのが原因
sudo localectl set-keymap us   USキーにすればOK
------------------------------------------------------------


---------------- sshd_configの記述ミス -----------------------
Bad configuration option: permitrootlogin のエラー

下記の記述場所間違い     sshd_configだけでなくssh_configにも記述していた。間違えやすいので注意が必要。
X11Forwarding yes
X11DisplayOffset 10

https://teratail.com/questions/187130
------------------------------------------------------------

・その他コマンド
ping IPアドレス                      IPアドレス宛にパケットを送信し、受信できるか確認する
yum list                            インストール済みのパッケージを表示
yum list >> /home/ユーザ名/log/20220112_yum_list_before.log   パッケージインストール前にyum listを控えておくと依存関係でエラーが出た時に遡って調べることができる
nmcli d                             ネットワークインターフェースの確認
ls /etc/sysconfig/network-scripts/  confファイルの確認 => ifcfg-enp0s3
xwininfo                            xtermのディスプレイのウィンドウ位置やサイズの表示
xdpyinfo                            起動しているウィンドウすべての情報を表示
edho $DISPLAY                       xtermで行うと接続しているディスプレイ情報を表示  /private/tmp/com.apple.launchd.CAoVzywGh0/org.xquartz:0
```

##### パッケージグループのインストール

複数パッケージを一括インストールする場合はパッケージグループでのインストールが便利。  
＊OSのバージョンなどで dnf(新) yum(旧) か違ってくる

https://monologu.com/confirm-packagegroup/

```
yum group list                       グループ名一覧表示
yum group info グループ名              グループのパッケージ一覧表示
dnf group install グループ名           グループのパッケージを一括インストール
```

centOSをGUIモードでログインする

```
yum group install Workstation
systemctl set-default graphical.target     ランレベル５に変更(テキストログインのマルチユーザモード)
reboot
systemctl set-default multi-user.target    ランレベル３に変更(グラフィカルログインのマルチユーザモード)
```