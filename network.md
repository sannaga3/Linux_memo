### インターネットプロトコル

/etc/services  TCP及びUDPのポート番号とそのポートが利用するサービスを対応させるファイル  
https://linuc.org/study/knowledge/511/

```
cat /etc/services

--------------- services --------------------
ftp             21/tcp
ftp             21/udp          fsp fspd
ssh             22/tcp                          # The Secure Shell (SSH) Protocol
ssh             22/udp                          # The Secure Shell (SSH) Protocol
---------------------------------------------
```

・TCPとUDP  
クライアントとサーバ間での通信チャネルの提供、通信管理を行う、レイヤー 4（トランスポート層）で動作するプロトコル。  
同じ IP アドレスであっても TCP や UDP のポートが異なれば、提供されるサービスが異なる。  

https://hldc.co.jp/blog/2019/07/11/2819/

```
・TCP (Transmission Control Protocol)  
IPモデルのプロトコル。HTTP、FTP、Telnet などの受け渡しをする。
高信頼性であり、ack による相手の受信確認や再送処理などを行う為、UDPより負荷が重い。  

・UDP (User Datagram Protocol)  
OSI参照モデルのプロトコル。DNS、NTP、DHCP などの受け渡しをする。TCPに比べると信頼性はないが高速に転送を行う。ヘッダサイズが小さい為、アプリケーションのデータを多く送受信可能。  
別途パケットロスの確認が必要。
```

・ICMP  
TCP/IPで補助的に利用されるプロトコル。エラー通知に利用される。

<hr>

### ・OSI参照モデルとTCP/IPモデル

#### OSI参照モデル

国際標準化機構（ISO）によって策定されたコンピュータ間で通信するためのルール。  
1977年から1984年にかけて定義されたが実際には使われず、通信のモデルの概念として普及した。  
https://medium-company.com/osi%E5%8F%82%E7%85%A7%E3%83%A2%E3%83%87%E3%83%AB-tcp-ip-%E9%81%95%E3%81%84/  

・７層構造

https://www.hcnet.co.jp/column/04.html  
https://www.itmanage.co.jp/column/osi-reference-model/  
https://www.n-study.com/network-architecture/osi-reference-model/  
https://thinkit.co.jp/story/2015/04/28/5799

| レイヤ | 名称 | プロトコル | 用途 | 具体例 |
| --- | --- | --- | ---| ---|
| 7層 | アプリケーション層 | HTTP,FTP,DNS,SMTP,POP | 個々のアプリケーション毎の通信の確立(ネットワーク上のアプリケーション同士のファイル転送方式などを規定) | www,メール |
| 6層 | プレゼンテーション層 | SMTP,FTP,Telnet | データ形式の共通化(文字コードなどの変換,暗号化,圧縮) | HTML |
| 5層 | セッション層 | TLS,NetBIOS| 通信チャネルの確率(認証やログインなどセッションの開始・確立,終了) | HTTPS |
| 4層 | トランスポート層 | TCP,UDP | 通信の信頼性を確保 | TCP,UDP |
| 3層 | ネットワーク層 | IP,ARP,ICMP | ネットワーク上の2台のコンピュータの接続を確立信(同一ネットワークである必要はない) | IP |
| 2層 | データリンク層 | PPP,Ethernet | 同一ネットワーク上での通信 | FTP,TELNET,Ethernet |
| 1層 | 物理層 | RS-232,UTP | 接続媒体の選定,電気信号規定 | Web,email,SNS,IP電話,パソコン,サーバー,ケーブルやスイッチも含む |


#### TCP/IPモデル

World Wide Webが発明し、世界標準的に利用されている通信プロトコル（通信規則）。  
TCP(正確な信号を送信する通信規格を用いて正確に通信できるか都度確認するデータ転送方式)とIP（IPアドレス）を用いて、機器やOSが異なっても共通のプロトコルを用いて通信を成立させる。  
TCPの通信では、確立したセッション間でHTTPリクエストとHTTPレスポンスが行われる。

https://www.itmanage.co.jp/column/osi-reference-model/#anc003  

・4層構造

| レイヤ | 名称 | プロトコル | 用途 | 具体例 |
| --- | --- | --- | ---| ---|
| 4層 | アプリケーション層 | HTTP,FTP,DNS,SMTP,POP | 個々のアプリケーション毎の通信の確立(ネットワーク上のアプリケーション同士のファイル転送方式などを規定) | www,メール |
| 3層 | トランスポート層 | IP,ARP,ICMP | ネットワーク上の2台のコンピュータの接続を確立信(同一ネットワークである必要はない) | IP |
| 2層  | インターネット層 | PPP,Ethernet | 同一ネットワーク上での通信 | FTP,TELNET,Ethernet |
| 1層  | ネットワークインターフェス層 | RS-232,UTP | 接続媒体の選定,電気信号規定 | Web,email,SNS,IP電話,パソコン,サーバー,ケーブルやスイッチも含む |

<hr>

### IPアドレス

インターネットに接続された機器(サーバ、ルータ等)に個別に割り当てられる一意の識別番号のこと。  
IPv4は2x32乗、で43億個のアドレスが存在する。IPv６は2x128乗で340澗(1兆の3乗)  

##### 2種類のIPアドレス  

・グローバルIPアドレス  
　プロバイダから与えられる一意のアドレス。外部ネットワーク用に割り当てられる。  
・プライベートIPアドレス  
　内部ネットワークで使うアドレス。異なるネットワークでは同じアドレスを扱える。  

・NAT  
　プライベートIPアドレスをグローバルIPアドレスに変換する技術

###### IPv4

　32bitのアドレスを8bit毎に区切り10進数(0~255)で表記

```
・ネットワーク部とホスト部
プライベートアドレスは前半16桁のネットワーク部と、後半のホスト部に区別される(厳密には16桁とは限らない)。ルーターを介して各機器と接続する
ネットワーク部      組織の識別
ホスト部           ホストPCの識別番号

・予約済みIPアドレス
ホストPCに割りあれられない予約済のIPアドレス。
https://www.infraexpert.com/study/ip4.html
https://e-words.jp/w/%E3%83%96%E3%83%AD%E3%83%BC%E3%83%89%E3%82%AD%E3%83%A3%E3%82%B9%E3%83%88%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9.html

ネットワークアドレス                     ホスト部が全て０bit。ネットワーク自体を指すアドレス。初期設定。
ディレクテッド・ブロードキャストアドレス    IPアドレス全体が1bit。255.255.255.255のこと。ルーターを介さず、自身の属するネットワーク内の全アドレスに対して通信を行う。
リミテッド・ブロードキャストアドレス        ホスト部が全て1bit。別のネットワークに対して、同一ネットワーク上のすべてのホストに対して通信を行う場合のアドレス。ルータを介した先にも送信される。
                                      セキュリティ上の問題があることから、ルータで転送されないようにデフォルトで設定されているのが一般的。
ループバックアドレス                     使用禁止とされているアドレス。自分自身を示す仮想的なIPアドレス。127.0.0.0/8 もしくは 127.0.0.1。
                                      ping 127.0.0.1 としてパケットが正常に通信されていれば、TCP/IPが正常に動作している事を意味する。
＊ ブロードキャスト                      ネットワーク乗の複数のコンピュタに一斉にデータを送信すること。
```

##### グローバルアドレスの区分

5つのクラスに区分される  
https://livescamp.com/ipaddr/

```
クラスA	0.0.0.0 ～ 127.255.255.255	* 0.0.0.0～10.255.255.255は含まれない(プライベートアドレス)
クラスB	128.0.0.0 ～ 191.255.255.255	* 172.16.0.0～172.31.255.255は含まれない(プライベートアドレス)
クラスC	192.0.0.0 ～ 223.255.255.255	* 192.168.0.0～192.168.255.255は含まれない(プライベートアドレス)
クラスD	224.0.0.0 ～ 239.255.255.255	IPマルチキャスト通信で利用される特別なIPアドレス
クラスE	240.0.0.0 ～ 255.255.255.255	研究目的に予約されていて利用できない。
```

##### プライベートアドレスの区分

3つのクラスに区分される

```
IPアドレスの範囲はクラスAが大きく、クラスCが小さい。家庭のWiFi接続ではクラスCが採用される。
https://www.iodata.jp/lib/manual/wn-wag-cb/htm/tcp-1.htm

クラスA: 10.0.0.0 〜 10.255.255.255       （10.0.0.0/8）    約1600万台
クラスB: 172.16.0.0 〜 172.31.255.255     （172.16.0.0/12） 約65000台
クラスC: 192.168.0.0 〜 192.168.255.255   （192.168.0.0/16）254台
```

##### サブネットマスク

ネットワークの範囲を定義するもの。  
IPアドレスとサブネットマスクを組み合わせることで、ネットワークの範囲を指定できる。  
https://www.cman.jp/network/term/subnet/

```
  IPアドレス    サブネットマスク      ネットワーク範囲                接続台数
172.16.0.1 / 255.255.255.0	172.16.0.0 ～ 172.16.0.255       254
           / 255.255.0.0	172.16.0.0 ～ 172.16.255.255     65534

サブネットマスクの使用していない部分がネットワーク範囲として、PCなどに割り当てられる。(サブネットマスクで2進数で1になる部分がネットワークアドレス、0になる部分がホストアドレスを指す)
255の表記に関して => 2進数(1bit)x8bit(8乗)=256(0~255の範囲を扱う)
アドレスの先頭(ネットワークアドレス)と最後(ブロードキャストアドレス)には割り当てができない為、実際の接続数は-2で計算する
https://livescamp.com/ipaddr/
```

クラス別サブネットマスク早見表  
https://www.ahref.org/doc/ipsubnet.html

##### CIDR(Classless Inter Domain Routing サイダー)

クラス区分により生じるネットワーク部・ホスト部の無駄を省く為の仕組み。  
クラスに縛られずIPアドレスのネットワーク部・ホスト部の桁数を自由に決めることができる。  
IPアドレス / prefix で記述する。  prefixの範囲はサブネット。  
https://www.iodata.jp/lib/manual/wn-wag-cb/htm/tcp-1.htm

```
  196   .  168  .  2     .   112 / 28    アドレスの先頭から28桁目までがネットワーク部で下4桁がホスト部。
11000000 10101000 00000010 0111 0000     192.168.2.112 ~ 192.168.2.127 


http://atnetwork.info/tcpip1/tcpip105.html
```

<hr>

### ポート

TCP/IP通信ではIPアドレスとポート番号をセットで用いて通信を行う。ポートはコンピュータが通信に使用するプログラムを識別するための番号。

##### ポート番号と対応プロトコル一覧

https://pasomaki.com/web-port-number/

| 番号 | プロトコル | サービス | 内容 |
| ---- | ----    | ---    | ---|
| 22   | TCP     | ssh    | リモートログイン（セキュア）|
| 23   | TCP/UDP | telnet | リモートログイン |
| 25   | TCP/UDP | smtp   | メール送信 |
| 53   | TCP     | domain | DNS |
| 80   | TCP     | http   | www |
| 110  | TCP     | pop3   | メール受信（バージョン3）|
| 123  | TCP     | ntp    | 時刻の同期 |
| 443  | TCP/UDP | https  | SSL/TLSによるhttp接続 |

・ポート80と443に関して
URLではドメイン名の後の:80 :443 が省略されている。それらがポート番号を表し、http、http通信でアクセスしているということ。  
https://qiita.com/shizen-shin/items/511aa4b1ad0c0ee2a434


##### telnet

ポート番号23を使い、ネットワークに接続された機器を遠隔操作する為に使用するアプリケーション層のプロトコル。  
PCではTelnetクライアント、ルータなどの機器にはTelnetサーバのサービスを有効にする必要がある。  
認証や通信の暗号化は行わない為、公のネットワークで使用すべきでない。  

https://eng-entrance.com/linux-command-telnet  
https://www.psid.co.jp/news/2020/10/19/%E3%80%902020%E6%96%B0%E5%8D%92%E7%A0%94%E4%BF%AE%E6%97%A5%E8%A8%98%E3%80%91ssh%E3%81%A8telnet%E3%81%AE%E9%81%95%E3%81%84%E3%82%84%E7%89%B9%E5%BE%B4%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%E3%80%902020/

<hr>

### ルーティング

https://www.fenet.jp/infla/column/network/%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%81%A8%E3%81%AF%EF%BC%9F%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%988%E3%81%A4%E3%81%A8%E8%A6%8B%E6%96%B9%EF%BD%9C/  
https://ascii.jp/elem/000/000/592/592307/

TCP/IPプロトコルを通じて、適切な経路で通信を行うこと。ルーターとルーティングテーブルの情報を扱う。

##### ルーティングテーブル

ルーティング処理を行う際に参照するルータに記録された経路情報。

##### ルーティングテーブルのデータ構造

```
ホストルート：サブネットが /32。         ネットワーク接続されたルーターがIPパケットをホストへ転送するための経路。ネットワークルートが設定されていることが前提。
ネットワークルート：サブネット（/24 等）  ルーティングテーブルの内容の表示・設定を行う。
デフォルトルート： 0.0.0.0/0           経路表に記載がない場合の転送先を定めたもの。ネクストホップなどが指定される。
ネクストホップ                         次に転送すべき隣接ルータのIPアドレスのこと。       https://e-words.jp/w/%E3%83%8D%E3%82%AF%E3%82%B9%E3%83%88%E3%83%9B%E3%83%83%E3%83%97.html
ローカルループバック                    同じ端末内のプログラム同士の通信経路
ローカルルート                         同じネットワーク（サブネット）内の経路
```

##### デフォルトゲートウェイ

 内部ネットワークと外部ネットワークを繋ぐためのネットワーク機器。ルータがデフォルトゲートウェイの役割をすることが多い。  
 https://medium-company.com/%E3%83%87%E3%83%95%E3%82%A9%E3%83%AB%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4/

<hr>

### ネットワークのファイルとコマンド

```
hostnamectl           ホスト名を確認するコマンド

-------------------------------------------------------
[root@localhost ~]# hostnamectl
   Static hostname: localhost.localdomain
         Icon name: computer-vm
           Chassis: vm
        Machine ID: ecaf490c35de4ea3b68a48a0fb5242ad
           Boot ID: baea04d32b35435a950693c406a0e2b9
    Virtualization: oracle
  Operating System: CentOS Linux 8
       CPE OS Name: cpe:/o:centos:centos:8
            Kernel: Linux 4.18.0-348.2.1.el8_5.x86_64
      Architecture: x86-64
--------------------------------------------------------

/etc/hostname         自身のPCのホスト名が記述されているファイル。

/etc/hosts            ホスト名とIPアドレスを対応させるためのファイル    https://linuc.org/study/knowledge/506/

--------------------------------------------------------
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
--------------------------------------------------------

* ::1 はIPV6のループバックアドレス  ping localhostでデフォルト設定ではIPv6で通信する
https://long-distance.jp/sb/log/eid23.html

--------------------------------------------------------
[root@localhost ~]# ping localhost
PING localhost(localhost (::1)) 56 data bytes
64 bytes from localhost (::1): icmp_seq=1 ttl=64 time=0.050 ms
64 bytes from localhost (::1): icmp_seq=2 ttl=64 time=0.100 ms
--------------------------------------------------------

hostnamectl set-hostname    ホスト名の変更

・NetworkManager
systemdによって管理されるCentOSのネットワークを管理する機能
https://qiita.com/gomi_ningen/items/baee5fada818a8ba944a

systemctl start NetworkManager
```

##### nmcliコマンド
ネットワークの状態把握・管理を行うNetworkManagerのコマンド
https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_ip_networking_with_nmcli

```
nmcli オブジェクト コマンド で記述
```

・オブジェクトとコマンド一覧
| オブジェクト | コマンド | 内容 |
| --- | ---| --- |
| general | status | NetworkManagerの状態確認 |
|         | hostname | telnet |
|         | hostname ホスト名 | smtp   |
| networking | on または off | domain |
|            | connectivity | http   |
|            | wifi | WiFiの状態確認 |
|            | wifi on または off | WiFiを有効・無効にする |
| radio | wwan | モバイルブロードバンドの状態を表示 |
|       | wwan on または off | モバイルブロードバンドを有効・無効にする |
|       | wwan on または off | モバイルブロードバンドを有効・無効にする |
|       | all on または off | 全ての有効・無効にする |
| connection  | show | 接続情報を表示 --activeは接続されているもののみを表示 |
|       | modify インターフェス名 | インターフェスの接続 |
|       | up ID | 接続を有効にする |
|       | down ID | 接続を無効にする |
| device | status | 全ての有効・無効にする |
|        | show インターフェス名 | デバイスの情報表示 |
|        | modify インターフェス名 | デバイスの設定 |
|        | connect インターフェス名 | デバイスの接続 |
|        | disconnect インターフェス名 | デバイスの切断 |
|        | delete インターフェス名 | デバイスの削除 |
|        | wifi list | WiFiのアクセスポイント一覧を表示 |
|        | wifi connect SSID | WiFiのアクセスポイントに接続 |
|        | wifi hotspot | WiFiのホットスポット作成 |
|        | wifi rescan | WiFiのアクセスポイントを再検索 |


##### モバイルブロードバンド

携帯電話やスマートフォン、モバイルPCなどの移動体通信機器（モバイル機器）を使用して、高速な通信速度（数Mbps〜数十Mbps）でインターネットに接続すること。４G、５G、LTE,モバイルルーターなどを用いてインターネットに接続する。  
http://touch-slide.jp/glossary/web%E6%8B%85%E5%BD%93%E8%80%85%E6%A7%98%E5%90%91%E3%81%91/%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E3%83%96%E3%83%AD%E3%83%BC%E3%83%89%E3%83%90%E3%83%B3%E3%83%89/


##### ipコマンド

ネットワークやルーティングの表示や設定を行う。ifconfigコマンドの後継。
https://linuxfan.info/ip-address  
https://ameblo.jp/bakery-diary/entry-12618075556.html

```
ip a(addrの省略)         ネットワークデバイスごとにアドレス情報を表示。ip addr show や ip addressでも同じ結果が表示される。

1: lo:  =>   localhost。自身のデバイス情報
2: enp0s3:    enp0s3、eth0、eth1、ens１、enp2s0などにあたる部分がデバイス名。inetがIPv4アドレス。inet6がIPv6のアドレス。

ip -4 a      IPv4の情報のみ表示

ip link                 ネットワークインターフェースの接続状況を表示
ip -c link              ip linkをわかりやすく項目毎に色分けして表示。インターフェース名、MACアドレス、リンク状態（UP/DOWN）。
https://engineers-life.com/linux_command/network_linux/linux_ip_link/

ip r(route)             ルーティングテーブルの内容を表示

・ipコマンドのサブコマンド
show 表示   add 追加   del 削除

ip addr add IPアドレス dev デバイス名                            ネットワークデバイスにIPアドレスを追加
ip addr del IPアドレス dev デバイス名                            ネットワークデバイスからIPアドレスの削除
ip route add アドレス/サブネット via ゲートウェイ dev デバイス名     ルーティングテーブルにエントリ(経路情報)の追加
ip route del アドレス/サブネット via ゲートウェイ dev デバイス名     ルーティングテーブルからエントリの削除
```

##### ifconfigコマンド

ipコマンドの旧式
https://eng-entrance.com/linux-command-ifconfig

```
ifconfig                            ネットワークの接続状況の確認
ifconfig ネットワーク名 up(down)      ネットワークの有効・無効化
ifconfig ネットワーク名 mtu 数値       1パケットのデータ量(MTU)変更
ifconfig ネットワーク名 ネットワーク    ネットワークの変更
ifconfig ネットワーク名:0 ネットワーク  ネットワークのエイリアスを設定

ifup    ネットワークインターフェス名       ネットワークインタフェスの有効化
ifdown  ネットワークインターフェス名       ネットワークインタフェスの無効化

route                               ルーティングの追加・削除を設定するコマンド。CentOS7以降非推奨、ipコマンドを使う。
```

##### ping コマンド

宛先までIPパケットが到達するか確認するコマンド  
https://eng-entrance.com/linux-command-ping

```
ping                                            Ctrl+Cが押されるまで接続を続ける

ping -i 時間(1/1000秒) IPアドレス or ホスト名       指定時間接続できるかチェック
ping -c 回数 IPアドレス or ホスト名                 回数分正常に接続できるかチェック
ping -s パケットサイズ 接続先IPアドレスまたはホスト名   パケットサイズを指定して送受信できるかチェック(実際には指定したパケットサイズ＋8バイトが送信される)
ping6                                            IPv6で
```

##### traceroute コマンド

あるホストから別のホストまでのネットワーク経路をリスト表示するコマンド  
https://atmarkit.itmedia.co.jp/ait/articles/0108/30/news003.html

```
traceroute       pingで相手ホストから正常な応答がなかった場合などに、ホスト自身や経路上のルーターのルーティング設定が正しいかどうかを確認する。
                 各ルーターからのレスポンス時間などの統計値も表示される。
```

##### netcat

CPもしくはUDP接続などを利用して、コマンドラインからデータを送受信するためのツール。ncコマンドを用いる。
https://www.intellilink.co.jp/column/security/2015/070100.aspx

```
nc 接続先ホスト ポート番号 で記述
nc -l 9999                    リッスンモード。9999(TCP)のポートを開く(サーバー側として起動)  *疎通に成功するとサーバ側のプロセスが終了する
nc -ul 9999                   リッスンモード。9999(UDP)のポートを開く(サーバー側として起動)  *疎通に成功するとサーバ側のプロセスが終了する
https://orebibou.com/ja/home/201511/20151117_001/

nc -kvl ポート番号                TCPのポートを開く(サーバー側として起動) *サーバー側のプロセスを終了するまで起動し続ける
nc -e /bin/cat -uvl ポート番号    UDPのポートを開く(サーバー側として起動) *サーバー側のプロセスを終了するまで起動し続ける

nc ホスト名 ポート番号             指定したホスト名のポート番号へ接続(クライアント側)
```

##### netstat コマンド

TCP/IP関連のトラブルシューティングを行う場合に、ほぼ必ず使うコマンド。通信中のTCPコネクション（TCP接続）の状態を表示する。  
https://atmarkit.itmedia.co.jp/ait/articles/0207/20/news003.html

```
netstat          アクティブなTCPコネクションを一覧表示

------------------------------------------------------------
[root@localhost ~]# netstat
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 localhost.localdoma:ssh _gateway:62339          ESTABLISHED
tcp        0      0 localhost.localdoma:ssh _gateway:60126          ESTABLISHED
tcp        0      0 localhost.localdoma:ssh _gateway:51759          ESTABLISHED
udp        0      0 localhost.locald:bootpc _gateway:bootps         ESTABLISHED
Active UNIX domain sockets (w/o servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  26     [ ]         DGRAM                    11790    /run/systemd/journal/dev-log
unix  2      [ ]         DGRAM                    24154    /run/chrony/chronyd.sock
unix  2      [ ]         DGRAM                    32866    /run/user/1000/systemd/notify
unix  3      [ ]         DGRAM                    11749    /run/systemd/notify
unix  2      [ ]         DGRAM                    11751    /run/systemd/cgroups-agent
unix  7      [ ]         DGRAM                    11765    /run/systemd/journal/socket
unix  3      [ ]         STREAM     CONNECTED     23227    /run/systemd/journal/stdout
-----------------------------------------------------------

netstat -aA inet  ホスト名とプロトコル名は数値で表示し、IPv4に限定する
https://atmarkit.itmedia.co.jp/fnetwork/netcom/netstat/netstat.html
https://network-beginners-handbook.com/netstat/

-----------------------------------------------------------
[root@localhost ~]# netstat -aA inet
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:sunrpc          0.0.0.0:*               LISTEN
tcp        0      0 localhost.locald:domain 0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN
tcp        0      0 localhost.localdoma:ssh _gateway:62339          ESTABLISHED
tcp        0      0 localhost.localdoma:ssh _gateway:60126          ESTABLISHED
tcp        0      0 localhost.localdoma:ssh _gateway:51759          ESTABLISHED
udp        0      0 localhost.locald:domain 0.0.0.0:*
udp        0      0 0.0.0.0:bootps          0.0.0.0:*
udp        0      0 localhost.locald:bootpc _gateway:bootps         ESTABLISHED
udp        0      0 0.0.0.0:sunrpc          0.0.0.0:*
udp        0      0 0.0.0.0:ntp             0.0.0.0:*
udp        0      0 0.0.0.0:mdns            0.0.0.0:*
udp        0      0 localhost:323           0.0.0.0:*
udp        0      0 0.0.0.0:50738           0.0.0.0:*
-----------------------------------------------------------
```

##### ss コマンド

netstatの後継コマンド。  
https://milestone-of-se.nesuke.com/sv-basic/linux-basic/ss-netstat/

```
ss -nltu         ポートの開放状態確認

オプション
-n : ポート番号をサービス名変換しない
-t : TCP を表示
-u : UDP を表示

ss -nltup        ポートを開放しているプロセスも表示する

・STATEについて
LISTEN  =>  TCPの待ち受けポート
UNCONN  =>  UDPの待ち受けポート
ESTAB   =>  TCPコネクションが確立した状態

・Recv-QとSend-Qについて
State = ESTAB の場合 => Recv-Q はプロセスに引き渡されていないパケットの総byte数を表す
                       Send-Q は通信先から TCP ack が返ってきていないパケットの総byte数を表す
State = LISTEN の場合 => Recv-Q は現在の TCP コネクション未確立の数
                        Send-Q は受け入れ可能な最大TCPコネクション未確立通信の数
```

・コマンドを使ってみる

```
hostname   hostnameの確認

------------------------------------------------------------------
[root@localhost ~]# hostname
localhost.localdomain
------------------------------------------------------------------

cat /etc/hosts      hostnameとIPアドレスの確認。ホスト名を指定してサービスに接続できる。

------------------------------------------------------------------
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

* ::1 はIPv6のループバックアドレスのこと。先頭の連続した０は ::1 と省略可能、0000:0000:0000:0000:0000:0000:0000:0001を意味する。
https://ja.stackoverflow.com/questions/22927/1-%E3%81%8C-%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E3%83%9B%E3%82%B9%E3%83%88%E3%81%AEip%E3%81%A8%E3%81%AA%E3%82%8B%E3%81%AE%E3%81%AF%E3%81%AA%E3%81%9C
------------------------------------------------------------------

hostnamectl set-hostname "new_hostname"      ホスト名の変更

systemctl start docker                       dockerを起動する
docker run -it -d centos                     バックグラウンドでCentOSを起動しておく
docker ps                                    CentOSの起動を確認
docker inspect コンテナID                     コンテナに関する全ての情報を取得し、IPアドレスを確認。"IPAddress": "172.17.0.2",
ping 172.17.0.2                              指定アドレスにパケット通信を行い、正確に到達するか確認

------------------------------------------------------------------
[root@localhost ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.053 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.066 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.105 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.080 ms
^C
--- 172.17.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3069ms
rtt min/avg/max/mdev = 0.053/0.076/0.105/0.019 ms
------------------------------------------------------------------

vi /etc/hosts                     複数のhost名とIPアドレスを追記(IPアドレスでなく、ホスト名でサービスを指定できるようにする)

---------------------- /etc/hosts --------------------------------
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
172.17.0.2  my_docker my_docker.com sample.com            ＊ 追記
------------------------------------------------------------------

ping my_docker                    ホスト名でサービスを指定(my_docker.com sample.comで指定してもOK)

systemctl start NetworkManager    NetworkManagerを起動
nmcli general hostname            ホスト名の確認
nmcli general status              NetworkManagerの状態確認

------------------------------------------------------------------
[root@localhost ~]# nmcli general status
STATE     CONNECTIVITY  WIFI-HW  WIFI  WWAN-HW  WWAN
接続済み      完全          有効     有効  有効     有効

* CONNECTIVITY 完全 =>  インターネットに接続可能状態
------------------------------------------------------------------

nmcli networking connectivity     インターネットへの接続可能状態を表す (none,portal,limited,full,unknown) * full => インターネットへ接続されていて、アクセス可能な状態

man nmcli => /connectivity        manコマンドで各状態を調べる

------------------------------------------------------------------
  none
      the host is not connected to any network.
  portal
      the host is behind a captive portal and cannot reach the full Internet.
  limited
      the host is connected to a network, but it has no access to the Internet.
  full
      the host is connected to a network and has full access to the Internet.
  unknown
      the connectivity status cannot be found out.
------------------------------------------------------------------

nmcli radio all                   WiFiの状態確認

------------------------------------------------------------------
[root@localhost ~]# nmcli radio all
WIFI-HW  WIFI  WWAN-HW  WWAN
有効     有効  有効     有効
------------------------------------------------------------------

nmcli connection show             ネットワークの接続情報一覧(showは付けなくても動作する)

------------------------------------------------------------------
[root@localhost ~]# nmcli connection show
NAME     UUID                                  TYPE      DEVICE
enp0s3   11b8e0ba-bd0a-43fa-bd4d-adb860fef1d4  ethernet  enp0s3      =>  外部ネットワークとの接続
docker0  416c128d-fc70-42df-9560-133e9a8e87d7  bridge    docker0     =>  docker
virbr0   730fb121-d8c7-434d-9887-c014720f5f03  bridge    virbr0
------------------------------------------------------------------

nmcli device status               ネットワークに接続しているデバイスの表示(カーネルの認識しているデバイスの表示)

* device(カーネルの認識する物理インターフェス)にconnection(ネットワーク設定（IPアドレスなど）)を紐付け、個別に管理している
https://endy-tech.hatenablog.jp/entry/2018/09/08/140950

------------------------------------------------------------------
[root@localhost ~]# nmcli device status
DEVICE       TYPE      STATE            CONNECTION
enp0s3       ethernet  接続済み          enp0s3
docker0      bridge    接続済み (外部)    docker0
virbr0       bridge    接続済み (外部)    virbr0
vethce39e66  ethernet  管理無し           --
lo           loopback  管理無し           --
virbr0-nic   tun       管理無し           --
------------------------------------------------------------------

ip -c link show                   ネットワークデバイスの情報を表示

------------------------------------------------------------------
[root@localhost ~]# ip -c link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:cb:64:3c brd ff:ff:ff:ff:ff:ff
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:4f:1a:1b brd ff:ff:ff:ff:ff:ff
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:4f:1a:1b brd ff:ff:ff:ff:ff:ff
5: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default
    link/ether 02:42:5f:3c:92:db brd ff:ff:ff:ff:ff:ff
7: vethce39e66@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP mode DEFAULT group default 
    link/ether 72:33:ce:a7:16:19 brd ff:ff:ff:ff:ff:ff link-netnsid 0

*  mtu 1500 => 1パケットあたりの通信量
------------------------------------------------------------------

ip addr show                      IPアドレス情報を表示

------------------------------------------------------------------
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:cb:64:3c brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3     =>  virturlboxの設定によるが、10.0.2.15になることが多い？  https://www.sps-katoh.com/soliloquy/?p=108
       valid_lft 61099sec preferred_lft 61099sec
    inet6 fe80::a00:27ff:fecb:643c/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:4f:1a:1b brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0        => 外部ネットワークに接続するIPアドレス
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state DOWN group default qlen 1000
    link/ether 52:54:00:4f:1a:1b brd ff:ff:ff:ff:ff:ff
5: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:5f:3c:92:db brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0           =>  dockerのプライベートIPアドレス
       valid_lft forever preferred_lft forever
    inet6 fe80::42:5fff:fe3c:92db/64 scope link
       valid_lft forever preferred_lft forever
------------------------------------------------------------------

ip route show                      通信経路を表示

------------------------------------------------------------------
[root@localhost ~]# ip route show
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100                 =>  デフォルトゲートウェイの情報
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1      =>  dockerのプライベートIPアドレス
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown  =>  外部ネットワークに接続するIPアドレス
------------------------------------------------------------------

ip route add 192.168.122.0/24 via 172.17.0.2                          192.168.122.0/24の範囲で接続する場合は172.17.0.2をゲートウェイとして使用する
traceroute 192.168.122.2                                              上記でdockerを経路にしているので表示される
ip route del 192.168.122.0/24                                         通信経路の削除
ip r                                                                  経路削除の確認
traceroute 192.168.122.2                                              デフォルトゲートウェイが表示される

ifconfig                                                              起動中のインターフェイスのネットワーク状態を確認
ifconfig docker0                                                      サービス名を指定してネットワーク状態を確認
ifconfig docker0 down                                                 サービスの停止

------------------------------------------------------------------
* サービスを停止できないエラーが発生  https://www.fixes.pub/linux/236091.html
sudo ip addr flush dev docker0                                        IPアドレスを消去
ifconfig docker0 172.17.0.2 netmask 255.255.255.0                     IPアドレスを再設定

*  アドレスを変更してもサービスがダウンできない(ifconfigでdocker0はdown担っているが、ping 172.17.0.2でパケットは送信され、docker psでもコンテナがdownしていなかった)
------------------------------------------------------------------


```