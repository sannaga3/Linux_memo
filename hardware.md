<hr>

### ハードウェア関連

##### ハードウェア
https://www.sophia-it.com/content/%E3%83%8F%E3%83%BC%E3%83%89%E3%82%A6%E3%82%A7%E3%82%A2  

コンピュータを物理的に構成している回路は周辺機器のこと。CPU、メモリ、HDD、マウス、キーボード、ディスプレイなど。

##### Linuxのハードウェア情報

https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-ja-4/s1-proc-topfiles.html

```
/sys                  ドライバ、カーネル、ハードウェア関連情報。                           システム起動時に作成される。
/proc                 カーネルパラメータ、プロセス情報など。内容変更すると即座に反映される。    システムとプロセス同時に作成される。
/proc/cmdline         システム起動時に、ブートローダーからカーネル起動時に渡されるパラメータが表示される。
/proc/cpuinfo         システムが使用するプロセッサーのタイプを確認。
/proc/inports         登録済みデバイスのポート一覧を表示。デバイスとの入出力時に使われる。
/proc/interrupts      IRQ毎の割り込み回数が記録される。

IRQ...キーボードやマウスなどの入出力デバイスが、CPUを呼び出すときに生成される割り込み要求の信号のこと。ハードウェア毎に0~15の番号が割 り振られ、CPUを使用するための優先度が決められている。
https://kotobank.jp/word/IRQ-62
https://www.techhub.tokyo/words/irqnumber/

proc/scsi/scsi        SCSIデバイス情報。

SCSI...Small Computer System Interface（スモールコンピュータシステムインタフェース）。
       ハードディスクやCD-ROM、イメージスキャナなど、様々な周辺機器の接続に使われていたインタフェース規格。パラレル方式。
       外付けの機器を数珠つなぎに連結（ティジーチェーン）していき、最大７台まで接続可能。2002年でパラレル方式の企画は終了している。
       https://medium-company.com/scsi/
       https://www.ratocsystems.com/products/subpage/scsi/scsi.html

シリアルバスとパラレルバス
  パラレルバスは高速になるほど信号間のタイミングを取りにくくなり、シリアルバスが主流になっている。
  シリアルバス...1bitずつ順番に直列でデータを転送していく。USB、HDD、CD・DVDドライブなどを接続する規格で使われている。
               SATA、IEEE1394、PCI Express、SASなど。
  パラレルバス...複数のbitをまとめて並列回路で転送していく。IDE、SCSI、PCI、パラレルATAなど。
  https://medium-company.com/%e3%83%91%e3%83%a9%e3%83%ac%e3%83%ab%e3%83%90%e3%82%b9-%e3%82%b7%e3%83%aa%e3%82%a2%e3%83%ab%e3%83%90%e3%82%b9/

proc/bus/pci/devices  PCIデバイス情報。

PCI...マザーボードに差込み機能を拡張するPCIカードがある。
      http://www.linux-beginner.com/linux_kihon272.html

proc/bus/pci/devices  USBデバイス情報
proc/dma              DMAチャネルに関する情報

DMA...Direct Memory Access。デバイスとメインメモリがCPUを使わずに高速データ転送を行う為の規格。
DMAコントローラ...マザーボードなどに搭載される、DMA転送の制御を行うICチップ。
DMAチャネル...各装置がDMAコントローラに対して転送を要求するために用いる通信経路.
http://linux-network.cocolog-nifty.com/blog/2011/12/dma-8d7f.html
https://e-words.jp/w/DMA.html


/dev                  HDDや接続されているデバイスのファイルが格納される。システム起動時に作成される。

コールドプラグデバイス...システム停止状態でデバイスの接続を行っておき、システム起動時に認識させる機能。メモリ、ビデオカード、サウンドカードjなど。
ホットプラグデバイス...システム起動状態でデバイスの接続ができる機能。USBメモリなど。

udev                  Linuxカーネル用のデバイス管理ツール。新しいデバイスを接続・解除した時に、事前に定義したルールに基づきデバイスを管理する。つまり、現在接続されているデバイスだけ /dev ディレクトリにデバイスファイルを作成ことができ、無駄がなくなる。/usr/lib/udev/rules.d/配下の 各rulesファイルに定義する。
https://hogetech.info/2020/10/04/udev-%E5%85%A5%E9%96%80/

D-Bus...１台のコンピュータ上で動作する複数のプログラムの間で情報を交換するメッセージバスシステム。デバイスの接続や設定変更の通知に使われる。
http://lfsbookja.osdn.jp/7.10-systemd/chapter06/dbus.html


cat /proc/cpuinfo | less     linuxOSのCPU情報が見れる
cat /proc/scsi/scsi | less   scsi情報を表示
ls  /dev                     接続されたデバイスを表示
ls /etc/udev/rules.d         デバイスルールの定義ファイルを表示
```

<hr>

##### モジュール

Linuxカーネルの機能の一部を、カーネル本体とは別にロード、アンロードできるように分離したバイナリファイルのこと。ハードウェアにアクセスし、操作するためのドライバ部分がモジュールに当たる。
https://atmarkit.itmedia.co.jp/ait/articles/1901/10/news012.html

・モジュールのコマンド
https://atmarkit.itmedia.co.jp/ait/articles/1812/27/news036.html

```
modprobe モジュール名     他のモジュールとの依存関係を考慮し、メモリに読み込む。rootユーザのみ実行可能.
modprobe -r モジュール名  メモリからモジュールを削除する

insmod モジュール名       モジュールをロードする。(依存関係を考慮しない)
rmsmod モジュール名       モジュールをアンロードする。(依存関係を考慮しない)
lsmod                   ロードされているモジュール一覧表示(ロード中のモジュールは/proc/modulesにある)
lspci                   接続されているPCIデバイスを表示    yum install pciutils
lsusb                   接続されているUSBを表示           yum install usbutils

・実際に使ってみる

lsmod | grep -E '^parport|^lp'    parportとlpのどちらもヒットしないのを確認
modprobe lp                       モジュールの読み込み
再度 lsmod | grep -E '^parport|^lp'  lpに依存したparportモジュールも同時に読み込まれた
modprobe -r lp                    parportも一緒に削除

unxz /lib/modules/4.18.0-348.2.1.el8_5.x86_64/kernel/drivers/parport/parport.ko.xz  xzファイルを解凍
insmod /lib/modules/4.18.0-348.2.1.el8_5.x86_64/kernel/drivers/parport/parport.ko   個別にモジュールをロード(lpモジュールも個別でロードが必要)
rmmod  parport
```

