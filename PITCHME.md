# Linuxでのテレビ録画あれこれ

---

## 録画ファイル
\\

過去の録画ファイルがいろいろ置いてあるけど、どうやって録画するの？

---

## ハードウェア
+++
### TVチューナー
+++
#### アースソフト PT3
https://www.amazon.co.jp//dp/B00857CQAM

<img src="https://images-na.ssl-images-amazon.com/images/I/81LCWH7LYIL._SL1500_.jpg" height="250">
- インターフェース: PCI Express x1
- 地上波 2ch、BS/CS 2ch
- 2016年3月までは1万円程度で買えた。プレミアが付いて現在は3万~円
- ドライバはchardev版、DVB版がある
- 録画コマンドはドラバによりrecpt1, recdvb
+++
#### PLEX PX-S1UD
https://www.amazon.co.jp/dp/B0141NFWSG/

<img src="https://images-na.ssl-images-amazon.com/images/I/31FDz970WgL.jpg" height="250">
- インターフェース: USB
- 地上波 1ch
- 5000円くらい
- 録画コマンドはrecdvb
+++
#### PLEX PX-BCUD
https://www.amazon.co.jp/exec/obidos/ASIN/B007L05B88/

<img src="https://images-na.ssl-images-amazon.com/images/I/41w9EwPfoWL.jpg" height="250">
- インターフェース: USB
- BS/CS 1ch
- 8000円くらい。今年の夏前位までは見かけたけど最近在庫なし
- 録画コマンドはrecdvb

+++
### Digibest さんぱくん外出 US-3POUT
https://www.amazon.co.jp/dp/B00C3TNA2G/

<img src="https://images-na.ssl-images-amazon.com/images/I/51DO2s3LRoL._SX425_.jpg" height="250">
- インターフェース: USB
- 地上波/BS/CS 1ch
- B-CASカード付属。ただしカードリーダーはLinuxでは使えないので別途用意する必要あり
- 7000円くらい。在庫が無い事が多いけど月末付近に復活する
- 録画コマンドはrecfsusb2n

+++
### PLEX PX-W3U4
https://www.amazon.co.jp/dp/B01MR4SLB6/

<img src="https://images-na.ssl-images-amazon.com/images/I/31u40Al-28L.jpg" height="250">

- インターフェース: USB
- 地上波 2ch、BS/CS 2ch
- 18000円くらい。
- ドライバがCentOS 6用のバイナリのみしか提供されていない

+++

### B-CASカードリーダー
- e-tax用、住基カード用として売ってる
- 2000円くらい

+++

#### ACR39
https://www.amazon.co.jp/dp/B017Y8QV4O/

<img src="https://images-fe.ssl-images-amazon.com/images/I/311eGtxB9yL.jpg" height="150">

#### SCR3310
https://www.amazon.co.jp/dp/B0085H4YZC/

<img src="https://images-na.ssl-images-amazon.com/images/I/81A-8IXYMzL._SL1500_.jpg" height="150">

+++

### B-CASカード
https://www.b-cas.co.jp/cardorder/view/order/agreement.html

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/78/B-CAS_CARD_3.JPG/1200px-B-CAS_CARD_3.JPG" height="250">

- 再発行手数料 1枚 2,050円

+++

### ストレージ
録画する量に応じてたくさん。

- 1分で126MB
- 30分で3.7GB
- 1時間で7.4GB

---

## ソフトウェア
---
### ドライバ
+++
#### PT3
##### chardev版

https://github.com/m-tsudo/pt3

##### DBV版

Linux kernel 3.18 からカーネルのメインラインに組み込み済み。

chardev版ドラバを使う場合はDVB版ドライバをブラックリストに追加する

+++

#### PX-S1UD
Linux kernel 3.15からカーネルのメインラインに組み込み済み。

#### PX-BCUD
Linux kernel 4.7からカーネルのメインラインに組み込み済み。

+++

#### さんぱくん外出 US-3POUT
ユーザーをvideoグループに追加して、udevルール書けばOK
```
# groupadd video
# gpasswd -a [username] video
# vi /etc/udev/rules.d/91-tuner.rules
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="0511", ATTRS{idProduct}=="0045", MODE="0664", GROUP="video"
# udevadm control --reload
```

+++
#### PX-W3U4
http://www.plex-net.co.jp/product/px-w3u4/download.html

---

### libarib25
暗号化されている放送をB-CASカードを使って復号化するためのライブラリ
https://github.com/stz2012/libarib25

---

### 録画コマンド
チューナー、ドライバによって録画コマンドが異なる。
- recpt1 
  - https://github.com/stz2012/recpt1
- revdvb
  - https://github.com/dogeel/recdvb
- recfsusb2n
  - https://github.com/epgdatacapbon/recfsusb2n

+++

コマンドは違うけど、使い方はほぼ同じ。
```
$ recpt1 --b25 27 1800 ch27_1800.ts
$ revdvb --b25 27 1800 ch27_1800.ts
$ recfsusb2n --b25 27 1800 ch27_1800.ts
```
- --b25: 復号化指定
- 録画チャンネルは物理チャンネルで指定する
  - 16: MX
  - 21: フジテレビ
  - 22: TBS
  - 23: テレ東
  - 24: テレビ朝日
  - 25: 日テレ
  - 26: NHK Eテレ
  - 27: NHK総合
- 録画時間
- 出力ファイル

+++

#### リアルタイム視聴
```
$ recpt1 --b25 --http 8888
$ revdvb --b25 --http 8889
```
VLCメディアプレイヤーでネットワークを開く http://host:8888/24

<img src="assets/img/VLC _open_network.png" height="400">

+++?code=assets/playlist/dvb_playlist.m3u

こんな感じのプレイリストを作って読み込むと、

+++
チャンネル変更のたびにURL打たなくて良いので楽。

<img src="assets/img/VLC _laylist.png" height="400">

---

### epgdump
番組表の抽出

https://github.com/Piro77/epgdump

```
$ epgdump
Usage : ./epgdump <tsFile> <outfile>
Usage : ./epgdump csv  <tsFile> <outfile>
Usage : ./epgdump json <tsFile> <outfile>
```

10秒くらい録画して、epgdumpで番組表を取り出す。
xml、jsonで取り出せる。
```
$ recpt1 --b25 27 10 ch27_10s.ts
$ epgdump ch27_10s.ts ch27.xml
$ epgdump json h27_10s.ts ch27.json
```

+++?code=assets/epgdump/ch27_2017_1224_10s.xml
XML

+++?code=assets/epgdump/ch27_2017_1224_10s.json
json

---

### 録画したTSの処理

---

### TsSplitter
- 録画したファイルは MPEG-2 TS 形式
- HD、 SD、 ワンセグ、EPG、字幕 等の様々な情報を含んでいる
- 目的のビデオストリームとオーディオストリームのみを含んだMPEG-2 TSへ変換する

- clean-ts 
  - https://github.com/eagletmt/eagletmt-recutils/tree/master/clean-ts
- tssplitter_lite
  - http://hp.vector.co.jp/authors/VA038175/download/tssplitter_lite.zip
- tss.py
  - http://allegro.dtiblog.com/blog-entry-177.html
  - http://project.pzfactory.org/blog/archives/112
- TsSplitter.exe
  - http://www3.wazoku.net/2sen/dtvup/
  - http://www3.wazoku.net/2sen/dtvup/source/up0797.zip
+++

```
$ wine TsSplitter.exe -EIT -ECM  -EMM -SD -1SEG 4696-10-20171209-2200.ts
$ ls -l 
-rw-r--r-- 1 haru haru 3.7G 12月 10 14:45 4696-10-20171209-2200.m2t
-rw-rw-r-- 1 haru haru 2.1G 12月 10 14:50 4696-10-20171209-2200_HD.m2t
```
- HDのみを取り出すとMXだと30分あたり 3.7GB が 2.1GB になる。
- MX以外の局だと 2.7GB

---
### ffmpeg
---
### assdumper

---
