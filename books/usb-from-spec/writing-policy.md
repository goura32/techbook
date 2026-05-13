# Writing Policy

## この書籍で重視すること

- 規格書を実装判断へ変換する視点
- marketing 名称より role、layer、failure mode の理解
- Type-C、PD、USB 3.2 を USB 全体像の中で整理すること
- 解析とデバッグでどこを見るかが残ること

## 本文を書く前の確認

1. `shared/style-guide.md` を確認する
2. `shared/terminology.md` を確認する
3. `shared/common-explanations.md` を確認する
4. 他書の `outline.md` と `status.md` を確認する
5. 規格や改訂日が出る箇所は USB-IF の公式資料で確認する
6. Type-C と PD の説明が重複しすぎていないか確認する

## この書籍固有のルール

- connector の見た目や marketing 用語より、host/device/power role の切り分けを優先する
- 仕様書の全文要約はせず、実装と解析で最初に押さえるべき箇所へ絞る
- electrical detail は必要な範囲に留め、論理層、列挙、電力、driver との接続を重視する
- 最新状況は chapter 冒頭で時点を明示し、本文では改訂点と採用判断へ絞る
- USB4 は存在を無視しないが、本書の中心は USB 2.0、USB 3.2、Type-C、PD に置く
