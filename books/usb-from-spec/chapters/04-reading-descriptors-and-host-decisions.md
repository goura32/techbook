# 4章 descriptor の読み方と host の判断

descriptor は説明文ではありません。host が `何がつながったのか` `どう扱うべきか` `どの endpoint を開くべきか` を決めるための契約書です。OS や driver の問題に見えるものの多くが、実際には descriptor の設計や整合性へ行き着きます。

## 4-1. 4つの中心 descriptor

この章でまず押さえたいのは、device、configuration、interface、endpoint descriptor の役割です。

- device descriptor: device 全体の入口
- configuration descriptor: 全体構成と属性
- interface descriptor: 機能単位
- endpoint descriptor: 転送口と transfer type

実装者にとって大事なのは、これらを field 一覧として暗記することではなく、`host がどの順に何を判断するか` を理解することです。

## 4-2. class をどこへ置くか

誤りやすい判断のひとつが、class を `device 全体` に置くか `interface ごと` に置くかです。単機能の device なら device descriptor 側へ意味を置けることもありますが、複数機能を持つなら interface ごとに意味を持たせるほうが自然です。ゲームコントローラーのように HID と別機能が混ざる可能性のある製品では、この判断がそのまま host 側 UX に効きます。

## 4-3. 電力値、interface 数、endpoint 定義

configuration descriptor の `MaxPower` や interface 数は、見た目以上に重要です。interface 数が期待と違うだけで class 問題より前に configuration 違いを疑えますし、endpoint 定義が transfer 実装とずれていれば driver より前に descriptor 側を見るべきです。descriptor は短いので軽く見られますが、最も密度の高い契約です。

## 4-4. descriptor dump をどう読むか

descriptor dump を読むときは、行を順に追うより、次の順で確認すると効率がよいです。

1. device が安定して見えているか
2. configuration 数と interface 数が想定通りか
3. class / subclass / protocol が自然か
4. endpoint type と最大 packet size が設計意図と合うか

この順番で見れば、`OS に見えるが使えない` 問題のかなりの部分を、driver の前に切り分けられます。

## 4-5. generic HID gamepad の descriptor で見るべきこと

generic HID gamepad なら、host がまず知りたいのは `HID interface があること` と `interrupt endpoint で input report を受けること` です。さらに report descriptor を読めば、button、axis、hat switch がどう表現されているかが見えてきます。ここで report descriptor が不自然だと、OS に見えても入力解釈が崩れます。

## 4-6. よくある崩れ方

- interface 数が足りない
  configuration tree の整合を疑う
- endpoint type が想定と違う
  descriptor 定義と firmware 実装のズレを疑う
- class が OS ごとに違って見える
  descriptor 文法だけでなく class 設計を見直す
- string descriptor だけ読めない
  文字列処理より前に control transfer 実装を見る

## 4-7. この章のまとめ

descriptor は `OS が勝手に何とかしてくれる情報` ではなく、host と device の契約書です。ここを読めるようになるだけで、USB の問題を `見える / 見えない` の二択から、一段深く分解できます。次の章では、endpoint が実際にどんな契約で動くかを、transfer type と scheduling の観点で整理します。
