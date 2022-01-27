### システム管理

#### アカウント管理

```
cat /etc/passwd         ユーザの設定情報が記述       https://www.server-memo.net/centos-settings/system/passwd_shadow.html

----------------- /etc/passwd -----------------------------
root:x:0:0:root:/root:/bin/bash         1行目
nagasan:x:1000:1001:nagasan:/home/nagasan:/bin/bash

：を区切り文字として、以下7つのフィールドでユーザ情報が表示される
root        ユーザ名
x:          /etc/shadowでパスワードを管理している(rootユーザのみ操作可能)。 *の場合はパスワードが設定されていない。
0:          ユーザID
0:          グループID
root:       コメント(ユルネームや役割を記述)
root:       ホームディレクトリ
/bin/bash:  ログインシェル
-----------------------------------------------------------

cat /etc/group           グループの設定情報が記述

----------------- /etc/group- -----------------------------
root:x:0:

root:       グループ名
x:          パスワード
0:          グループID
            ユーザーリスト

* ユーザは複数のグループに所属でき、以下の区別がある
プライマリグループ  =>   一番に所属するグループ
セカンダリグループ  =>   プライマリ以外のグループ
-----------------------------------------------------------

ls -a /etc/skel          ユーザディレクトリの雛形。ユーザーを作成すると、このディレクトリと同じ構成で作成される。ユーザー共通のファイルを設定する。

-----------------------------------------------------------
[root@localhost ~]# ls -a /etc/skel
.  ..  .bash_logout  .bash_profile  .bashrc  .mozilla
-----------------------------------------------------------
```

##### id コマンド

ユーザのUID、GID、セキュリティコンテキスト（context）を表示する。  

```
id          ログインユーザの情報表示

-----------------------------------------------------------
[root@localhost ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
-----------------------------------------------------------

id ユーザ名  指定ユーザの情報表示

-----------------------------------------------------------
[root@localhost ~]# id nagasan
uid=1000(nagasan) gid=1001(my_group) groups=1001(my_group)
-----------------------------------------------------------
```

##### セキュリティコンテキスト

SELinuxがファイルやプロセス、ユーザーに付けるラベルのこと。ユーザー識別子、ロール識別子、タイプ、レベルに分けられる。  
https://www.solima.net/centos7/archives/166

##### SELinux

Security-Enhanced Linux。カーネルの制御機能のひとつであり、システムにアクセス可能なユーザーをより詳細に制御できるようになったもの。  
セキュリティポリシーを使用してシステム上のアプリケーション、プロセス、ファイルのアクセス制御を定義する  

https://www.redhat.com/ja/topics/linux/what-is-selinux  
https://eng-entrance.com/linux-selinux

##### useradd userdel usermodコマンド

ユーザーの作成・削除・変更コマンド。  
https://atmarkit.itmedia.co.jp/ait/articles/1612/14/news022.html

```
useradd -d         ホームディレクトリ指定
useradd -g         プライマリグループの指定
useradd -G         セカンダリグループの指定
useradd -s         ログインシェルの指定

usermod -d         ホームディレクトリ変更
usermod -e         アカウントが使用不能になる日付を指定
usermod -g         プライマリグループの変更
usermod -G         セカンダリグループの変更
usermod -s         ログインシェルの変更
usermod -l         ログイン名の変更
usermod -L         ユーザーをロック（ログインできなくなる）
usermod -U         ユーザーのロックを解除
usermod -p         新規パスワードの発行
```

/etc/default/useradd    useraddコマンド時のデフォルト設定を記述。

```
-------------- /etc/default/useradd -----------------------
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
-----------------------------------------------------------
```

##### grpupadd grpupdel grpupmodコマンド

ユーザー操作のグループ版

##### passwd コマンド

ユーザのパスワード設定を行う。rootユーザのみ全ユーザのパスワードを操作可能。
https://atmarkit.itmedia.co.jp/ait/articles/1612/05/news021.html

```
passwd -l ユーザ名           ユーザーをロック
passwd -u ユーザ名           ユーザーのロックを解除
passwd -x 日数 ユーザ名       パスワードが有効な日数を設定
```

グループとユーザーの追加

```
groupadd user_group
groupadd user_group2             プライマリとセカンダリのグループを作成

getent group | grep user_group   グループのIDを調べておく

-----------------------------------------------------------
user_group:x:1002:
user_group2:x:1003:
-----------------------------------------------------------

useradd -g 1002 -G 1003 user1    プライマリとセカンダリのグループを指定してユーザを作成

id user1                         グループ指定で作成されているか確かめる

-----------------------------------------------------------
[root@localhost ~]# id user1
uid=1001(user1) gid=1002(user_group) groups=1002(user_group),1003(user_group2)
-----------------------------------------------------------

groupdel user_group  => 「groupdel: ユーザ 'user1' のプライマリグループは削除できません。」  * プライマリグループに設定されているグループは削除できない
groupdel user_group2             セカンダリグループは削除できる
id user1                         セカンダリグループが削除されているのを確認

-----------------------------------------------------------
[root@localhost ~]# id user1
uid=1001(user1) gid=1002(user_group) groups=1002(user_group)
-----------------------------------------------------------

cat /etc/shadow | grep user1     パスワードが設定されていないのを確かめる(下記の !! の部分。xだと暗号化されている)

-----------------------------------------------------------
[root@localhost ~]# cat /etc/shadow | grep user1
user1:!!:19017:0:99999:7:::
-----------------------------------------------------------

passwd user1                     パスワードを設定
cat /etc/shadow | grep user1     パスワードが暗号化されているのを確かめる

-----------------------------------------------------------
[root@localhost ~]# cat /etc/shadow | grep user1
user1:$6$COfUFNQdXWNfTt3c$kD89jJ9ct9WalQGRPmF2UFPMvDddBM0Gq7EvpLq2lSrAshU.hbqxMPkD9ZbWNaD8eTdem5diav0uVbcLQBUNM/:19017:0:99999:7:::
-----------------------------------------------------------

passwd -l user1                 パスワードをロック
su - 他のユーザ                  rootからはアクセスできてしまうので別のユーザに切り替える
su - user1                      正しいパスワードを入力しても失敗する
passwd -u user1                 パスワードのロック解除

vi /etc/skel/.bash_logout       下記を追記すると、ユーザ作成時に反映される

-----------------------------------------------------------
echo $USER logoutしました
-----------------------------------------------------------

useradd user2
cat /etc/skel/.bash_logout      echo $USER logoutしました が追記されているのを確認
su - user2
exit                            ログアウト時に追記した記述が表示される
```

<hr>

### ジョブスケジューリング

指定日時にジョブの自動実行を行うジョブスケジューラ

```
crond                           スケジュール管理と実行するデーモン     https://eng-entrance.com/linux-command-crontab

/etc/cron.{daily, hourly, weekly, monthly}/: cron(毎時、毎日、毎週、毎月)実行されるタスク格納
/etc/cron.d                                      クローン起動する時間を記述したファイルを格納
/etc/crontab                                     cronの一番メインのファイル

cat /etc/crontab

-----------------------------------------------------------
SHELL=/bin/bash                         実行するシェルの指定
PATH=/sbin:/bin:/usr/sbin:/usr/bin      パスの定義
MAILTO=root                             実行結果のメール送信先

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
-----------------------------------------------------------

/etc/cron.allow                 crtontabを実行できるユーザを記述
/etc/cron.deny                  crontabを実行できないユーザを記述
/var/spool/cron/                ユーザごとのcrontabファイルを保存したディレクトリ
```

### atコマンド

時間を指定し、コマンドやプログラムを実行するコマンド
https://eng-entrance.com/linux-command-at

```
at 15:00 01042021     2021年1月4日15時に設定したプログラムを実行
    時刻  月 日 年
at -l                 予約中のジョブを表示
at -d or -r           予約中のジョブを削除
at -f                 コマンドを記述したファイルの指定

/etc/at.allow         atコマンドを実行できるユーザを記述
/etc/at.deny          atコマンドを実行できないユーザを記述

atq                   atコマンドで登録されたジョブ一覧表示
atrm                  atコマンドで登録されたジョブを削除
run-parts             クーロンで利用するディレクトリ内のスクリプトを全て実行

/etc/anacrontab       システム起動していない為にジョブが実行されないのを防ぐ。システム停止中のスケジュールも実行される。
```

・コマンドを使ってみる

```
crontab -e            cronを設定(作成したクーロンは /var/spool/cron/root に格納されている)

-----------------------------------------------------------
*/1 * * * uptime >> $HOME/uptime.log        1分ごとにuptimeをログファイルに出力

分 時 日 月 曜日 で設定  =>  分 */1 が1分間隔  その他の設定はいつでも

1 * * * * だと 毎時の1分で実行する
1 12 * * * だと 毎日12時1分に実行する
-----------------------------------------------------------

crontab -l            設定したcronの表示
cd
ls                    1分すぎるとホームディレクトリにuptime.logが作成されている
tail -f uptime.log    別ターミナルを開き、リアルタイムでスケジュールの実行を確認できる
crontab -r            設定したcronを削除


.crontab以外でのスケジュール設定方法①

ls /etc | grep cron   /etc配下のファイルでスケジュール設定できるファイルを確認

-----------------------------------------------------------
[root@localhost ~]# ls /etc | grep cron
anacrontab
cron.d               クローンジョブを記述したディレクトリ(この中にあるファイルでスケジュールを実行する)
cron.daily
cron.deny
cron.hourly          1時間置きの設定を記述するファイル
cron.monthly         1ヶ月置きの設定を記述するファイル
cron.weekly          1週間置きの設定を記述するファイル
crontab
-----------------------------------------------------------

cat /etc/cron.d/0hourly
https://www.server-memo.net/tips/etc-crontab.html
https://www.ricecake24book.com/linux-cron-conf/

------------------------ 0hourly --------------------------
[root@localhost ~]# cat /etc/cron.d/0hourly
# Run the hourly jobs
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root                                 *   スケジュール実行結果を伝えるユーザを記述
01 * * * * root run-parts /etc/cron.hourly  *   スケジュールの設定 毎時1分に rootユーザが /etc/cron.hourly の内容を実行する(run-partsが実行のコマンド)
-----------------------------------------------------------

cp -p /etc/cron.d/0hourly /etc//cron.d/my_cron    0hourlyをコピーしてスケジュールを作成してみる

vim /etc//cron.d/my_cron                          スケジュールの内容を設定

------------------------ my_cron --------------------------
00 * * * * root run-parts /etc/cron.hourly/my_cron.sh    最後の行を変更
-----------------------------------------------------------

vim /etc/cron.hourly/my_cron.sh                   毎時0分に実行するプログラムを作成

------------------------ my_cron.sh -----------------------
#!/bin/bash

uptime >> $HOME/uptime_hourly.log
-----------------------------------------------------------

chmod 755 /etc/cron.hourly/my_cron.sh
date -s '2022/1/27 9:59'                          時刻をスケジュールの実行される直前に変更
cat uptime_hourly.log                             1分経つとこのファイルが作成される
date -s '2022/1/27 9:11'                          時刻を戻しておく

.crontab以外でのスケジュール設定方法②

vim /etc/crontab                                  直接crontabファイルを編集し、スケジュールの設定

-----------------------------------------------------------
2 0 * * * root /etc/cron.daily/crontab_sample.sh   最後の行に追記
-----------------------------------------------------------

vim /etc/cron.daily/crontab_sample.sh             スケジュールで実行するプログラムを記述

--------------------- crontab_sample.sh -------------------
#!/bin/bash

echo crontabを実行 >> $HOME/crontab_daily.log
-----------------------------------------------------------

chmod 755 /etc/cron.daily/crontab_sample.sh

date -s '2022/1/27 00:01'                          時刻をスケジュールの直前に変更
cat crontab_daily.log                              1分経つとこのファイルが作成される
date -s '2022/1/27 09:27'                          時刻を戻しておく


・allowについて

vim /etc/cron.allow                                cronを実行できるユーザを記述

------------------- cron.allow ----------------------------
test2
-----------------------------------------------------------

su - nagasan
crontab -e                                         test2 ではない為エラーになる(cron.allowをnagasanにしていれば使用可)
exit

vim /etc/cron.deny                                 cronを使用禁止にするユーザを記述
```