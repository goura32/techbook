# Appendix C. capture、dump、debug の代表断片

## この付録の役割

付録 A は `TraceDock` の構造、付録 B は descriptor と class の対応を整理するためのものでした。この付録 C では、本文で断片的に登場した capture や debug の例を、もう少し連続した形で確認できるようにします。

## 1. device descriptor を読む

```text
Device Descriptor:
  idVendor           0x1209
  idProduct          0x0001
  bNumConfigurations 0x01
  bMaxPacketSize0    0x40
```

ここで大事なのは、まず device 全体が見えているかを確認することです。class が interface 側にある設計なら、device descriptor だけを見て結論を急がないほうが安全です。`bMaxPacketSize0` のような初期列挙に効く値は、最初の往復が成立するかどうかに直結します。

## 2. configuration tree を読む

```text
Configuration 1
  Interface 0: HID Control
  Interface 1: Bulk Logging
  Endpoint 1 IN: Interrupt
  Endpoint 2 IN: Bulk
```

interface の分け方は、そのまま host 側の責務分離につながります。endpoint の種類が設計意図と一致しているかも、ここで最初に確認します。

ここで見たいのは「機能があるか」だけではありません。最低限の制御経路と、重いログ経路が分かれているかを見ると、異常時にどこまで観測を続けられるかも判断できます。

## 3. control request の流れを見る

```text
SETUP bmRequestType=0x80 bRequest=GET_DESCRIPTOR
DATA  Descriptor bytes...
STATUS ACK
```

enumeration の問題は、この流れのどこで止まっているかを見るだけでもかなり切り分けが進みます。SETUP まで見えるのか、DATA が途中で崩れるのか、STATUS まで閉じるのかで、疑う層が変わります。

たとえば SETUP は見えるのに DATA が途中で崩れるなら、request の意味を理解していないというより、応答長や初期 packet size の前提を疑うほうが早いです。逆に STATUS まで閉じるのに class 初期化へ進まないなら、descriptor tree や host 側 binding の問題へ重心を移せます。

## 4. bulk transfer の詰まり方を見る

```text
OUT bulk queue full
retry count increased
host timeout threshold reached
```

bulk は完全性を優先するので、遅いこと自体が直ちに異常とは限りません。遅延と欠損を分けて考えます。queue 詰まりが firmware 側か host 側かを分けて見ることも重要です。

## 5. power negotiation の前提を確認する

```text
Type-C attached
Default USB power available
PD contract not established
Requested power profile denied
```

Type-C で接続されていても、PD 契約が必ず成立しているとは限りません。attach 成功と contract 成功を別イベントとして見ると、切り分けがかなり楽になります。

ここで高機能モードだけが無効なら、enumeration 全体を疑う前に power budget を見たほうが近道です。商用製品では「動かない」ではなく、「最低限は動くが拡張だけ落ちる」という症状がよくあるためです。

## 6. host 側の見え方をそろえる

```text
lsusb: two interfaces listed
device manager: HID present
vendor tool: logging endpoint unavailable
```

同じ device でも、host 側の見え方は道具ごとに違います。だからこそ、descriptor dump、OS の一覧、専用ツールの表示を並べて読むと、「enumeration は成功しているが logging 経路だけ死んでいる」のような状態を説明しやすくなります。

## 7. debug の順番を固定する

重要なのは、いきなり firmware バグだと決めつけないことです。power、cable、enumeration、descriptor、class、application の順に切り分けるだけでも、無駄な調査は大きく減ります。商用現場では「どの証拠を見て次にどこへ進むか」が固定されているだけで、再現性と説明責任が大きく上がります。
