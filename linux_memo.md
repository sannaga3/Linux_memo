<hr>

### ディストリビューション（配布形式）

配布形式...Slackware系、Redhat系、Debian系。カーネル(OS本体)とソフトウェアパッケージ群が一体となってLinuxOSとして配布される。

https://lpi.or.jp/lpic_all/linux/intro/intro02.shtml

<hr>

### linuxコマンド メモ
```
cat -n filename    行数付きでファイル閲覧
cat -b filename    行数付きでファイル閲覧(空白行は省く)
touch -t 202001010101 file4　　　作成日時を指定してファイル作成
ls -lt   ファイルを作成日時順で表示（-ltrで新しいものが一番上に来る）
```

* ファイル圧縮コマンド
```
tar filename   ファイル圧縮・解凍（複数可）。yum install tar で導入
オプション　-c 圧縮(.tar) -f ファイル名指定　-x 解凍 -z 圧縮・解凍(gzip) -j 圧縮・解凍(bzip2) -J 圧縮・解凍(xz)
tar -cf(圧縮) -xf(解凍)をセットで使う  tar -cf file.tar file1 file2(file1とfile2をfile.tarとして圧縮)
tar -czf filename.gz filename1 filename2(圧縮)
tar -cxf filename.gz filename1 filename2(解凍)
tar -t 圧縮されたファイル ...圧縮されたファイルの中身を全て表示

gzip bzip2 xz はいずれも下記の方法で圧縮・解凍を行う
gzip filename(圧縮)
gzip -r dirname(ディレクトリの中身を丸ごと圧縮)
gzip -d filename(解凍) もしくは gunzip filename
```
<hr>

### 文法 メモ
```
if [ $# -ne 1 ];   引数が１つでない場合。$#で引数の数を取得する。
```

<hr>

### シグナル番号

* シグナル番号・・・プロセスとプロセスの間で通信を行う際に使用される番号であり、シグナルを受け取ったプロセスは番号に対応する処理を行う。
  https://atmarkit.itmedia.co.jp/ait/articles/1708/04/news015.html
* シグナル番号一覧
  https://www.xmisao.com/2013/11/10/linux-kill-signals.html
<br>

* kill -l でシグナル一覧が見れる

<hr>

### kill -9 と -15 の違い

* シグナル番号15もしくはオプション省略でプロセスの正常終了
```
kill -15 PID
kill PID
```
* サーバーエラーなどの異常時はシグナル番号9かkillコマンドで強制終了
```
kill -9 PID
kill -KILL PID
```

* killしたいプロセスを探す。 puma unicorn など
```
ps ax | grep 〇〇
```

<hr>

### trapコマンド
実行中のプロセスから放出されるシグナルを検知し、指定された処理を返す。
http://peace3110.oops.jp/unix-linux-%E3%82%B7%E3%82%B0%E3%83%8A%E3%83%AB%E3%81%A8%E3%83%88%E3%83%A9%E3%83%83%E3%83%97/

```
if [ $# -ne 1 ];  # $#でファイル実行時の引数の数を取得する。-ne not equal
then
        echo 'argument is wrong'
        exit 1
fi

# プロセスが シグナル2(強制終了)かシグナル15（正常終了）した場合、ロックファイルを削除する
function delete_lock_file() {
        rm sample.lock
        exit 0
}

if [ $1 = 'start' ];
then
        if [ -f 'exam7.lock' ];
        then
                echo 'process is already running'
                exit 0
        else
                echo $$ > sample.lock              # lockファイルにPIDを書き込む。lockファイルは実行中なので触るべからずを意味し、処理終了時に消去するのが一般的。
                trap "delete_lock_file" 2 15       # 割り込みまたは正常終了した場合stop_process関数を実行する
                for i in `seq 1 1000`;             # メインの実行処理。1から1000までファイルに書き込む。
                do
                        echo $i >> output_$$.txt
                        sleep 1
                done
                rm sample.lock
                exit 0
        fi
fi
```

<hr>

### 数値の比較演算子

```
-eq  => equal 同じ
-ne  => not equal 同じでない
-ge  => great equal 以上
-gt  => great than  より大きい
-le  => less equal  以下
-lt  => less than   より小さい
```

<hr>

### 外部ファイルを用いた四則演算

```
function calculate_sum(){
	sum=0
        while read p;
        do
        	sum=$(( sum + p ))
       	done < $1
        echo SUM: $sum
 	exit 0
}

function calculate_average(){
	sum=0
        count=0
        while read p;
        do
        	sum=$(( sum + p ))
                count=$(( count + 1 ))
        done < $1
        echo AVG: $(( sum / count ))
        exit 0
}


function calculate_max(){
	max=0
	while read p;
	do
		if [ $max -lt $p ];
		then
			max=$p
		fi
	done < $1
	echo MAX: $max
	exit 0
}

function calculate_min(){
	min=101
	while read p;
	do
		if [ $min -gt $p ];
		then
			min=$p
		fi
	done < $1
	echo MIN: $min
	exit 0
}

read -p 'please input file_name: ' fh

if [ -f $fh ];
then
	read -p 'choose from ( sum, avg, min, max, exit ) ' command
	if [ $command = 'sum' ];
	then
		calculate_sum $fh
	elif [ $command = 'avg' ];
	then
		calculate_average $fh
	elif [ $command = 'max' ];
	then
		calculate_max $fh
	elif [ $command = 'min' ];
	then
		calculate_min $fh
	elif [ $command = 'exit' ];
	then
		echo 'exit'
		exit 0
	else
		echo 'that command dose not exist'
		exit 1
	fi
else
	echo 'that file dose not exist'
	exit 1
fi
```

<hr>

### ファイルの操作

```
-d	引数がディレクトリであれば真
-e	引数が存在すれば真
-f	引数がファイルであれば真
-h	引数シンボリックリンクであれば真
-s	引数が存在し、空ファイルでなければ真
-r	引数がREAD許可されていれば真
-w	引数がWRITE許可されていれば真
-x	引数が実行許可されていれば真
-ef	引数がハードリンクであれば真
file1 -ot file2	 file1がfile2より古い場合真  -ot => older than
file1 -nt file2	 file1がfile2より新しい場合真  -nt => newer than
```

```
select command in 'list' 'delete' 'rename' 'copy' 'show' 'exit'
do
	case $command in
	'list' )
		ls;;
	'delete')
		ls
		read -p 'Enter file name want to delete: ' file_name
		if [ -f $file_name ];
		then
			rm $file_name
		fi;;
	'rename')
		ls
		read -p 'Enter file name you want to rename: ' file_name
		read -p 'Enter new file name: ' new_file_name
		if [ -f $file_name ];
		then
			mv $file_name $new_file_name
		fi;;
	'copy')
		ls
		read -p 'Enter file name you want to copy: ' file_name
		read -p 'Enter file name after coping: ' new_file_name
		if [ -f $file_name ];
		then
			cp $file_name $new_file_name
		fi;;
	'show')
		ls
		read -p 'Enter file name you want to look: ' file_name
		cat $file_name;;
	'exit')
		break;;
	esac
done
```

<hr>

### ShellScriptのデバッグ

```
#!/bin/bash -x     # ファイル自体がデバッグモードになる

set -x             # set-x と set+x の間がデバッグモードになる
echo Hello
set +x

bash -x base19.sh  # ファイルをデバッグモードで実行する
```

<hr>

### パーミッション

* chown...所有者を変更する
* chgrp...所有グループを変更する。使い方はchownと同じ

```
chown 所有者名 ファイル名またはディレクトリ名

chown -R 所有者名 ディレクトリ名...ディレクトリ配下のファイルも含めて全て所有者を変更　
```

* chmod...アクセス権(rwx)を変更する。
lsコマンド実行時の表示順 => 所有ユーザ　所有グループ　その他ユーザ  
r => 4  
w => 2  
x => 1

```
chmod 755 ファイル名...対象ファイルに対し　所有ユーザ(7)　所有グループ(5) その他ユーザ(5)の権限を与える
＊ 7 => rwx  5 => rx

chmod 744 ファイル名..所有ユーザのみ全権限、その他は読み取りのみ

chmod 000 ファイル名...全ての権限削除
```

* SUID...SUIDが設定されたファイルの実行は所有ユーザの権限で行われる(その他ユーザで権限がなくとも、SUID設定されていれば実行可能)

下記でUSIDの設定が可能...USIDが設定されるとlsコマンドでの実行権限にsが追加される rwxr => rwsr
```
chmod u + s ファイル名（所有ユーザーに設定）
chmod 4xxx ファイル名...xxxはパーミッション

chmod g + s ファイル名（所有グループに設定）
chmod 2xxx ファイル名

chmod o + s ファイル名（その他ユーザに設定）
chmod 1xxx ファイル名

chmod o + t ファイル名 ... stickyビット。所有者かrootユーザ以外は対象ファイルを削除できなくなる
chmod 1xxx ファイル名
find / -perm -1000 ...stickyビットが付与されたファイル・ディレクトリのみを検索
```

* umask...ファイル作成時のデフォルトのアクセス権を設定
https://atmarkit.itmedia.co.jp/ait/articles/1907/26/news011.html

```
umask 002   =>  CentOSのデフォルト設定は666。666 - 002 => 664 がデフォルトの設定として変更される
```

* groupに関するコマンド

```
groupadd グループ名 ...パーミッション用グループ作成

groups　...所属グループの表示

usermod ...rootユーザの権限でHomeディレクトリ やグループ、パスワード情報を変更する

chgrp グループ名 ファイル名 => 作成したグループをパーミッションに適用
```

<hr>

### Linuxのファイルシステムとiノード

* iノード...ファイルやディレクトリを管理するための一意の番号。ファイルの中身とは別の作成者・作成日時・サイズ・実データの位置などを含む管理情報。inodeには上限があり、データ領域が残っていてもinodeが上限に達するとデータを保存できなくなる。

```
df -i  inodeの使用状況を確認
ls -i  ファイルやディレクトリのinodeを確認
```

<hr>

### ハードリンクとシンボリックリンク

ファイルの名前(外見)、inode、実体が個別に管理されている。

##### <ハードリンク>

* リンク元が削除されてもアクセスできる(ファイルのinodeと実体は削除されていない)
* バックアップや複数ディレクトリに同じデータを配置する場合に使う(データ変更は全てのファイルに反映される)
* リンク元を別ディレクトリに移動するとinodeが上書きされる為、ハードリンクの変更が反映されなくなる

```
file1(UI) => file1 inode => file1 実体
　　　　　          ⬆︎　アクセス
          file1のハードリンク
```

* ハードリンクの作成

```
ln      リンク元ファイル名 ハードリンクのファイル名
ls -li  ノード番号の確認
```

<br>

##### <シンボリックリンク>

* ディレクトリに対してもシンボリックリンクを作成できる
* 元ファイルが削除または名前が変更されるとアクセスできなくなる(元ファイルのinodeではなくファイルの外見にアクセスしている為)
* 元ファイルを消してもシンボリックリンクは残るので注意

```
file1(名前) => file1 inode => file1 実体
　　 ⬆︎ アクセス
file1(シンボリックinode) <= file1のシンボリックリンク
```

* シンボリックリンクの作成

```
ln -s リンク元ファイル名 シンボリックリンクのファイル名

ln -s リンク元ディレクトリ名 シンボリックリンクのディレクトリ名
cd シンボリックリンク => リンク先のディレクトリへ移動
```

<br>

*  無効なクロスデバイスリンクです  =>  異なるパーティション間ではハードリンクを作成できない
https://qiita.com/lnznt/items/6178e1c5f066f22fe9c2

<br>

* パーティション確認
```
fdisk -l
```

<hr>

### その他

gcc ... GNUプロジェクトが開発および配布している、さまざまなプログラミング言語のコンパイラ集。
```
yum install gcc ...CentOSにgccを導入
gcc hello.c ... hello.cというC言語のファイルをコンパイルする
./a.out ...ファイルの実行
```