# 3章 列挙と USB 2.0 仕様のデバイスフレームワーク

この章で扱う `USB 2.0 仕様のデバイスフレームワーク` は、規格書上ではしばしば `USB 2.0 規格書第9章` と呼ばれる部分です。ここには standard request、device state、descriptor 読み出し、address と configuration の基本が入っています。USB の列挙を理解するなら、まずこの枠組みを押さえるのが近道です。

## 3-1. `USB 2.0 規格書第9章` とは何か

ここでいう `USB 2.0 規格書第9章` は本書の 9章ではなく、USB 2.0 規格書の一章です。中身は `USB device framework`、つまり `host が device をどう認識し、standard request でどう会話するか` です。読者が知るべきなのは章番号そのものではなく、`列挙の基本契約はここに書かれている` という事実です。

## 3-2. attach から set configuration まで

典型的な流れは次のとおりです。

1. attach を検知する
2. bus reset をかける
3. device descriptor の先頭を読む
4. address を割り当てる
5. descriptor 全体と configuration tree を読む
6. set configuration で使用構成を決める

この流れは階段状です。前段が崩れると後段は一切見えません。だから `OS で class が見えない` 問題でも、実はもっと手前の reset や descriptor 読み出しで止まっていることがあります。

## 3-3. setup / data / status stage

control transfer の基本形は `setup` `data` `status` の 3 段階です。setup で host が何を欲しているかを宣言し、data で実データが流れ、status で要求が閉じます。この順を理解していると、Wireshark や analyzer のログで `どこまで進んだか` をすぐ判断できます。

たとえば `SETUP までは見えるが DATA が崩れる` なら、request の意味の理解不足というより、応答長や初期 packet size の前提を疑うほうが近道です。逆に status まで閉じるのに class 初期化へ進まないなら、descriptor や host 側 binding を優先して見るべきです。

## 3-4. どこで止まると何を疑うか

列挙停止の見方はある程度パターン化できます。

- reset 前後で反応しない
  power、pull-up、port、cable、PHY を疑う
- device descriptor の途中で崩れる
  応答長、`bMaxPacketSize0`、control transfer 実装を疑う
- configuration tree 取得後に進まない
  descriptor 整合性、interface / endpoint の定義を疑う
- set configuration 後にだけ問題が出る
  class 初期化、endpoint enable、driver binding を疑う

USB の debug が機械的に進めやすいのは、このように停止位置と疑う層を結びつけられるからです。

## 3-5. 観測で見るべき最小単位

この章で最低限見られるようになりたいのは、`GET_DESCRIPTOR` `SET_ADDRESS` `SET_CONFIGURATION` の流れです。packet を全部暗記する必要はありませんが、

- どの request が出たか
- 期待した長さの data が返ったか
- status まで閉じたか

の 3 点が見えるだけで、かなり多くの問題を絞り込めます。

## 3-6. generic HID gamepad を例にすると

generic HID gamepad でも、この章の流れは変わりません。host はまず device を認識し、configuration tree から `HID interface` と `interrupt endpoint` を見つけ、そこから report を読む準備へ入ります。つまり `ゲームコントローラーが OS に見えない` ときも、最初に見るべきは HID 独自の話ではなく、この device framework の段階です。

## 3-7. この章のまとめ

列挙は `認識したら終わり` の儀式ではありません。host と device が最初に契約を結ぶ場面です。どこで止まったかを見られるようになるだけで、USB の問題はかなり分解しやすくなります。次の章では、その契約書そのものである descriptor を、host の判断材料として読み解きます。
