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

### linuxコマンド メモ
```
cat -n filename    行数付きでファイル閲覧
cat -b filename    行数付きでファイル閲覧(空白行は省く)
```

<hr>

### 文法 メモ
```
if [ $# -ne 1 ];   引数が１つでない場合。$#で引数の数を取得する。
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

