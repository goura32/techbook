# 付録 C. USB Type-C / PD の最小ハード前提

## この付録の役割

本書は回路設計専用の本ではありませんが、Type-C と PD を理解するには、最低限のハード前提が必要です。この付録では、`CC1 / CC2` `VBUS` `VCONN` `SBU` `Rp / Rd / Ra` と、PD controller が返す状態情報を、実務に必要な範囲へ絞って整理します。

## C-1. CC1 / CC2、VBUS、VCONN、SBU

- `VBUS`: 主電力の供給線
- `CC1 / CC2`: attach 判定、role、cable orientation の入口
- `VCONN`: 一部 cable や active component への補助電力
- `SBU`: Alt Mode で意味を持つことがある補助信号

全部の pin 配列を暗記する必要はありませんが、`どの信号が接続と役割へ効くのか` は把握しておく価値があります。

## C-2. Rp / Rd / Ra の意味

Type-C では、CC に対する抵抗モデルが attach や role の判定へ効きます。

- `Rp`: source 側の広告
- `Rd`: sink 側の存在表明
- `Ra`: cable / accessory 系で出てくる補助的な前提

ここで大切なのは値の丸暗記より、`期待した role を広告しないと attach 自体が崩れる` という感覚です。`つながらない` 問題の中には、もっと手前の CC 前提違いが普通に含まれます。

## C-3. PD controller の status と fault

PD controller は、たとえば次のような情報を持つことがあります。

- attach したか
- source / sink のどちらか
- どの PDO で contract したか
- hard reset や over-current の履歴
- cable identity や e-marker 関連情報

こうした status が読めると、`列挙はしたが高機能モードに入らない` 問題を、firmware の前に power 契約として疑いやすくなります。

## C-4. 最初に確認したい配線前提

Type-C / PD まわりで最初に確認すると役立つのは次の点です。

- CC1 / CC2 が想定した controller へ入っているか
- VBUS の供給元と監視点が明確か
- VCONN が必要な cable を前提にしていないか
- PD controller と host MCU の責務が分かれているか

この 4 点だけでも、`なぜ attach しないのか` `なぜ高電力化しないのか` の初手はかなり安定します。
