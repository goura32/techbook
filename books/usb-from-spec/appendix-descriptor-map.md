# 付録 A. descriptor dump の読み方

## この付録の役割

本文 3章と4章では、列挙と descriptor を `契約` として読みました。この付録では、その契約を dump の形でどう読むかをまとめます。field を暗記するためではなく、`何を見ると次の判断へ進めるか` を整理するための付録です。

## A-1. Device Descriptor

まず見るのは device 全体の入口です。

```text
Device Descriptor:
  idVendor           0x1234
  idProduct          0x5678
  bcdUSB             0x0200
  bDeviceClass       0x00
  bNumConfigurations 0x01
  bMaxPacketSize0    0x40
```

ここで重要なのは、device が安定して見えているか、`bMaxPacketSize0` が列挙の初期往復に適した値か、class の意味を device 全体へ置いているかです。`bDeviceClass = 0x00` は interface ごとに class を持つ設計でよく見ます。

## A-2. Configuration Descriptor

次に全体構成を確認します。

```text
Configuration Descriptor:
  wTotalLength       0x0034
  bNumInterfaces     0x02
  bmAttributes       0x80
  MaxPower           0x32
```

ここでは interface 数、全長、属性、電力値を見ます。interface 数が想定より少ないなら class 問題より前に configuration tree の整合を疑います。MaxPower は bus power 前提や support 説明にも効きます。

## A-3. Interface Descriptor

機能単位としての見え方はここで決まります。

```text
Interface Descriptor:
  bInterfaceNumber   0x00
  bNumEndpoints      0x01
  bInterfaceClass    0x03
  bInterfaceSubClass 0x00
  bInterfaceProtocol 0x00
```

HID gamepad のような例では、ここで `HID class` が見えます。複合デバイスなら interface ごとに class が分かれます。`OS には見えるが期待した機能だけ使えない` ときは、この層の設計を見直す価値があります。

## A-4. Endpoint Descriptor

どの契約で転送するかは endpoint に現れます。

```text
Endpoint Descriptor:
  bEndpointAddress   0x81
  bmAttributes       0x03
  wMaxPacketSize     0x0040
  bInterval          0x01
```

ここでは transfer type、方向、最大 packet size、polling 間隔を見ます。interrupt endpoint のつもりが bulk になっていれば、OS や driver より前に descriptor 定義の問題を疑うべきです。

## A-5. generic HID gamepad で見るポイント

ゲームコントローラーのような HID では、device descriptor より interface と report descriptor が重要です。少なくとも次の 3 点を見ると役立ちます。

- HID interface が独立して見えているか
- interrupt IN endpoint が自然か
- report descriptor が button、axis、hat switch を無理なく表現しているか

## A-6. よくある崩れ方

- device descriptor までは読めるが interface が足りない
  configuration tree を疑う
- HID だけ見えて入力が崩れる
  report descriptor と host 側解釈を疑う
- endpoint type が不自然
  firmware 実装より先に descriptor 定義を見る
- OS ごとに見え方が違う
  class 設計と support 範囲を見直す

## A-7. この付録の使い方

本文 3章で `どこまで列挙したか` を確認し、4章で `何を契約したか` を理解し、この付録で `実際の dump をどう読むか` へ戻る使い方が自然です。
