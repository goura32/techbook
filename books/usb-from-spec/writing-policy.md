# Writing Policy

## この書籍で重視すること

- 規格書を `実装判断` `観測手法` `故障切り分け` へ変換する視点
- marketing 名称より role、layer、failure mode、discovery sequence の理解
- USB 2.0、USB 3.2、USB Type-C、USB PD、USB4、Alt Mode、Thunderbolt 互換の境界を混線させないこと
- ソフトウェアだけでなく、CC、VBUS、VCONN、抵抗モデル、PD controller のような最低限のハード前提も扱うこと
- 読者が `明日どこを見ればよいか` を持ち帰れること

## 本文を書く前の確認

1. `shared/style-guide.md` を確認する
2. `shared/terminology.md` を確認する
3. `shared/common-explanations.md` を確認する
4. 他書の `outline.md` と `status.md` を確認する
5. 規格や改訂日が出る箇所は `USB-IF` `VESA` `Wireshark` `Linux kernel docs` などの一次情報で確認する
6. `Type-C` `PD` `USB4` `Alt Mode` `Thunderbolt 互換` の説明が混線していないか確認する

## この書籍固有のルール

- 特定の架空デバイスは主役にしない。必要ならケーススタディか付録へ下げる
- 仕様書の全文要約はせず、実装と解析で先に押さえるべき箇所へ絞る
- chapter 冒頭で時点を固定するより、後付の参考情報で更新点を受け止める
- 章構成は `基本概念` `仕様上の要点` `よくある誤解` `観測点` `実務判断` を意識して組む
- 図表は概念図だけで終わらせず、`比較表` `切り分け表` `配線モデル` `観測手順` を含める
- ハード詳細は必要な範囲へ留めるが、`CC1/CC2` `Rp/Rd/Ra` `VBUS/VCONN` `PD controller status` は省略しない
- 観測手法は `usbmon` `Wireshark` `USBPcap` `analyzer` の違いまで入れる
- `Chapter 9` のような規格書内の章番号は、そのままではなく意味が伝わる日本語も併記する
- HID は USB の具体例として扱うが、用途本にならないよう `report descriptor` `OS の見え方` `切り分け` に絞る
