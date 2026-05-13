# 4章 転送方式とスケジューリング

USB の transfer type は、control、bulk、interrupt、isochronous の 4 つに整理されます。名前だけ覚えるのは簡単ですが、実務では「どの機能をどこへ載せるか」が重要です。transfer type の選択は、性能だけでなく、host 側の扱いやすさ、デバッグのしやすさ、失敗時の見え方にも影響します。

control transfer は必須の基礎で、enumeration や設定変更のような小さな制御に使います。bulk transfer は完全性を重視し、大きなデータを欠損なく流したいときに向きます。interrupt transfer は短い通知を定期的に取りたいときに便利です。isochronous transfer は時間優先で、再送しないかわりに遅延を抑えたい音声や映像に向きます。

ここで誤解しやすいのは、4 種類の transfer が「性能のランク」ではないことです。bulk が上位、interrupt がその次、isochronous が特殊、といった見方ではなく、それぞれが違う契約を持っています。どれが優れているかではなく、どの失敗を許容し、どの性質を優先するかで選びます。

ここで大切なのは、「速そうだから bulk」「リアルタイムっぽいから interrupt」のような雑な決め方をしないことです。たとえば `TraceDock` では、設定変更や mode 切り替えのような小さな制御は HID 側へ、ログの本体は bulk へ分けます。これにより host 側での責務が明確になり、ログが多少遅くても欠けてほしくない、という要件とも整合します。

もしここで logging まで interrupt 的に流そうとすると、host 側の polling 周期や payload の前提に引きずられやすくなります。逆に control 的なやり取りまで bulk へ押し込むと、debug 時に「設定経路が生きているか」が見えにくくなります。つまり transfer type は payload のサイズだけでなく、切り分けやすさにも効きます。

USB は host 主導なので、device は勝手に送れません。そこには polling や scheduling の考え方が関わります。device 側から見ると「通知したい」ように見える場面でも、実際には host がその endpoint を見に来る仕組みです。この前提を見失うと、firmware 側で無理な設計をしやすくなります。

特に interrupt transfer は名前が誤解を生みやすいところです。割り込みのように device が主体で押し込むのではなく、host が周期的に見に来る契約です。ここを誤解すると、device 側で「即時通知」を期待しすぎたり、host 側で「なぜ遅延するのか」を違う層で説明しようとしてしまいます。

transfer type の選択は debug にも効きます。bulk で詰まっているのか、enumeration 自体が失敗しているのか、interrupt が想定周期で観測されないのか。どの transfer に何を載せたかが明確であれば、capture を見たときの解釈もかなり楽になります。反対に、全部を vendor-specific のひとつの流儀で抱えると、解析は重くなります。

商用製品では、「何が最低限生きていれば field で観測を続けられるか」を考えて transfer を選ぶと強いです。たとえば `TraceDock` では、HID control が生きていれば mode や状態を確認でき、bulk logging が失敗しても少なくとも制御経路は残ります。全部が同じ経路に乗っていると、異常時に観測そのものが失われやすくなります。

この章の結論は、transfer type は性能表ではなく契約だということです。何を優先するかを先に決め、その契約に合う転送方式を選びます。次の章では、USB の見た目を大きく変えた USB Type-C を扱い、コネクタ、ケーブル、能力表示の関係を切り分けます。
