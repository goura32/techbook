# 11章 観測手法、試験、コンプライアンス

USB の問題に向き合うとき、つい `analyzer がないと何も分からない` と思いがちです。もちろん protocol analyzer は強力ですが、それだけが観測点ではありません。Linux の `usbmon`、Wireshark、Windows の `USBPcap`、descriptor dump、OS 標準ツール、試験仕様書は、それぞれ別の層を見ています。まず必要なのは、何が何を教えてくれるのかを分けることです。

## 11-1. usbmon、Wireshark、USBPcap、tshark

ソフトウェア観測でまず役立つのは、Linux の `usbmon` と Wireshark、Windows の `USBPcap` です。これらは主に `どの request が流れ、どこで止まり、class traffic がどう見えているか` を知るのに向いています。全部を毎回追う必要はなく、最初は `どこまで進んだか` を見るだけでも十分価値があります。

たとえば Linux なら、まず `usbmon` で採取し、Wireshark で `GET_DESCRIPTOR` や class request を追い、必要なら `tshark` で再現条件ごとの差分を残す、という流れが取りやすいです。Windows でも `USBPcap` と Wireshark を組み合わせれば、少なくとも software 層でどこまで進んだかは見やすくなります。

## 11-2. descriptor dump と OS 標準ツール

protocol trace を取る前に、descriptor dump や OS の device 一覧だけで分かることは多いです。interface 数、class、endpoint type、power 値、string descriptor。ここで崩れているなら、driver 問題より前の話です。逆にここが自然なら、次は transfer や host 側 binding へ視点を移せます。

ここでの狙いは、`まず静的な契約を見る` ことです。たとえば HID gamepad が OS から期待どおりの入力機器として見えないなら、最初に見るべきは report が来ているかどうかより、descriptor と class の整合です。逆に descriptor が素直で、OS でも interface が妥当に見えているなら、その次に input report や class request を追えばよく、調査順がかなり安定します。

## 11-3. protocol analyzer が必要になる境界

ソフトウェア観測で足りる場面と、hardware analyzer が必要な場面は分けて考えます。`どの request が出たか` `descriptor がどう見えたか` はソフトウェア観測で十分なことが多いです。一方、signal quality や電気的条件、attach 直後の微妙な挙動、高速側だけの不安定は analyzer や electrical test の領域に入りやすくなります。

逆に言えば、毎回 hardware analyzer から始める必要はありません。software 観測で `何が起きていないか` を先に絞ってから hardware 側へ降りたほうが、時間もコストも抑えやすくなります。商用現場では、この順番が固定されているだけで説明責任がかなり上がります。

## 11-4. interop と electrical test

xHCI interop 文書は、現実の host / hub / device の組み合わせをどう見るかに向いています。electrical compliance は signal quality の世界です。両者は競合する道具ではなく、見る層が違います。だから `analyzer で packet が見えたから電気的には問題ない` とも、`electrical test が通ったから OS で自然に使える` とも言えません。

商用製品では、この違いをチーム内で共有しておくことが特に重要です。ソフトウェア側は packet が見えると安心しがちで、ハード側は electrical が通ると安心しがちですが、USB はその中間で壊れます。だから interop、electrical、OS 実機確認を `別の合格条件` として扱うほうが、後で責任分界を説明しやすくなります。

## 11-5. 再現条件を残す

USB の debug で大切なのは、道具探しより再現条件の固定です。少なくとも次の 5 点を残すとかなり助かります。

- host OS と host controller
- cable の種類
- 直結か hub / dock 経由か
- bus power か、PD contract があるか
- 失敗する操作手順

この 5 点が揃うだけで、同じ `つながらない` という報告の中に別現象が混ざっていることがかなり見えやすくなります。

可能なら、`直結での結果` `別 host での結果` `別 cable での結果` も 1 行ずつ残すとさらに強くなります。USB の不具合は一発で特定するより、条件を潰しながら狭めるほうが現実的です。そのため、観測ログだけでなく比較条件の記録自体が evidence になります。

## 11-6. ゲームコントローラーを観測するとき

generic HID gamepad のような題材は、観測手法の練習にも向いています。descriptor dump で HID interface を見て、Wireshark や HID ツールで input report を追えば、USB が `report descriptor` `interrupt transfer` `OS の入力層` までつながっていることが理解しやすくなります。複雑な vendor-specific device へ行く前に、この段階で観測の筋肉をつける価値があります。

## 11-7. この章のまとめ

USB の観測は、万能の道具を探すことではなく、層ごとに適切な観測点を持つことです。次の章では、ここまでの規格、観測、ハード前提を、実装・解析・長期保守の原則としてまとめ直します。
