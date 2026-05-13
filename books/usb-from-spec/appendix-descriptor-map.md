# Appendix B. descriptor と class の対応

## この付録の役割

この付録は、本文で登場した descriptor と class の関係を、`何のために存在するか` で見直すためのものです。細かなフィールド値の暗記より、どの情報が host 側判断に効くかを整理します。

## 主な対応

### Device Descriptor

- 役割: デバイス全体の入口
- 何を見るか: vendor ID、product ID、基本的な class 設定
- `TraceDock` での意味:
  - まず host が識別できる最低限の名札
- host 側判断に効く点:
  - ここで vendor / product が安定して見えるかどうかで、attach 後の最初の足場が決まる
  - `bMaxPacketSize0` のような初期列挙に効く値は、問題が descriptor tree より前か後かを分ける

### Configuration Descriptor

- 役割: 全体構成
- 何を見るか: interface 数、電力値、属性
- `TraceDock` での意味:
  - HID control と bulk logging が分かれていることを伝える
- host 側判断に効く点:
  - interface 数が期待と違うだけで、class 問題より前に configuration 違いを疑える
  - bus powered 前提と電力値の整合も、ここで早めに確認できる

### Interface Descriptor

- 役割: 機能単位
- 何を見るか: class、subclass、protocol
- `TraceDock` での意味:
  - host 側ツールが「どの経路へ触るべきか」を見分ける手掛かり
- host 側判断に効く点:
  - HID control が見えるかどうかで、最低限の制御経路が残っているかを判断しやすい
  - vendor-specific を増やしすぎると、切り分けの起点が重くなる

### Endpoint Descriptor

- 役割: 転送方式
- 何を見るか: endpoint address、transfer type、max packet size
- `TraceDock` での意味:
  - 制御経路とログ経路の性質差を確定する
- host 側判断に効く点:
  - endpoint の transfer type が設計意図とずれていれば、driver より前に descriptor 側を疑える
  - max packet size は性能だけでなく、途中で詰まるかどうかの観測点にもなる

## `TraceDock` での対応

- HID control interface:
  - 短い command と status
- Bulk logging interface:
  - 大きめのログデータ
- 必要に応じた vendor-specific 拡張:
  - firmware update のような限定機能

## host 側の見方

- HID control が見える:
  - 最低限の制御経路は生きている
- bulk logging が見えない:
  - logging 側 interface / endpoint の問題を優先して疑える
- どちらも見えない:
  - enumeration か configuration tree の手前を優先して疑う

## よくある崩れ方

- device descriptor までは読めるが interface が足りない:
  - configuration tree の整合を優先して疑う
- HID だけ見えて bulk が見えない:
  - endpoint 定義か class / binding の差を見る
- OS ごとに見え方が違う:
  - descriptor の文法だけでなく、class の選び方と support 範囲を見直す
- cable や hub を変えると interface 数が変わる:
  - 世代差や power 条件が descriptor の手前へ影響していないかを確認する

## 最後に見るべきこと

descriptor は書類ではなく、host と device の契約です。field 単位の誤りが driver 問題や class 問題に見えることがあるため、構成全体の整合を見ることが大切です。
