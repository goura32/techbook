# 後付

## おわりに

USB の難しさは、仕様が多いことだけではありません。コネクタ、速度、電力、列挙、転送、クラス、OS の見え方が、ひとつのケーブルの中で同時に起きていることにあります。だからこそ、USB を理解するには、全部を暗記することより、どの問題をどの層で切り分けるべきかを掴むことのほうが重要です。

本書では、USB 2.0、USB 3.2、USB Type-C、USB Power Delivery、USB4 を、規格の歴史順ではなく、実装と解析の観点でつないできました。途中では generic HID gamepad やゲームコントローラーのような身近な題材も使いましたが、主役はあくまで規格そのものと観測のしかたです。

USB は、知っているつもりの部分ほど誤解が残りやすい技術です。Type-C と USB PD を同じ話だと思ってしまうこと、driver 問題を cable 問題と混同すること、enumeration の失敗を firmware だけの問題だと決めつけること。そうした混線を減らせるだけでも、実務の負担はかなり小さくなります。

ここで扱った考え方が、今後ほかの規格やファイル形式を読むときにも、役割、層、観測点を分けて考えるための土台として残ればうれしく思います。USB を理解する価値は、周辺機器が増えることではなく、複雑な接続のどこで判断を誤りやすいかを見抜けることにあります。

## 参考情報

### USB-IF 公式情報

- USB-IF Document Library
  種別: 公式ドキュメントライブラリ
  URL: `https://www.usb.org/documents`
- USB 3.2
  種別: 公式技術ページ
  URL: `https://www.usb.org/usb-32`
- USB Type-C® Cable and Connector Specification
  種別: 公式技術ページ
  URL: `https://www.usb.org/usb-type-cr-cable-and-connector-specification`
- USB Charger (USB Power Delivery)
  種別: 公式技術ページ
  URL: `https://www.usb.org/usb-charger-pd`
- DisplayPort over USB-C
  種別: 公式技術ページ
  URL: `https://www.displayport.org/displayport-over-usb-c/`
- usbmon — The Linux Kernel documentation
  種別: 公式ドキュメント
  URL: `https://docs.kernel.org/usb/usbmon.html`
- Wireshark USB capture setup
  種別: 公式ドキュメント
  URL: `https://wiki.wireshark.org/CaptureSetup/USB`

### 本書で特に参照した仕様と試験情報

- USB 2.0 Specification
  種別: 公式仕様
  日付: 2025年6月3日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索
- USB Type-C® Cable and Connector Specification Release 2.5
  種別: 公式仕様
  日付: 2026年4月8日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索
- USB Power Delivery Revision 3.2 Version 1.2
  種別: 公式仕様
  日付: 2026年3月24日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索
- USB4 Specification Version 2.0
  種別: 公式仕様
  日付: 2026年4月2日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索
- xHCI Interoperability Test Procedures For Peripherals, Hubs and Hosts
  種別: 公式試験仕様
  日付: 2025年6月3日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索
- USB 2.0 Electrical Compliance Test Specification
  種別: 公式試験仕様
  日付: 2026年4月21日
  URL: `https://www.usb.org/documents`
  補足: USB-IF Document Library で資料名を検索

## 著者紹介

大島 のりあ。ソフトウェア開発の現場で、言語やツールそのものの機能よりも、「どこで壊れやすく、どこで判断を誤りやすいか」を言葉にすることに強い関心を持つ技術書執筆者。アプリケーション実装に加えて、CLI、開発基盤、技術文書、モノレポ運用まで含め、変化の多いコードベースを長く保守できる形へ整えることを重視している。

シリーズ `技術の輪郭` では、文法の紹介や手順の列挙で終わらず、言語、OSS、規格、ファイル形式のそれぞれについて、「全体像をつかみ、実務で判断できるようになること」を目指している。特に、境界設計、公開 API、変更管理、更新に強いツールチェーン運用のような、技術が変わっても残り続ける論点を扱うことを大切にしている。

## 奥付

- 書名: `技術の輪郭 USB`
- サブタイトル: `規格、Type-C、PD、USB4、観測までを実務でつなぐ`
- シリーズ名: `技術の輪郭`
- 著者名: `大島 のりあ`
- 発行日: 2026年5月13日（仮）
- バージョン: 初版
- 発行形態: Kindle Direct Publishing による電子書籍
- 連絡先または案内先: `noria.library@gmail.com`
