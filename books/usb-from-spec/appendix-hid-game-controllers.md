# 付録 D. HID と USB ゲームコントローラー

## この付録の役割

この付録は、USB HID を具体的な題材で理解するための補助資料です。本文では HID を device class の代表例として扱いますが、ここでは USB 接続のゲームコントローラーを例に、`report descriptor`、`button`、`axis`、`hat switch`、OS からの見え方をまとめて確認します。

本付録の主役は、特定メーカーの独自機能ではなく、`generic HID gamepad` としてどこまで説明できるかです。そのうえで、`DualSense` や `Nintendo Switch 2 Pro Controller` のような代表的な製品は、`複合機能を持つ実例` として比較対象に置きます。

## D-1. generic HID gamepad の基本
generic HID gamepad は、USB HID を理解するための良い題材です。device class としては HID、転送としては interrupt IN、内容としては button、axis、hat switch の report を持つことが多く、USB の基本が比較的素直に見えます。

ここで大切なのは、`ゲームパッドだから特別` ではなく、`HID の代表例として分かりやすい` ことです。入力 report、feature report、report descriptor の関係を理解する入口として使えます。

## D-2. report descriptor の考え方

report descriptor は、button や axis をどんな usage で、どんな bit 幅で、どう並べるかを host へ伝える契約です。読者がまず押さえたいのは、`button` `X/Y axis` `hat switch` のような入力が、report の中でどう表現されるかです。

generic HID gamepad では、次のような要素がよく現れます。

- button 群
- X / Y 軸
- trigger のような追加軸
- hat switch
- 振動や LED などの output / feature report

ごく単純化した断片なら、たとえば次のようなイメージです。

```text
Usage Page (Generic Desktop)
Usage (Game Pad)
Collection (Application)
  Usage Page (Button)
  Usage Minimum (1)
  Usage Maximum (12)
  Logical Minimum (0)
  Logical Maximum (1)
  Report Count (12)
  Report Size (1)
  Input (Data,Var,Abs)
  Usage Page (Generic Desktop)
  Usage (X)
  Usage (Y)
  Usage (Hat Switch)
  ...
End Collection
```

ここで見たいのは byte 列そのものより、`button 群` と `軸` と `hat switch` が別の usage と bit 幅で記述されることです。OS はこの契約を読んで、どの入力をどう意味づけるかを決めます。

## D-3. OS からどう見えるか

generic HID に収まる範囲なら、多くの OS は標準 HID として扱いやすくなります。ただし `見える` と `ゲームごとに自然に解釈される` は別です。OS の入力層、ゲーム API、マッピングレイヤで扱いが変わることがあります。

## D-4. `DualSense` をどう読むか

PlayStation 公式情報では、`DualSense` は USB Type-C または Bluetooth で PC やモバイル機器へ接続でき、機能によっては USB 接続が必要と説明されています。つまり、HID の基本例としては useful ですが、触覚や adaptive trigger のような複合機能は generic HID の枠だけでは語り切れません。

本書では `DualSense` を、`標準 HID の上に複合機能が重なる代表例` として扱います。本文の主役にはせず、`どこから先が vendor 固有の話になるか` を見る比較対象として置きます。

## D-5. `Nintendo Switch 2 Pro Controller` をどう読むか

Nintendo 公式サイトには `Nintendo Switch 2 Pro Controller` の製品情報があります。読者関心は高いですが、本文の主役にすると vendor 固有の話へ寄りすぎます。そこで本書では、`generic HID として見える部分` と `任天堂固有の複合機能や無線機能` を分けて読む対象とします。

## D-6. よくある不具合と切り分け

- button は取れるが axis が崩れる
  report descriptor と host 側解釈を疑う
- OS により見え方が違う
  HID 自体より API 層の違いを見る
- USB 接続時と Bluetooth 接続時で挙動が違う
  transport 差を前提に切り分ける
- 複合機能だけ使えない
  generic HID の外側を疑う

## 本文との関係

- 6章 `device class、driver、OS の見え方` の具体例
- 11章 `観測手法、試験、コンプライアンス` での観測対象
- 付録 A `descriptor dump の読み方` と合わせて参照する想定
