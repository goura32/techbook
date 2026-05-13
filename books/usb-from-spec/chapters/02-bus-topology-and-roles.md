# 2章 USB 2.0 の基本モデル

USB を理解するとき、最初に固定したいのは `host 主導` という原則です。device は好きなタイミングで勝手に送信しません。host が列挙し、host が polling し、host が転送を進めます。この基本があるだけで、transfer、class、descriptor、driver の話がかなり整理しやすくなります。

## 2-1. host、hub、device、interface、endpoint

最低限分けたい単位は、host、hub、device、interface、endpoint です。host はバス全体を管理し、hub は接続点を増やし、device は機能を提供します。device の中では interface が機能の単位で、endpoint が実際の転送口です。

ここで大切なのは、`USB device がひとつだから機能もひとつ` ではないことです。複合デバイスでは複数 interface が同居し、OS や driver は interface ごとに別の機能として解釈します。ゲームコントローラー、マイク、タッチパッド、ドックのような製品ほど、この分離が重要になります。

## 2-2. host 主導という前提

USB の転送は host が開始します。interrupt transfer ですら、device が割り込みのように押し込むのではなく、host が周期的に見に来る契約です。ここを誤解すると、firmware 設計で「なぜ即時に飛べないのか」が見えにくくなります。

この前提は観測にも効きます。ログを見たときに、device が壊れているのか、host 側の scheduling が期待と違うのか、hub や cable 条件で遅れているのかを見分けるには、まず host 主導であることを忘れないほうがよいです。

## 2-3. port、address、configuration

USB device は挿した瞬間から最終形になるわけではありません。host は port で attach を検知し、reset をかけ、address を割り当て、configuration を選びます。つまり USB device は、`物理的に挿さっている状態` と `論理的に使える状態` の間に段階があります。この段階性が、列挙エラーや OS ごとの差分を理解する土台になります。

## 2-4. bus power と default power

電力の詳しい話は後の PD 章で扱いますが、USB 2.0 の段階でも `default power` と `構成後の消費` を分けて考える必要があります。列挙の途中では、まだ高機能モードや追加電力を前提にしてはいけません。だから USB device 設計では、`最低限どこまで default power で成立させるか` が最初の判断になります。

## 2-5. 何が物理層で、何が論理層か

同じ「認識しない」でも、原因は別の層にあります。たとえば cable 不良や connector の接触不良は物理層寄りです。descriptor の崩れや class 設計の不整合は論理層寄りです。PD contract の問題はさらに別軸です。USB の不具合報告はしばしば全部を `つながらない` でまとめてきますが、実装側では層を分けて聞き直す必要があります。

## 2-6. 直結、hub 経由、host 差分

実務で役立つ最初の切り分けは、次の 3 つです。

- 直結で再現するか
- hub や dock 経由でだけ再現するか
- host を変えると再現性が変わるか

この 3 点だけでも、問題が device 側に近いのか、bus 条件に近いのか、OS や host controller に近いのかをかなり絞れます。

この章の結論は、USB を `ケーブル 1 本` の話ではなく、`host / hub / device / interface / endpoint` の関係として見ることです。次の章では、この土台の上で列挙と USB 2.0 仕様のデバイスフレームワークを整理します。
