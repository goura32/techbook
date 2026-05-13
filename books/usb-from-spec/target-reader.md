# Target Reader

## 想定読者

- USB 接続デバイスを扱う組み込み・ファームウェア開発者
- USB デバイスを OS 側やアプリケーション側から制御、解析、デバッグしたい開発者
- USB Type-C、USB PD、USB4、DisplayPort Alt Mode、Thunderbolt 互換の関係を整理したい人
- USB-IF や関連仕様を読み始めたが、どこから押さえるべきか分からない人
- `Wireshark` `usbmon` `USBPcap` のような観測手法まで含めて実務へ戻したい人
- HID や USB 接続のゲームコントローラーを具体例に USB を理解したい人

## 前提知識

- 基本的なプログラミング経験
- バイナリ、シリアル通信、電圧や電流のごく基本的な概念
- OS 上でデバイスを扱った経験があると読みやすい
- 回路設計の専門知識は必須ではないが、抵抗、電源、信号線の基本があると理解しやすい

## 読了後の到達目標

- USB 2.0 の列挙、descriptor、transfer type を説明できる
- USB Type-C の CC、VBUS、VCONN、Rp / Rd / Ra の意味を説明できる
- USB PD の contract、role、PDO、controller の役割を整理できる
- USB 3.2、USB4、Thunderbolt 互換、DisplayPort Alt Mode の境界を説明できる
- USB の不具合を `物理` `電力` `列挙` `descriptor` `class` `OS` `高速側` のどこで切り分けるべきか判断できる
- usbmon、Wireshark、USBPcap、analyzer の使い分けを判断できる
- HID の report descriptor を見たときに、button、axis、hat switch の表現を大づかみに理解できる

## 読み進め方の目安

- まず USB 2.0 の土台を固めたい読者は 1章から5章を主軸に読む
- Type-C とハード前提を整理したい読者は 8章と9章を重点的に読む
- USB4、Thunderbolt、映像出力との関係が気になる読者は 10章を中心に、7章から順に読む
- 実装や解析が主目的なら 3章、4章、9章、11章、12章を主軸にすると実務へ戻しやすい
- 規格書の読み方を掴みたい読者は 1章、3章、8章、9章、11章を通して見ると判断軸が残りやすい

## 対象外

- 高速信号の SI/PI だけを深掘りする専門書
- 特定の MCU ベンダ、USB IP コア、PD コントローラに完全特化した実装本
- Thunderbolt 物理層や DisplayPort 規格だけを単独で深掘りする専門書
- 単なる製品レビューや買い方ガイド
