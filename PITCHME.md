# Linuxでのテレビ録画あれこれ

---

## 録画ファイル
\\

過去の録画ファイルがいろいろ置いてあるけど、どうやって録画するの？

---

## 必要なハードウェア
+++
### TVチューナー
+++
#### アースソフト PT3
https://www.amazon.co.jp//dp/B00857CQAM

<img src="https://images-na.ssl-images-amazon.com/images/I/81LCWH7LYIL._SL1500_.jpg" height="300">
- インターフェース: PCI Express x1
- 地上波 2ch、BS/CS 2ch
- 2016年3月までは1万円程度で買えた。プレミアが付いて現在は3万~円
- ドライバはchardev版、DVB版がある
- 録画コマンドはドラバによりrecpt1, recdvb
+++
#### PLEX PX-S1UD
https://www.amazon.co.jp/dp/B0141NFWSG/

<img src="https://images-na.ssl-images-amazon.com/images/I/31FDz970WgL.jpg" height="300">
- インターフェース: USB
- 地上波 1ch
- 5000円くらい
- 録画コマンドはrecdvb
+++
#### PLEX PX-BCUD
https://www.amazon.co.jp/exec/obidos/ASIN/B007L05B88/

<img src="https://images-na.ssl-images-amazon.com/images/I/41w9EwPfoWL.jpg" height="300">
- インターフェース: USB
- BS/CS 1ch
- 8000円くらい。今年の2,3月位までは見かけたけど最近在庫なし
- 録画コマンドはrecdvb

+++
### Digibest さんぱくん外出 US-3POUT
https://www.amazon.co.jp/dp/B00C3TNA2G/

<img src="https://images-na.ssl-images-amazon.com/images/I/51DO2s3LRoL._SX425_.jpg" height="300">

- インターフェース: USB
- 地上波/BS/CS 1ch
- 7000円くらい。在庫ないことが多いけど月末付近に復活する
- 録画コマンドはrecfsusb2n

+++
### PLEX PX-W3U4
https://www.amazon.co.jp/dp/B01MR4SLB6/

<img src="https://images-na.ssl-images-amazon.com/images/I/31u40Al-28L.jpg" height="300">

- インターフェース: USB
- 地上波 2ch、BS/CS 2ch
- 18000円くらい。
- ドライバがCentOS6用のバイナリのみしか提供されていない

+++

### B-CASカードリーダー
e-tax用、住基カード用として売ってる
2000円くらい
#### ACR39-NTTCom
https://www.amazon.co.jp/dp/B017Y8QV4O/

#### SCR3310
https://www.amazon.co.jp/dp/B0085H4YZC/

### B-CASカード
再発行手数料 1枚 2,050円
https://www.b-cas.co.jp/cardorder/view/order/agreement.html

+++

### ストレージ
録画する量に応じてたくさん。

- 1分で126MB
- 30分で3.7GB
- 1時間で7.4GB

---

## ソフトウェア
### ドライバ
### libarib25
### 録画コマンド
#### recpt1
#### revdvb
### epgdump
### TsSplitter
### ffmpeg
### assdumper

---
