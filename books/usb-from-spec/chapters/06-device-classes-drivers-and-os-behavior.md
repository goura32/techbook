# 6章 device class、driver、OS の見え方

USB device が `見える` ことと、`すぐ使える` ことは別です。OS は descriptor と class を見て driver binding を判断しますが、その結果は OS ごとに少しずつ違います。だから class の選択は protocol の選択であると同時に、support 方針の選択でもあります。

## 6-1. class / subclass / protocol

class は機能分類の入口です。subclass と protocol はその内訳です。HID、CDC、MSC のような標準 class は、host 側の期待値がすでにあるため、比較的扱いやすいです。一方 vendor-specific は自由度が高いぶん、host 側ライブラリや support の負担が増えます。

## 6-2. HID、CDC、MSC、vendor-specific

HID は入力や簡易制御に向きます。CDC はシリアル的なやり取りに自然です。MSC はストレージとして見せたいときに強いです。vendor-specific は設計自由度が高い一方で、`最初にどこへ触ればよいか` を読者や利用者へ別途説明しなければなりません。商用製品では、この違いが field support の負担へ直結します。

## 6-3. OS による見え方差

同じ descriptor でも、Windows、macOS、Linux で `見える` と `使える` は少しずつ違います。device manager や system report に現れることと、user-space から自然に開けることは別です。したがって class 選択は、列挙成功だけでなく、実際にどの API で触れるかまで含めて評価する必要があります。

## 6-4. generic HID gamepad を例にする価値

generic HID gamepad は、USB class の基本例として優秀です。button、axis、hat switch を report descriptor で表現し、OS は標準 HID として入力を取り込みます。だからこそ、本書では HID を `具体例として分かりやすい` 題材として使えます。一方、DualSense や Nintendo Switch 2 Pro Controller のような実機は、そこへ独自機能や複合機能が重なる例として読むのが自然です。

## 6-5. driver 問題と descriptor 問題

見えている問題が driver binding の失敗でも、原因は descriptor 設計かもしれません。逆に descriptor が自然でも、host 側ライブラリの選び方が悪いこともあります。だから次の順で確認すると役立ちます。

1. device が見えているか
2. interface 数が想定通りか
3. class / subclass / protocol が自然か
4. 目的の interface を user-space から開けるか
5. 開いた後の transfer が設計通りか

## 6-6. support コストとしての class 選択

標準 class を選ぶ価値は、仕様適合だけではありません。`最低限つながって見えるライン` を作りやすいことです。vendor-specific を選ぶなら、そのぶん `どのツールで、どの interface を、どう観測するか` を明示する必要があります。ゲームコントローラーのように読者が親しみやすい題材でも、実際には `標準 HID で説明できる部分` と `独自 report や拡張でしか説明できない部分` が分かれています。

## 6-7. この章のまとめ

class は device 側の都合ではなく、host 側の見え方と support 方針を決める重要な契約です。ここを理解しておくと、後の Type-C、PD、USB4 の話でも `何が物理の問題で、何が host 側 UX の問題か` を混ぜにくくなります。次の章では、USB 3.2 と高速側で追加される論点を整理します。
