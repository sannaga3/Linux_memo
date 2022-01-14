<hr>

### ディストリビューション（配布形式）

配布形式...Slackware系、Redhat系、Debian系。カーネル(OS本体)とソフトウェアパッケージ群が一体となってLinuxOSとして配布される。

https://lpi.or.jp/lpic_all/linux/intro/intro02.shtml

<hr>

### Linuxコマンド
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

* manコマンド

```
man -a  全ての説明を表示
man -f  簡易説明を表示
man -k  キーワードを含む説明を表示
man -w  マニュアルのパスを表示
```

* unameコマンド(Linuxのシステム情報を確認)

```
uname -a  全ての情報を表示
uname -v  カーネルのバージョンを表示
uname -r  カーネルのリリース情報を表示
uname -p  CPU情報を表示
  32bit => x86、i386、i686、IA-32
  64bit => x64、x86_64、x86-64、AMD64、IA-64、Intel 64
uname -n  hostnameを表示
```

* historyコマンド

```
history 件数        件数分のコマンド履歴表示
history -c         コマンド履歴削除
history -d 行番号   行番号のコマンド履歴表示
history -a filename   bash起動後のコマンド履歴をファイルに追記
history -w filename   bash起動後のコマンド履歴をファイルに上書き
history -r filename   bash起動後のコマンド履歴を読み込んでからファイルに上書き
!!                 直近のコマンドを再度実行
!行番号             行番号のコマンド実行
！コマンド名         コマンド名で始まる直近のコマンドを実行  !ls => ls -a

```

* cutコマンド(ファイルから1部を抽出して出力)

```
cut -b
cut -c n文字目 hoge.txt    対象ファイルのn文字目を出力(各行のn文字目が対象になる)  -c = --characters
cut -c n-m hoge.txt       対象ファイルのnからm文字目を出力
cut -c 5- hoge.txt　　　　 対象ファイルのn文字目から最後まで出力
cut -d   区切り文字を指定して出力         -d = --delimiter
cut -d , -f 1 file1       各行を,で区切り、1なら左辺、2なら右辺を表示
cut -f  出力するフィールド番号を指定      -f = --fields
```

* expandコマンド(tabをspaceに変換)

```
expand filename   tab区切りのテキストファイル作成
expand            標準入力受け付け、タブ区切りの入力に対して各列のデータを揃える
expand -t n hoge.txt n文字ごとに各スペースの頭を揃える。デフォルトは8。
expand -i hoge.txt   行頭のタブ文字のみ空白に変換
```

* unexpandコマンド(tabをspaceに変換)

```
unexpand filename       tab区切りのテキストファイル作成
unexpand　-a filename   行頭以外のスペースもタブに変換
unexpand                標準入力受け付け、タブ区切りの入力に対して各列のデータを揃える
unexpand -t n hoge.txt  n文字ごとに各スペースの頭を揃える
```

* lessとmoreの違い

moreはファイルを全て読み込んでから表示するが、lessは必要な箇所を適宜読み込む。  
lessの方がメモリが少なく高速。

* fmtコマンド

```
fmt -w 1 filename   1行の文字数を1文字に指定(文字列の場合は末端までを１行として表示)
fmt -u filename     文字間のスペースが均等でない場合、スペース1つ分に揃えて表示
```

* prコマンド

```
pr /etc/passwd | less                          パスワードを印刷ページで表示
pr -h "password" /etc/passwd | less            ヘッダーを設定
pr -h "password" -l 15 +3:5 /etc/passwd | less      1ページの行数設定(ヘッダーをフッダー含める) 3~5ページ目を表示
```

* headコマンド

```
head -n test.txt   先頭からn行表示
head -c n test.txt   先頭からnバイト表示
```

* tailコマンド

```
tail -n test.txt     末尾からn行表示
tail -c n test.txt   末尾からnバイト表示
tail -F test.txt     テキストの変更を監視し、末尾への追加を即座に反映(ファイル名が変更されてもストップしない)
tail -f test.txt     テキストの変更を監視し、末尾への追加を即座に反映(ファイル名が変更されるとストップ)
```

* odコマンド(ファイルを8進数や16進数でダンプ（記録や表示）する)...バイナリファイルの中身を見る

```
od filename             8進数で表示
od -x filename          16進数で表示
od -t x1 filename       16進数かつ1byte単位で表示
od -t x1 -c filename    元の入力も合わせて表示
```

* sedコマンド(ファイルの編集を行う。文字列の全置換、行単位の抽出・削除など)

```
sed s/AAA/aaa/ filename        各行の最初のAAAをaaaに置換
sed s/AAA/aaa/g filename       ファイル内全てのAAAをaaaに置換
sed -i s/AAA/aaa/ filename     ファイル内全てのAAAをaaaに置換した後上書き保存
sed s/^[0-9]*,/・/ filename     ,を区切りとして主キーの数値を・に変換
sed 3d filename                ３行目を削除
sed 3,4d filename              ３〜４行目を削除
sed 's/ //g' filename          空白削除
sed -n 1p filename             1行目を表示
sed y/AB/ab/ filename          A => a, B => b にそれぞれ変換される。s/ は文字列を単位 y/ は文字を単位として変換する
```

* trコマンド(文字列の変換、消去)

```
cat file1.txt | tr 'a-z' 'A-z' > file2.txt          file1のアルファベットを全て大文字に変換してファイル2に保存
cat file1.txt | tr [:lower:] [:upper:] > file2.txt  同上
cat file1.txt | tr -d :                             ファイル１から : を削除して表示
tr -s 'a' < filename                                連続する文字を1つに縮小して表示
```

* sortコマンド(文字列の並び替え)

```
sort -f filename     大文字小文字を区別せずに並び替え
sort -n filename     文字列を数値として並び替え
sort -b filename     先頭の空白を無視して並び替え
sort -r filename     降順で並び替え
sort -R filename     ランダムに並び替え
sort -t , -k2 filename  ,で区切って右辺で比較して並び替え
```

* uniqコマンド(重複行の削除)

```
uniq filename                  重複行の削除
uniq -i filename               大文字小文字を区別せずに削除
sort -f filename | uniq -i     sortすることで重複行として認識させることができる
sort filename | uniq -d        重複行の表示
sort -f filename | uniq -i -c  各行の重複が何回起きたかカウント表示を追加
sort -f filename | uniq -i -c  各行の重複が何回起きたかカウント表示を追加
sort -f filename | uniq -u        重複していない行の表示

```

* joinコマンド(ファイルの連結)

```
join file1 file2       2つのファイルを連結して表示
join -a 1 file1 file2    ファイル2に対応する項目が無くとも、ファイル1を表示する
join -a 1 -o auto -e "---"  file1 file2    ファイル2に対応する項目が無い場合、---で表示する
```

* pasteコマンド(ファイルを行で連結)

```
paste file1 file2 file3         file1~file3の各行を連結    paste file[1-3]でも可
paste -d ,: file1 file2 file3   ,と；で区切り連結
```

* wcコマンド(ファイルのサイズを表示)

```
wc -c filename   文字数を表示
wc -l filename   行数を表示
wc -w filename   単語数を表示
```

* splitコマンド(ファイル分割)

```
split -100 filename bk/filename2.    ファイルを100行単位で、filename.aa filename.ab という形式に分割
less bk/filename.a*                  分割したファイルを合わせて表示
split -b 20 filename bk2/filename2   20バイトごとにファイルを分割
```

* grepコマンド(文字列と正規表現を用いた検索)

```
grep -v abc             　　abcを含まない行を検索
grep -i abc filename       大文字小文字を区別せずに検索
grep -n abc filename       マッチした行数も表示

grep [a-z] filename        アルファベットを含む行を検索
grep -F [abc] filename     正規表現を無視して検索。 -F がないと [abcd] だけでなく abcd もマッチしてしまう
fgrep [abc] filename       同上

grep -E 'is(.*?)file' filename   grepでは正規表現に制限がある為、 -Eで機能を拡張する
egrep 'is(.*?)file' filename     同上
```


<hr>

##### ビット数とメモリサイズ

8bit  = 2の8乗  => 256Byte  
16bit = 2の16乗 => 64KB  
32bit = 2の32乗 => 4GB  
64bit = 2の64乗 => 128TB  

x86-64 => 64bitの32bit互換対応  
https://blog.framinal.life/entry/2020/04/22/041548　

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

オプション -R 配下のすべてのディレクトリ・ファイルにパーミッション変更を適用
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

### ファイル検索

```
find -name "filename"       文字列での検索（大文字小文字の区別あり）   *を使う場合は必ずダブルクォートが必要。付けないとbashが展開と勘違いしてエラーになる。find ./ -name "*.txt"

find -iname "filename"      文字列での検索（大文字小文字の区別なし）
find -not -name "filename"  文字列を含まない検索

find . -atime -day          引数の日付までにアクセスされたファイルを検索(catコマンドなどもアクセスに含まれる)
find . -mtime -day          引数の日付までに修正されたファイルを検索
find . -amin -min           (引数)分前までに修正されたファイルを検索
find ./ -type f             ファイルのみ検索。　オプション　d => ディレクトリ　s => シンボリックリンク
find ./ -size 0             サイズで検索
find ./ -user test          所有者で検索
find ./ -mtime +1 -type f -exec rm {} \;    オプションの組み合わせで該当ファイルを削除

which find                  コマンドがどこにあるか検索
type cat                    シェルが何のコマンドを実行しているか表示(ハッシュ(プログラムファイルの実行)、組み込み関数(内部コマンド)、エイリアス、予約語)
whereis find                コマンド、設定ファイル、マニュアルのパスを検索
```

* locateコマンド ... /etc/updatedb.conf で対象ディレクトリや検索条件を変更する必要がある為、あまり使われない

```
yum install mlocate         システム内のファイルを素早く検索してくれるlocateコマンドをインストール
updatedb                    dbの更新
locate *txt                 対象ディレクトリで高速でファイル検索
```

<hr>

### シェルとコマンドライン

##### シェル(CLI...Command-Line Interface、Command-Line Interpreter)

https://pikawaka.com/word/shell

- コマンドラインを用いてユーザーとカーネル(OSの中核部分)の仲介を行うプログラム。ユーザーはシェルを介してPCをへの入力や出力の受け取りを行っている(Command-Line Interface)
- ユーザーのコマンド入力を機械語に翻訳してカーネルに伝える役割も持っている(Command-Line Interpreter)

##### 環境変数・シェル変数

環境変数 ... 子シェルまたはプロセスにも引き継がれる変数  
シェル変数 ... 現在実行中のシェルだけで有効な変数

##### ユーザーログインの流れ(下記3つの流れでユーザーのログインが行われ、シェルが起動する)   https://zenn.dev/yururi/articles/a7b2d72f418fba4e16b7
.bash_profile ファイルには特定のユーザーの環境変数とシェル変数が保存されている
.bashrc ファイルは bashrc ファイルを実行する。また、特定ユーザーのエイリアスや関数が保存されている
/etc/bashrc ファイルにはシステム全体のエイリアスや関数が保存されている

##### カーネル

https://www.idcf.jp/words/kernel.html

OSの中核となるプログラムであり、基本機能の役割を担うソフトウェアを指す。
アプリケーションの実行やプロセス管理、メモリへのアクセス管理やコンピュータに接続された周辺機器のデバイス管理を行う。
Linux系OSは、OSの中核である「カーネル」とOSを構成する「他のプログラム」から構成されている。

###### bash

Linuxでサポートされるシェルのひとつ。元々はキーボードからコマンドを入力・実行するための対話プログラムだったが、変数、関数、制御構文、配列なども可能なので、簡易的なプログラミング言語といえる。

###### sh

古くから利用されているシェル

##### csh

C言語を用いたシェル

<hr>

### 環境変数コマンド

```
printnv                   環境変数一覧表示
env 環境変数名=値 コマンド   環境変数の値を設定し、かつコマンドを実行　　env user=Bob age=10 ./ファイル名(シェル変数として定義する為、環境変数は変更なし)
env LANG=C date           環境変数LANGに値を設定してからdateコマンドを実行
set                       環境変数とシェル変数の一覧を表示
unset 変数名               環境変数、シェル変数を削除
export 変数名              シェル変数おw環境変数として設定(別のシェルから呼び出し可能にする)
```

* 環境変数一覧

```
HOME ...ホームディレクトリ  
LANG ...使用言語  
PATH ...実行コマンドの検索パス  
PWD  ...カレントディレクトリ  
USER ...ログインユーザ名  
LOGNAME　...ログインシェルのユーザ名 su => USER変数が変更されない(ユーザを切り替えるだけ)　su - => USER変数が変更される(ログインし直す)  
PS1  ...プロンプトの定義。デフォルトは export PS1="[\u@\h \W]\\$ "  [ユーザー名@ホスト名　カレントディレクトリ]  
https://atmarkit.itmedia.co.jp/flinux/rensai/linuxtips/002cngprmpt.html  

HISTSIZE       コマンド履歴の最大値     env | grep HISTSIZE => export HISTSIZE=20  コマンド履歴を確認後、履歴の表示を20件に変更  
HISTFILE       コマンド履歴を格納するファイルの場所  less /home/ユーザ名/.bash_history でコマンド履歴確認  
HISTFILESIZE   HISTFILEに保存する履歴の数  
```

##### 変数の操作

```
変数名=値                  代入
print_msg="msg is $msg"  文字列に変数を組み込む場合はダブルクォーテーションで囲む
echo $変数名              値の取得
```

<hr>

##### FHS（Filesystem Hierarchy Standard）

UNIX系OSの標準的なディレクトリ構成を定めたもの。
ルートディレクトリ直下のディレクトリで必須とされているものは 「/bin」「/boot」「/dev」「/etc」「/lib」「/media」「/mnt」「/opt」「/sbin」「/srv」「/tmp」「/usr」「/var」。


<hr>

##### GNU（GNU is Not Unix）   https://atmarkit.itmedia.co.jp/aig/03linux/gnu.html

UNIXと上位互換性のあるフリーな総合ソフトウェアシステムの名称。システムにはOSカーネル、シェル、エディタ、プログラム言語処理系など、ハードウェア以外のすべてのソフトウェアが含まれる。カーネル以外はほぼ完成しており、カーネルとしてLinuxの開発が進められた。狭義の「Linux」ではカーネル部分のみを指す。

<hr>

##### gcc

gcc ... GNUプロジェクトが開発および配布している、さまざまなプログラミング言語のコンパイラ集。

```
yum install gcc ...CentOSにgccを導入
gcc hello.c ... hello.cというC言語のファイルをコンパイルする
./a.out ...ファイルの実行
```

<hr>

### ストリーム

コンピューターの「入力 → コマンド → 出力」 の流れをストリームという。  
Linuxにも 「標準入力 → コマンド(プログラム) → 標準出力 or 標準エラー出力」 がある。  
https://www.kenschool.jp/blog/?p=1143

```
Linuxではストリームを番号で識別している

標準入力 => 0          デフォルト設定  キーボード
標準出力 => 1                        末端端末
標準エラー出力 => 2	                  末端端末
```

<hr>

### リダイレクトとパイプ

入出力はコマンド実行時に切り替えられる。リダイレクトとパイプにより
コマンド実行後の流れを切り替えることができる。
https://www.infraeye.com/study/linuxz15.html

```
リダイレクト(コマンドの入出力を別ファイルに切り替える)

a > b          標準出力の出力先変更(上書き)
a >> b         追記(標準出力)
a 2> b         上書き(エラー)
a 2>> b        追記(エラー)
a >> b 2>&1    追記(標準出力とエラー)
a &> b         標準出力とエラーを同じファイルに上書き
a < b          標準入力aにコマンドbを渡す    grep -v abc < filename  filenameからabc以外の行を取得
grep abc << EOF ~~~~ EOF                                        標準入力の中から該当行を探す
./standard_input.txt > output.txt 2> standard_input_errors.txt  標準出力とエラー出力を分けて出力
```

```
パイプ(コマンドの出力結果を別のコマンドの標準入力に渡す)
1つ目のコマンドの標準出力を2つ目のコマンドへ渡す。  標準入力 →  コマンド  →  コマンド  →  標準出力 or 標準エラー出力

cat filename | grep "abc"      コマンド(cat filename) => コマンド(grep "abc") => 標準出力
```

##### teeコマンド

リダイレクト先への書き込みと標準出力を同時に行う

```
 ls -lt | tee data.txt      上書き
 ls -lt | tee -a data.txt   追記
 ls -lt | tee -a data.txt | less   追記した後lessで閲覧
```

##### xargsコマンド

標準入力の内容をパラメータ(引数)としてコマンドに渡す
https://hydrocul.github.io/wiki/commands/xargs.html

```
find . -name "*.txt" | xargs rm                テキストファイルを全て削除
mkdir bk && find . -type f | xargs mv -t bk    作成したバックアップディレクトリにファイルを全て移動  オプション -t => 引数をディレクトリに指定
```

##### 疑似デバイスファイル /dev/null

書き込んだデータを全て破棄し、データの読み込みに対して何も返さない。  
ブラックホールと呼ばれるゴミ捨て用ファイル。  
リダイレクト先に指定することで不要な出力を抑えたり、空データとして使うこともできる  

https://webbibouroku.com/Blog/Article/dev-null


<hr>

### エディタ

```
vi     Linuxの標準エディタ
vim    vi の改良版    Ubuntuでの vi コマンドは vim が起動する
       C/C++、Python、Perl、Shellなどの構文サポートが含まれる
       圧縮後ファイルの変更が可能(gzipやzip)

vimdiff filename1 filename2   2つのファイルを比較しながら編集可能。ctrl + w で交互のエディタを行き来する

nano    Ubuntuの標準エディタ。vimより簡単。

・デフォルトエディタの変更
which editorname でパス検索
export EDITOR=上記のエディタパス     環境変数のデフォルトエディタを変更
crontab -e で現在のデフォルトエディタが起動する
```

<hr>

### パッケージ管理

##### rpmとyum

どちらもRedHat系のパッケージ管理方式。rpmはパッケージ個々を管理し、yumはrpmを管理する。rpmは自動で依存関係を考慮してインストールしないが、yumは依存関係を解決してインストールできる
https://eng-entrance.com/linux-package-rpm-yum-def

##### rpm

RedHat社が開発したパッケージ管理方式。rpmコマンドでパッケージを管理する

```
rpm -i      パッケージのインストール(依存関係があるとエラーになる)   --install
rpm -U      パッケージのアップグレード  --upgrade
rpm -F      パッケージのアップグレード  --freshen
上記と併用するオプション
-h          インストール進み具合の表示  --hash
➖v         詳細情報表示
--nodeps    依存関係を無視してインストール
--prefix    インストールするディレクトリを指定
--relocate  インストール済みのパッケージのディレクトリ変更(再インストール)
-e          パッケージを削除          --erase

rpm -q      インストール済みパッケージの検索  --query
rpm -V      パッケージの検査
上記と併用するオプション
-a          全パッケージを表示                   --all
-l          パッケージに含まれるファイル一覧を表示  --list
-i          パッケージの詳細表示                 --info
-f          ファイル名を指定し、どのパッケージにインストールされているかを表示    --packeage
-p          パッケージ名を元に、含まれる全てのファイルを表示
--changelog パッケージの更新履歴を表示
-R          依存ファイルの表示

rpm --nomd5 md5sumチェックによるファイル改竄の検査をしない
＊md5sumコマンド   ファイルをダウンロード（コピー）した後に、破損や改変がないことを確認する。ファイルのダウンロード元やメッセージの送信元がハッシュ値を公開している必要がある。
https://atmarkit.itmedia.co.jp/ait/articles/1711/17/news013.html

rpm2cpio パッケージ名 | cpio --list   パッケージ内のファイルリスト表示
rpm2cpio パッケージ名 | cpio -ivd ファイルパス   指定したファイルの取得
http://wordpress.honobono-life.info/lin-base/%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%9B%E3%81%9A%E3%81%ABrpm%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E5%86%85%E3%81%8B%E3%82%89%E7%89%B9%E5%AE%9A%E3%81%AE%E3%83%95/
```

##### yum

RedHat系で使われるrpmパッケージ管理ツール。  
yumのパッケージはそれぞれがレポジトリとしてインターネット上に保存されている。  
https://eng-entrance.com/linux-package-yum

```
yum check-update           アップデート可能なパッケージを表示
yum update                 すべてのパッケージをアップデート
yum update パッケージ名      パッケージのアップデート
yum install パッケージ名     パッケージのインストール
yum remove パッケージ名      パッケージのアンインストール
yum info パッケージ名        パッケージ情報の表示
yum list                   パッケージ一覧の表示
yum search パッケージ名      パッケージを検索
yum grouplist              パッケージグループ一覧を表示
yum groupinstall           パッケージグループのインストール
yum provides ファイル名      ファイルの含まれるパッケージを検索

/etc/yum.conf              yumの設定ファイル
/etc/yum.repos.d/          yumコマンドで使うレポジトリ情報が書かれている。このディレクトリの情報を元にパッケージがインストールされる(パッケージが保存されているレポジトリの情報が書かれている)
yumdownloader              rpmパッケージをインストールせずにダウンロードのみ行う
yumdownloader --source     rpmパッケージではなくソースファイルをダウンロードする
```

・yumコマンドの情報

```
ls /etc/yum.repos.d/                              yumコマンドで使う情報が格納されたディレクトリ
ls /etc/yum.repos.d/CentOS-Linux-AppStream.repo   各yumコマンドのインストール先情報が記述されているファイル
cat /etc/yum.repos.d/CentOS-Linux-AppStream.repo  下記のURL情報でバージョンとアーキテクチャの部分を書き換える
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=AppStream&infra=$infra
↓
http://mirrorlist.centos.org/?release=8&arch=x86_64&repo=AppStream&infra=    ブラウザにコピペするとyumコマンドのソース元にアクセスできる
```

##### zsh

・zshの利点
メニュー補完、コンテキスト補完、多段階層補完など
https://www.mimir.yokohama/serials/linux-one/0007-zsh.html

```
rpm -ivh http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/zsh-5.5.1-6.el8_1.2.x86_64.rpm    CentOS8用のzshシェルのパッケージを詳細と進捗を表示しながらインストール

cat /etc/shells           shell一覧にzshが追加されているのを確認
zsh --version             バージョン確認

rpm -V zsh                パッケージに問題がないか検査
echo $?                   前のコマンドの実行結果を表示。0 => true  1 => false  127 => コマンドが見つからない
https://qiita.com/satton6987/items/6031785e23d8fdbb1a5a

rpm -qf /usr/bin/zsh       指定したファイルを含むパッケージを表示  => zsh-5.5.1-6.el8_1.2.x86_64
rpm -qa                    インストール済みファイルすべて表示
rpm -ql openssh            openssh含まれるファイル一覧表示
rpm -qi openssh            パッケージの詳細情報表示
rpm -e zsh                 パッケージの削除
```

・httpd

Apacheをインストールして使えるようにする  
https://qiita.com/yasushi-jp/items/c5feeaff2024d3c069db  
https://densan-hoshigumi.com/server/apache/install-rhel8-centos8  

```
rpm -ivh http://repo.okay.com.mx/centos/8/x86_64/release/httpd-2.4.37-12.el8.x86_64.rpm
↓  依存関係の警告
yum install httpd
```

##### dpkg

debian系のパッケージ管理コマンド。依存関係を解決しない為、使うことは少ない。

```
dpkg -i     インストール                         --install
dpkg -r     設定ファイルを残してパッケージを削除     --remove
dpkg -p     設定ファイルも含めてパッケージ削除       --purge
dpkg -l     一覧表示                            --list
dpkg -i     アーカイブ表示                       --info
dpkg -c     パッケージに含まれるファイル一覧表示     --contents
dpkg -C     パッケージのインストール状態表示        --audit
dpkg -s     詳細表示                            --status
dpkg -S     文字列に一致するパッケージ表示          --search
```

##### apt

dpkgを拡張したパッケージ管理コマンド。
依存関係を解決し、パッケージ管理サイト(リポジトリ)から最新のパッケージをインストールする。

「apt-get」ではなく「apt」コマンドが推奨されている
https://linuxfan.info/package-management-ubuntu  
https://qiita.com/karaage0703/items/f01db1cf49b151022b7c

/etc/apt/source.list  パッケージのダウンロード元の設定ファイル

```
apt install パッケージ名      パッケージのインストール
apt install -d 設定ファイル   パッケージのダウンロードのみ
apt update                  リポジトリからパッケージの名前・バージョン・依存関係を取得し、アップデート可能なパッケージリストを更新して表示
apt upgrade                 アップグレード可能なパッケージを更新する。依存関係などで削除が必要なものは更新されない
apt full-upgrade            他のパッケージで削除が必要なものを含めてパッケージを更新する
sudo apt remove パッケージ名  パッケージの削除
sudo apt autoremove         依存関係で不要になったパッケージを全て削除
apt purge パッケージ名        パッケージ削除時に残る場合のある設定ファイルも含めて削除
apt search 文字列 | less     単語を含むパッケージを検索する
apt-cache search ruby | grep ^ruby | sort | less   1行につき１パッケージで検索
apt clean                   ダウンロードされたdebパッケージ(/var/cache/apt以下にキャッシュ)を削除
apt list                    インストールしていないものも含むパッケージ一覧
apt list --installed        インストール済みのパッケージ一覧
dpkg -L パッケージ名          パッケージ内のファイル一覧
apt show パッケージ名         パッケージ詳細
apt depends パッケージ名      依存、提案、推奨、競合関係のパッケージを表示
dlocate ファイル名            ファイルからパッケージ名の検索
apt changelog パッケージ名     パッケージの更新履歴を表示
apt-file update パッケージ名   パッケージ情報の更新
apt-file search ファイル名     ファイル名を含むパッケージを検索   apt install apt-fileが必要
```

### ubuntuでaptコマンドを使ってみる

```
docker run -it ubuntu
cat /etc/os-release         コンテナのOS情報確認
apt update                  パッケージリストの更新で下記エラー発生
E: Release file for http://security.ubuntu.com/ubuntu/dists/focal-security/InRelease is not valid yet (invalid for another 1d 5h 54min 44s). Updates for this repository will not be applied.
↓
centOS側の時刻がズレていた
timedatectl set-timezone Asia/Tokyo
timedatectl set-time "2022-01-14 21:39:00"

再度 apt update
apt install less
less /etc/apt/sources.list   パッケージのインストール先記述ファイル
apt show less                lessのパッケージ情報表示
apt install apt-file
apt-file update              先にリスト更新が必要
apt-file apt-file search etc/security   対象ディレクトリのファイルを検索
```
