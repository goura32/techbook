# Appendix A. TraceDock の全体構成

## この付録の役割

この付録は、本文中で断片的に登場する `TraceDock` の全体像をまとめて参照するためのものです。各章では descriptor、transfer、Type-C、PD、debug の断片だけを見ますが、それだけだと device 全体の責務が見えにくくなります。

## `TraceDock` の前提

`TraceDock` は USB Type-C 接続の小型測定デバイスです。意図的に次の要素を持ちます。

- USB Type-C receptacle
- bus powered の基本動作
- 条件付きで USB PD による追加電力要求
- HID による制御
- bulk transfer によるログ取得

## 想定構成

```text
TraceDock
  usb/
    device_descriptor
    configuration_descriptor
    hid_interface
    bulk_logging_interface
  power/
    default_5v_path
    pd_policy
  host_tool/
    enumerate
    capture
    firmware_update
```

## 各部分の責務

### USB device core

- descriptor を返す
- control request に応答する
- interface と endpoint を公開する

### HID control

- 設定読み出し
- 動作モード切り替え
- 簡易な状態取得

### Bulk logging

- 大きめのログデータを転送する
- 完全性を優先する

### Host tool

- device の識別
- control interface の操作
- bulk logging の取得
- debug 用 dump の出力

host tool を別責務として切り出しておくのは、field support のためでもあります。device firmware の変更とは独立に、列挙確認、descriptor dump、簡易 self-check、power 条件の記録を書き出せるようにしておくと、解析の初手がかなり軽くなります。

### Power path

- 通常時は既定の USB 電力で動く
- 追加機能が必要なときだけ USB PD 契約を使う

## 運用前提

- Type-C を採用しても、必ずしも高速度や高電力を意味しない
- power role と data role は分けて考える
- class 選択は host 側の使いやすさに直結する
- analyzer がなくても descriptor dump と電力条件の観測だけで分かることは多い
- control 経路と logging 経路を分けると field debug がかなり楽になる

## 最低限残したい運用情報

- 使用した cable の種類
- attach は成功したか
- PD contract は成立したか
- host 側で見えた interface 一覧
- logging 経路だけ失敗していないか

この 5 点が毎回そろうだけでも、「USB が全部だめなのか」「高機能側だけだめなのか」をかなり短時間で分けられます。

## 付録 B / C との関係

付録 A は `TraceDock` の構造全体を見るためのものです。付録 B では descriptor と class の対応を、付録 C では capture や debug の代表断片を確認できます。
