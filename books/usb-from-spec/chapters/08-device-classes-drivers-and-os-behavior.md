# 8章 デバイスクラス、ドライバ、OS の見え方

USB device は、単に「つながる」だけでは役に立ちません。host 側がそれをどう認識し、どう扱うかが重要です。ここで効いてくるのが device class と driver の関係です。device が自分をどう名乗るかで、OS 側の扱いや必要なソフトウェアが大きく変わります。

class、subclass、protocol は、その device や interface が何者かを host へ伝えるための重要な情報です。HID、CDC、MSC のような class は、それぞれ host 側での扱いやすさがかなり違います。たとえば HID は比較的 driver を用意しやすく、簡易制御に向いています。CDC はシリアル通信として自然で、MSC はストレージとして見せやすい。一方、vendor-specific は自由度が高いぶん、host 側の負担も増えます。

ここで大切なのは、「標準 class を使うこと」そのものが目的ではないということです。標準 class の強みは、host 側の期待値がすでに存在することにあります。つまり、device 側の自由度を少し減らす代わりに、driver や user-space の負担を減らせます。逆に vendor-specific は自由ですが、その自由は host 側の実装コストとして戻ってきます。

商用製品では、この選択が support コストに直結します。HID を選べば、少なくとも「最低限つながって見える」ラインを作りやすくなります。CDC は診断やログ取得で役に立つことが多いですが、OS や権限設定との相性を見る必要があります。MSC は扱いやすい一方で、device の責務を storage 的に見せることになります。class 選択は protocol の選択であると同時に、support 方針の選択でもあります。

`TraceDock` では、制御を HID に寄せ、ログ本体は bulk transfer を持つ別 interface に分けます。これは device 側の都合ではなく、host 側の見え方を整えるためでもあります。最小限の制御は既存の class で処理し、大きなデータは別責務へ逃がすと、実装も debug も素直になります。

この構成の利点は、異常時にも切り分けやすいことです。HID 制御が見えるなら、少なくとも enumeration と class 基本部は生きています。そのうえで bulk logging だけが失敗するなら、疑うべき層はかなり狭まります。もし全部をひとつの vendor-specific pipe へ押し込んでいたら、host 側では「何が最低限生きているのか」すら見えにくくなります。

OS 差分も忘れてはいけません。同じ descriptor でも、OS によって driver の当たり方や user-space からの触り方が違うことがあります。だから「Windows で動いたから正しい」「Linux で見えたから class 設計は十分」と早く結論づけないほうが安全です。OS 差分は仕様違反だけでなく、実装の期待値差でも起きます。

特に見落としやすいのは、OS ごとに「見える」と「すぐ使える」が違うことです。device manager や system information に見えることと、user-space から意図した interface を自然に開けることは別です。したがって、class 選択を評価するときは、列挙成功だけでなく、実際に host 側アプリケーションがどの API で触れるかまで含めて考える必要があります。

`TraceDock` の host 側ツールでも、この違いを意識します。通常運用で欲しいのは「まず安定して開ける制御経路」と「必要なときだけ重いログ経路へ入る構造」です。つまり class 選択は、device だけでなく host tool の責務分離にもつながっています。

この章で押さえたいのは、driver 問題と descriptor 問題を切り分けることです。見えている現象が driver のバインド失敗でも、その原因は descriptor の class 設定かもしれません。反対に、descriptor が正しくても、host 側ライブラリの選び方が悪いこともあります。問題の層を分けて見ることが大切です。

そのための見方として、まず次の順で確認すると役立ちます。

- OS から device が見えているか
- 想定した interface 数が見えているか
- class / subclass / protocol が想定どおりか
- user-space から開きたい interface へ到達できるか
- 到達後の transfer が設計どおりか

この順番で見ていけば、「OS 依存」に見える問題の中にも、descriptor 設計の不整合と host tool 側の設計不足が混ざっていることが見えやすくなります。

ここまで来ると、`TraceDock` の構成判断がなぜそうなっているかがかなり見えやすくなります。次の章では、その `TraceDock` 自体を実装視点で読み直し、descriptor、interface、power、host tool の責務を一度まとめます。
