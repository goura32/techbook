# 10章 解析、テスト、コンプライアンス

USB の不具合に向き合うとき、つい「analyzer がないと何も分からない」と思いがちです。もちろん protocol analyzer は強力ですが、それだけが観測点ではありません。power 条件、descriptor dump、OS の列挙ログ、xHCI interop 文書、electrical compliance 文書。それぞれが別の層を見ています。だからまず必要なのは、どの観測点が何を教えてくれるのかを分けることです。

本書では debug の順番を、power / cable、enumeration、descriptor、class / driver、application の順で見ることを勧めます。いきなり firmware のバグだと決めつけると、原因の層を飛ばしやすいからです。特に Type-C と PD が絡む現代の USB では、「見えているのに足りない」現象の背景に power contract の問題があることもあります。

この順番を、`TraceDock` でありがちな症状へ落とすと次のようになります。

- まったく認識しない:
  - power、cable、port、pull-up / pull-down を先に見る
- device は見えるが interface が不自然:
  - descriptor tree と class 設定を疑う
- 制御はできるがログ取得が不安定:
  - bulk transfer 側と host ツールの queue 処理を疑う
- 高負荷モードだけ不安定:
  - PD 契約と power budget を疑う

この対応表が頭にあるだけで、「何から見るべきか」がかなり固定されます。USB debug で苦しいのは、情報が足りないこと以上に、見る順番がぶれることです。

xHCI の interoperability test は、host/device の組み合わせを現実の相互接続として見る助けになります。一方、electrical compliance は signal quality の観点です。protocol analyzer は request/response や packet の流れを見せてくれますが、signal quality そのものを代替するわけではありません。つまり、試験仕様と analyzer は競合する道具ではなく、見る層が違います。

商用レベルの評価では、この違いを早い段階で認識しておくことが重要です。interop test は「この host controller とこの hub 経由で現実に成立するか」を見るのに向いています。electrical compliance は PHY や signal integrity の問題を見に行くときに必要です。protocol analyzer は、enumeration や class-specific request がどう流れているかを追うのに強い。ひとつの道具で全部を見ることはできません。

descriptor dump も非常に重要です。class や interface が想定どおりに見えているか、power 値や endpoint が整合しているかは、capture 以前に見える情報です。ここでずれているなら、driver 問題より手前の可能性が高くなります。反対に descriptor が自然なら、次は transfer や host 側 binding を疑いやすくなります。

たとえば descriptor dump で次のようなズレがあれば、原因候補はかなり絞れます。

- interface 数が想定と違う:
  - configuration descriptor 長や parser 側を確認する
- endpoint type が期待と違う:
  - firmware 側 descriptor 定義と endpoint enable 処理を確認する
- power 値が過小 / 過大:
  - host 側挙動だけでなく設計前提そのものを見直す
- string descriptor が読めない:
  - 文字列処理より先に enumeration 実装を疑う

capture を読むときも、全部を同じ粒度で追う必要はありません。商用製品の debug では、まず「どこで止まったか」「どの request までは進んだか」を取れれば十分なことが多いです。細かな packet の全理解は強い武器ですが、毎回そこから始める必要はありません。

再現手順も USB debug では大切です。どの host で、どの cable で、どの port で、どの power condition で再現するのか。USB は環境差が大きいので、「たまに失敗する」だけでは情報が足りません。再現条件の切り出し自体が、debug の大きな部分を占めます。

再現条件の記録には、最低でも次の 5 点を残すと役に立ちます。

- host OS と host controller
- cable の種類
- 直結か hub 経由か
- bus power か、PD 契約があるか
- 失敗する操作手順

この 5 点が揃うだけで、「USB の問題」という巨大な箱が、かなり扱いやすい単位に分解されます。逆にここが欠けると、同じ不具合名でもまったく別の現象が混ざります。

付録 C には、descriptor dump、control request、bulk transfer、power negotiation の代表断片を置いています。短い例ですが、「この症状ならまずどの断片を見るか」を確認するには十分です。商用現場で大切なのは、毎回完璧な波形解析から始めることではなく、必要な観測点へ速く辿り着くことです。

この章の結論は、USB の debug は道具探しより切り分け順の固定が先だということです。最後の章では、こうした判断を長く保守できる USB 製品設計へどうつなぐかを整理します。
