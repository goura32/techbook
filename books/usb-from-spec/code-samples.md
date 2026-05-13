# Code Samples

## 目的

- 本文中のコード、descriptor 断片、capture 断片の掲載方針を固定する
- 実装例を入れすぎて本文の論点が散らないようにする

## 方針

- 本文では短い descriptor 断片、transfer 例、host 側コード断片だけを載せる
- ベンダ固有の長い実装例は付録へ寄せ、本文は判断軸の説明を優先する
- firmware と host 側コードは、どちらか一方だけで完結したように見せない
- bus capture は 1 パケット単位より、何を見るべきかが伝わる粒度にとどめる

## 本文へ置くもの

- device descriptor と configuration descriptor の短い例
- control transfer の setup packet 例
- HID report / bulk transfer の簡略例
- host 側で interface を選ぶ短いコード断片

## 付録へ置くもの

- descriptor dump の行ごとの読み方
- generic HID gamepad の report descriptor 断片
- capture log の読み方
- firmware 更新や debug の長めの断片

## 避けること

- ベンダ依存 SDK の長いコード
- analyzer ツールの UI 手順の大量掲載
- 電気特性表の丸写し
