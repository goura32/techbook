# 8章 USB Type-C の配線、CC、役割、ケーブル

USB Type-C は、単なる小さなコネクタではありません。向きの自動判定、電力交渉の入口、Alt Mode の足場、USB4 の入口、e-marker cable のような追加要素まで含んだ `接続の仕組み` です。だから Type-C を正しく理解するには、形だけでなく信号線と役割の意味まで押さえる必要があります。

## 8-1. Type-C の信号群

ここで最低限押さえたいのは、`VBUS` `GND` `CC1 / CC2` `VCONN` `SBU` と、USB 2.0 / 高速差動対の存在です。すべての pin 配列を暗記する必要はありませんが、`どの信号が接続判定と役割決定に効くのか` は知っておく価値があります。

## 8-2. CC1 / CC2、VBUS、VCONN

CC1 / CC2 は attach 判定や role の入口です。VBUS は主電力の供給線、VCONN は一部 cable や active component へ補助的に使われます。SBU は Alt Mode で意味を持つことがあります。つまり Type-C は、`データ線以外にも多くの前提を抱えた connector` です。

## 8-3. Rp / Rd / Ra の意味

Type-C の attach 判定でよく出るのが `Rp` `Rd` `Ra` です。詳しい回路設計本のように数式まで掘る必要はありませんが、`どの抵抗モデルが source / sink / cable accessory を表すのか` は理解したほうが実務に効きます。少なくとも、`CC に何も入れない` `期待した role と逆の広告をする` と attach 自体が崩れる、という感覚は必要です。

実装者が最初に見たいのは、`CC1 / CC2 のどちらが有効になったか` と `想定した role を広告できているか` です。たとえば source のつもりで設計しているのに sink 的な前提で配線していれば、上位の PD や Alt Mode 以前に attach が不安定になります。ここは software log だけでは見えにくいので、配線図、controller の status、最低限の電圧観測を合わせて見たほうが安全です。

## 8-4. source / sink、DFP / UFP、DRP

Type-C では data role と power role を混同しないことが大事です。DFP / UFP はデータ側、source / sink は電力側です。さらに DRP のように両方の可能性を持つ構成もあります。ここを正しく分けられないと、`認識はするが期待した動作に入らない` という現象を、全部ひとまとめにしやすくなります。

## 8-5. cable orientation、mux、e-marker

Type-C は向きを選ばないので、内部では lane の切替や mux の存在が効いてきます。さらに cable によっては e-marker が入り、能力情報を持ちます。つまり `同じ USB-C cable に見える` ことと `同じ能力を持つ` ことは別です。Type-C の support が難しい理由のひとつは、外見が揃っているぶん内部差が見えにくいことです。

現場で困るのは、`充電はできる` `低速では見える` `映像だけ出ない` `高速 data だけ不安定` のように、条件付きで機能が欠けることです。こうしたときに cable を `ただの受動部品` と見ないほうがよいです。e-marker の有無、高速 lane の前提、電力条件の前提で、同じ外見の cable が別物として振る舞うことがあります。

## 8-6. Alt Mode と USB4 の入口としての Type-C

DisplayPort Alt Mode も USB4 も、現代では Type-C の上で語られることが多いです。だから Type-C の章で `映像も出るし高速 data も通るし給電もする` という現象の入口を作っておく必要があります。重要なのは、これらを Type-C そのものと同一視しないことです。Type-C は足場であり、そこで何が成立するかは別仕様が決めます。

## 8-7. この章のまとめ

Type-C は `形が変わった USB` ではなく、接続、役割、電力、拡張の入口をまとめた connector ecosystem です。次の章では、その上でどの電力契約を結ぶかを決める USB Power Delivery を整理します。
