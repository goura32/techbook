# 9章 USB Power Delivery と PD コントローラの実務

USB Power Delivery は、Type-C とよく一緒に語られますが、同じものではありません。Type-C が connector と attach の土台なら、PD は `どの電力契約を結ぶか` を扱う仕様です。この違いを分けて理解できるかどうかで、現代の USB 製品の説明力は大きく変わります。

## 9-1. Type-C と PD の違い

Type-C で接続されていても、PD contract が必ず成立しているとは限りません。逆に PD がなくても USB device として列挙することはあります。だから `挿したが高機能モードへ入らない` 現象を説明するとき、まず `attach 成功` と `contract 成功` を別イベントとして見る必要があります。

## 9-2. source capabilities、sink capabilities、PDO

PD では source と sink が能力情報をやり取りし、どの電圧・電流条件で動くかを交渉します。読者がまず知りたいのは、`PDO とは何か` と `どこが折り合えば explicit contract になるのか` です。詳細な bit 配列を全部覚える必要はありませんが、`どの条件を要求し、どの条件が拒否されると高機能モードへ入れないのか` を理解する価値は大きいです。

実務では、`要求した PDO が source 側に存在しない` `contract は成立したが期待電力に届かない` `cable 条件のせいで高電力を使えない` といった形で問題が出ます。つまり PDO は `仕様書の表` ではなく、`高機能モードへ入れるかどうかの境界` として理解したほうが役立ちます。

## 9-3. explicit contract と role swap

attach だけでは高機能動作の前提にならないことがあります。explicit contract が成立して初めて、高電力モードや追加機能が開く設計は珍しくありません。さらに role swap が絡むと、`電力側での役割` と `データ側での役割` が別々に動くこともあります。ここを混ぜないことが、PD の実務で最も重要です。

## 9-4. EPR と高電力化

近年の PD は高電力化が進んでいますが、本書で大事なのは最大値そのものより、`高電力を前提にしたとき設計と観測がどう変わるか` です。高負荷時だけ不安定、特定 cable でだけ落ちる、charger を変えると結果が変わる、といった症状は、EPR を含む電力前提を見ないと説明しづらくなります。

## 9-5. PD controller / port controller / PMIC

製品設計では、MCU が全部を直接処理するとは限りません。Type-C port controller、PD controller、PMIC のように役割が分かれることがあります。本書では、`どの chip が attach、contract、fault、power path を見ているのか` を分けて考えることを勧めます。これだけで、`firmware の問題か` `PD controller の状態か` `電源系の問題か` がかなり整理しやすくなります。

## 9-6. PD チップが返す情報

実務で役立つのは、PD controller が持っている status、fault、capability 情報です。たとえば、

- attach したか
- source / sink のどちらか
- どの PDO で contract したか
- cable identity が読めたか
- hard reset や over-current の履歴があるか

といった情報が読めるだけで、`つながるのに期待機能へ入らない` 問題の切り分けはかなり速くなります。

ここで重要なのは、PD controller の register や status を `低レベルすぎる内部状態` と見ないことです。USB 機器の現場では、`attach はした` `PDO は見えた` `contract が拒否された` `fault が立った` という差だけでも、FW、power path、cable のどこへ戻るべきかが大きく変わります。

## 9-7. ケーススタディ: attach はするが期待機能へ入らない

この症状でよくあるのは、列挙や class の話ではなく、PD contract が成立していないケースです。device 自体は見えるが高負荷機能だけ無効、cable を変えると直る、charger を変えると結果が変わる。このような現象では、descriptor dump より先に `どの contract が取れたか` `どの fault が出たか` を見たほうが近道です。

## 9-8. この章のまとめ

PD は高電力のおまけではなく、現代の USB を理解する別軸の仕様です。Type-C と同時に出てきやすいですが、役割は違います。次の章では、USB4、Thunderbolt 互換、DisplayPort Alt Mode を、現代の Type-C 文脈の中で整理します。
