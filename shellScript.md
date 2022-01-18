### shellとscript(102)

##### 環境変数の設定

環境変数を設定するファイル一覧

```
/etc/profile       ユーザーログイン時に読み込まれるディレクトリ。環境変数の設定を行うファイルが格納されている。

~/.bash_profile    ユーザーログイン時に呼び出される。PATHの設定などユーザー毎の内容を記述する。
~/.bash_login      .bash_profileがない場合のみ、ユーザーログイン時に呼び出される。
~/.profile          .bash_profileと.bash_loginのどちらもない場合のみ呼び出される。
~/.bashrc          非ログインシェル起動時に呼び出される。(su に - なしで起動、GUIでのターミナル起動など)
/etc/bashrc        全てのユーザー共通の設定
~/.bash_logout     ログアウト時に呼び出される

PATH               コマンドの実行ファイルを探す為のパス
---------------------------------------------
・PATHを読み込む場合の違い        https://lpi.or.jp/lpic_all/linux/intro/intro09.shtml
カーネル  ←  組み込みコマンド  ←  ユーザー
カーネル  ←  実行可能ファイルの格納されたディレクトリ ← 環境変数PATH  ← ユーザー
---------------------------------------------

alias              コマンドを別名で定義する

・環境設定呼び出し順まとめ
1  ~/.bash_profile
2  ~/.bash_login
3  ~/.profile

非ログインシェル  ~/.bashrc
```

・Linuxでのユーザー接続  
命令系統の流れ

```
ハードウェア ← OS ← (sshd) ← bash ← ユーザ

* bashのプロセスについて
  .シェルファイル もしくは source シェルファイル => 同一プロセスで実行
  ./シェルファイル名.sh                      => 新しくbashプロセスを起動して実行

. と ./ の違いを試す

vi sample.sh

---------------------------------------------
#!/bin/bash

echo $var
---------------------------------------------

chmod 755 ./sample.sh
var=abc
./sample.sh      =>    変数varの値を出力したいが、./で子プロセスを起動している為、varの値を表示できない。
. sample.sh      =>    現在のプロセス実行する為、値が表示される
```

・シェルの作成

```
mkdir command && cd command
vi hello.c   helloを出力するだけのプログラムを作成

--------------- hello.c ---------------------
#include<stdio.h>

int main(){
	print("Hello");
	return 0;
}
---------------------------------------------

gcc -o hello hello.c                        c言語からシェルスクリプトへコンパイル
pwd  =>  /home/nagasan/Lp102/01/command
PATH=$PATH:/home/nagasan/Lp102/01/command   ＄PATHにカレントディレクトリを追加する
echo $PATH                                  パスが追加されたか確認
hello                                       どこでもcommandディレクトリ内のhelloが使えるようになる
```

・エイリアスの登録

```
alias ll="ls -l"          エイリアスの登録
ll                        エイリアスを実行
```

・bachrcの設定

```
cd
vi ./.bashrc

------------- .bashrc -----------------------
以下を追記
export alpha=abc
function hello(){
        echo hello
}
---------------------------------------------

su ユーザ名                ログアウトせずにユーザーを切り替える(環境変数を引き継ぐ)
echo $alpha や hello とすると環境変数をそのまま扱える

＊ 全ユーザで共通の環境変数を設定したい場合は /etc/bashrc に記述する。ユーザ個別で設定する場合はhomeディレクトリの.bashrcに記述。
```

・bach_profileの設定

```
cd
vi ./.bash_profile

------------- .bash_profile -----------------
追記
echo 'called bash_profile'
---------------------------------------------

su - ユーザ名    =>   ログアウトしてユーザを切り替える為、ログイン時にechoの文が表示される
su ユーザ名      =>   ログアウトしない為、bash_profileは読み込まれない
```

・bach_loginの設定

```
cd
mv bash_profile bash_profile.ab   bash_profileが読み込まれないようにしておく
su - ユーザ名                       bash_profileが読み込まれてないのを確認
vi .bash_login

------------- .bash_login -------------------
以下を記述

echo 'called bash_login'
---------------------------------------------

su - ユーザ名                      bash_loginが読み込まれ、echo文が表示される。
```

・.profileも今までと同じ設定方法。bash_profileとbash_loginを無くすと読み込まれるのを確認できる。  

・/etc/profileの設定               全ユーザー共通の設定（root権限のみ変更可）

```
su -
vi /etc/profile
今までと同じようにPATHやエイリアスを記述すると、どのユーザでログインしても設定した環境変数が読み込まれる
```

