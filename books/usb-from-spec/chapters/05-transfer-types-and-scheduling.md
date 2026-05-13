# 5章 transfer type と scheduling

USB の transfer type は、control、bulk、interrupt、isochronous の 4 つです。名前だけ覚えるのは簡単ですが、実務では `何をどの契約へ載せるか` が重要です。ここで言う契約とは、帯域、遅延、再送、完全性、host 側の扱いやすさまで含んだ性質です。

## 5-1. 4種類の契約差

control transfer は列挙と設定の基礎で、必須の汎用経路です。bulk transfer は完全性を重視し、大きなデータを欠損なく流したい場面に向きます。interrupt transfer は短い通知を定期的に受けるのに向いています。isochronous transfer は時間優先で、再送しない代わりに遅延を抑えたい音声や映像に向きます。

ここで重要なのは、これらが性能の上下関係ではないことです。`bulk のほうが上` `interrupt は速い` のように見るのではなく、`どの失敗を許容し、どの性質を優先するか` の違いとして理解したほうが役立ちます。

## 5-2. scheduling と host 主導

USB は host 主導なので、device 側から見る `通知したい` は、そのまま `送れる` ではありません。interrupt transfer も host が周期的に見に来る契約です。ここを誤解すると、device 側で過剰にリアルタイム性を期待したり、host 側で遅延の理由を別の層へ押し付けたりしやすくなります。

## 5-3. 何をどの transfer に載せるか

たとえば generic HID gamepad は、button や axis の入力を interrupt transfer で届けるのが自然です。一方、設定変更や feature report は control や HID の別経路で扱うほうが分かりやすいことがあります。大量ログや firmware update のような大きなデータを同じ interrupt pipe へ押し込むと、host 側の扱いが一気に重くなります。

## 5-4. bulk は遅いが壊れていない

bulk transfer は完全性を優先するため、遅いこと自体は直ちに異常ではありません。大事なのは `遅延` と `欠損` を分けて考えることです。queue が詰まっても再送しながら成立しているなら、設計判断としては許容範囲かもしれません。逆に遅延が問題になるなら、最初から別の transfer 契約を選ぶべきです。

## 5-5. debug にどう効くか

transfer type が明確なら、観測の起点も明確になります。bulk で詰まっているのか、interrupt の周期が期待と違うのか、control request すら通っていないのか。この切り分けが早いほど、USB の debug は軽くなります。全部を vendor-specific のひと塊で扱うと、観測点が減り、原因の層も混ざります。

## 5-6. ゲームコントローラーを例にすると

generic HID gamepad では、`入力イベントは interrupt` という設計が自然です。もし同じ device にファームウェア更新や高頻度 telemetry のような別用途を追加するなら、同じ経路へ全部を押し込むのではなく、別 interface や別契約へ分けたほうが保守しやすくなります。ここでも USB の基本は `単機能のきれいさ` ではなく、`責務分離のしやすさ` です。

## 5-7. この章のまとめ

transfer type は性能表ではなく契約です。何を優先するかを先に決め、その契約に合う transfer を選ぶべきです。次の章では、class と driver の関係を OS の見え方まで含めて整理します。
