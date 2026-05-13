# 付録 B. 観測ログの読み方

## この付録の役割

本文 11章では、`usbmon` `Wireshark` `USBPcap` `descriptor dump` `OS 標準ツール` を観測点として整理しました。この付録では、実際にどんな断片を見て、どこまで判断すればよいかを短い例で確認します。

## B-1. setup / data / status stage

```text
SETUP  bmRequestType=0x80 bRequest=GET_DESCRIPTOR
DATA   12 01 00 02 ...
STATUS ACK
```

最初に見たいのは、`request が出たか` `data が返ったか` `status まで閉じたか` です。全部の byte を暗記しなくても、この 3 点が見えれば列挙のどこで止まったかをかなり説明できます。

## B-2. descriptor dump と組み合わせる

```text
lsusb: HID interface present
descriptor dump: bNumInterfaces = 1
```

OS の一覧と descriptor dump を並べるだけでも、`OS には見えているが class 解釈が崩れている` のか、`そもそも interface 数が違う` のかを分けやすくなります。

## B-3. bulk transfer の詰まり

```text
OUT bulk queue full
retry count increased
host timeout threshold reached
```

bulk は完全性優先なので、遅いこと自体は直ちに異常ではありません。遅延と欠損を分け、queue が device 側で詰まっているか host 側で詰まっているかを見ます。

## B-4. PD contract の不成立

```text
Type-C attached
Default USB power available
PD contract not established
Requested power profile denied
```

attach 成功と contract 成功は別です。`高機能モードだけ無効` のような症状では、enumeration を全部疑う前に power contract を見たほうが近道です。

## B-5. generic HID gamepad の観測

```text
Interface 0: HID Gamepad
Endpoint 1 IN: Interrupt
Input Report: buttons, X/Y axes, hat switch
```

ゲームコントローラーのような題材では、descriptor dump、HID report、OS の入力一覧を並べると、`列挙` `class` `report 解釈` がどこで崩れているかをかなり説明しやすくなります。

## B-6. 観測の順番を固定する

USB の debug で大切なのは、最初に見る順番を固定することです。

1. power / cable
2. 列挙
3. descriptor
4. class / OS の見え方
5. application logic

この順だけでも、無駄な調査はかなり減ります。
