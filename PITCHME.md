# Linuxでのテレビ録画あれこれ

---

### 動画ファイル
smb://TILTOWAITO\Public\movie

過去の録画ファイルがいろいろ置いてあるけど、どうやって録画するの？

---

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

<img src="https://images-na.ssl-images-amazon.com/images/I/51DO2s3LRoL._SX425_.jpg" height="200">
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

#### US-3POUT
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
$ revdvb --b25 --http 8888
```
VLCメディアプレイヤーでネットワークを開く http://host:8888/24

Android、iOS版のVLCでも可。

<img src="assets/img/VLC _open_network.png" height="350">

+++?code=assets/playlist/dvb_playlist.m3u
`tv_channel.m3u`

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
Usage : epgdump <tsFile> <outfile>
Usage : epgdump csv  <tsFile> <outfile>
Usage : epgdump json <tsFile> <outfile>
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

### 録画したファイルの処理

---

### TsSplitter
- 録画したファイルは MPEG-2 TS 形式
- HD、 SD、 ワンセグ、EPG、字幕 等の様々な情報を含んでいる
- 目的のビデオストリームとオーディオストリームのみ取り出す

+++ 
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
- BonTsDemuxC.exe
  - http://www2.wazoku.net/2sen/friioup/
  - http://www2.wazoku.net/2sen/friioup/source/up1091.zip
+++

```
$ wine TsSplitter.exe -EIT -ECM -EMM -SD -1SEG input.ts
$ ls -l 
-rw-r--r-- 1 haru haru 3.7G 12月 10 14:45 input.ts
-rw-rw-r-- 1 haru haru 2.1G 12月 10 14:50 input_HD.ts
```
- HDのみを取り出すとMXだと30分あたり 3.7GB が 2.1GB になる。
- MX以外の局だと 2.7GB

---
### ffmpeg
https://www.ffmpeg.org/

録画したままのMPEG-2 TSだとファイルサイズが大きいので、MP4にエンコードしてファイルサイズを小さくする

+++
#### H.264
```
$ ffmpeg -i input.ts \
  -s 1920x1080 -aspect 16:9 -r 30000/1001 -f mp4 \
  -vcodec libx264 -preset ultrafast -crf 22 -tune animation -vf yadif \
  -acodec libfdk_aac -ac 2 -ar 48000 -vbr 4 -async 100 \
  -ssim 1 output.mp4
```
- -i 入力ファイル名
- -s 出力解像度 1920x1080, 1280x720, 768x432, 640x360
- -preset エンコード設定のプリセット ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow, placebo
- -crf エンコード品質 0-51の範囲で指定する
- -tune 特定のソース向けのチューニング film, animation
- -vf ビデオフィルタ yadif はインターレース解除フィルタ

+++
- ソースは30分、ファイルサイズ 2.1GB
- CPU: Core i7 4770K (3.5GHz 4C/8T)

+++

###### プリセット毎のエンコード時間とファイルサイズ

|プリセット|解像度|crf|時間|サイズ|SSIM
|-------:|-----:|--:|--:|----:|:-------:|
|ultrafast|1920x1080|22|0:07:28|1.53GB|0.9856122|
|superfast|1920x1080|22|0:08:50|1.18GB|0.9881147|
|veryfast|1920x1080|22|0:11:19|731MB|0.9859923|
|faster|1920x1080|22|0:16:28|863MB|0.9881946|
|fast|1920x1080|22|0:20:09|919MB|0.9883434|
|medium|1920x1080|22|0:25:32|836MB|0.9881622|
|slow|1920x1080|22|0:39:10|794MB|0.9880770|
|slower|1920x1080|22|1:26:38|785MB|0.9882092|
|veryslow|1920x1080|22|2:48:38|721MB|0.9878426|
|placebo|1920x1080|22|6:19:18|735MB|0.9879642|

+++
###### 出力解像度毎のエンコード時間とファイルサイズ
|プリセット|解像度|crf|時間|サイズ|SSIM
|-------:|-----:|--:|--:|----:|:-------:|
|veryslow|1920x1080|22|2:48:38|721MB|0.9878426|
|veryslow|1280x720|22|0:47:55|363MB|0.98662260|
|veryslow|768x432|22|0:18:53|174MB|0.98597470|
|veryslow|640x360|22|0:14:43|141MB|0.98576910|

+++
###### crf毎のエンコード時間とファイルサイズ
|プリセット|解像度|crf|時間|サイズ|SSIM
|-------:|-----:|--:|--:|----:|:-------:|
|veryslow|1280x720|18|0:57:37|640MB|0.9899438|
|veryslow|1280x720|19|0:54:49|552MB|0.9891958|
|veryslow|1280x720|20|0:52:27|479MB|0.9884105|
|veryslow|1280x720|21|0:51:04|416MB|0.9875544|
|veryslow|1280x720|22|0:47:55|363MB|0.9866226|
|veryslow|1280x720|23|0:47:13|319MB|0.9856278|
|veryslow|1280x720|24|0:46:23|282MB|0.9845350|
|veryslow|1280x720|25|0:45:22|250MB|0.9833251|
|veryslow|1280x720|26|0:44:50|223MB|0.9819973|
|veryslow|1280x720|27|0:42:06|200MB|0.9805385|
|veryslow|1280x720|28|0:40:34|181MB|0.9789239|

+++
#### H.265
```
$ ffmpeg -i input.ts \
  -s 1920x1080 -aspect 16:9 -r 30000/1001 -f mp4 \
  -vcodec libx265 -preset ultrafast -crf 22 -vf yadif \
  -acodec libfdk_aac -ac 2 -ar 48000 -vbr 4 -async 100 \
  -ssim 1 output.mp4
```

---

### 字幕
#### assdumper
https://github.com/eagletmt/eagletmt-recutils/tree/master/assdumper

---

### CMカット
#### Comskip
https://github.com/erikkaashoek/Comskip

---

### 録画アプリケーション

+++
#### epgrec UNA
http://d.hatena.ne.jp/katauna/

http://tekinani.blogspot.jp/2011/09/blog-post.html

- 日経Linuxの記事で作られたやつのフォーク版
- PHP/Mysql

+++
#### foltia
http://www.dcc-jpl.com/soft/foltia/

- アニメ録画特化
- しょぼいカレンダー( http://cal.syoboi.jp/ )と連携して動く
- PHP,Perl/Postgres,SQLite
- 最近開発されてない模様。PHP7で動かない、文字コードがEUC-JP等今では使いにくい。
- 文字コードをUTF-8にしてPHP7でも動くようにしたやつ
  - https://github.com/haru8/foltia 

+++
#### foltia ANIME LOCKER
https://foltia.com/ANILOC/

- foltiaの有償版
- アプリケーションではなく、CentOS6ベースのディストリビューションとして配布されているので、既存のサーバーには導入しにくい
- コンソールに殆ど触れずにWebだけで導入できるのでLinuxに不慣れな人に優しい
- gizazineに最近レビューが出てた
  - https://gigazine.net/news/20171208-foltia-anime-locker/

+++
#### Chinachu / Mirakurun
https://chinachu.moe/

https://github.com/Chinachu

- 最近一番活発
- チューナーの抽象化サーバー(Mirakurun)と録画サーバー(Chinachu)の組み合わせ
- node

---

### 電気代

- CPU: Core i7 4770K
- 3.5インチ HDD ×2
- 2.5インチ SSD ×1
- PT3 × 2


- アイドル時: 40W前後 |
- エンコード中: 110W前後 |
- 30日間の使用電力量: 44.1KWh |
- 1KWhあたり26円で計算すると1月辺り: 1146円 |

---

## おわり



