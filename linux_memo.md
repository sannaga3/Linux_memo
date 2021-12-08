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
cat -n filename    # 行数付きでファイル閲覧
```

<hr>

### 外部ファイルを用いた四則演算

```
function calcurate_sum(){
	sum=0
        while read p;
        do
        	sum=$(( sum + p ))
       	done < $1
        echo SUM: $sum
 	exit 0
}

function calcurate_average(){
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


function calcurate_max(){
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

function calcurate_min(){
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
		calcurate_sum $fh
	elif [ $command = 'avg' ];
	then
		calcurate_average $fh
	elif [ $command = 'max' ];
	then
		calcurate_max $fh
	elif [ $command = 'min' ];
	then
		calcurate_min $fh
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

