# 2章 バス、トポロジ、ホストとデバイスの役割

USB を理解するとき、まず整理したいのは「誰が主導権を持つのか」です。USB は host 主導のバスです。device は好きなタイミングで勝手に送信しません。host が列挙し、host が polling し、host が転送を進めます。この原則を押さえておくだけで、転送方式や class の理解がかなり楽になります。

役割として最低限分けたいのは、host、hub、device です。host はバス全体を管理し、hub は接続点を増やし、device は機能を提供します。ここで気をつけたいのは、Type-C や PD の世界に入ると power role や data role の話が加わるため、「電力を出す側」と「host」が同じとは限らなくなることです。この章ではまず、USB 2.0 由来の基本トポロジとしての host/device を押さえます。

この土台を曖昧にしたまま Type-C や PD の話へ進むと、data role と power role が混線しやすくなります。だから本書では、最初に host 主導という原則を固定し、そのあとで「電力側では別の role がある」という順番を取ります。順番を守るだけで、かなり多くの誤解を避けられます。

device の中でも、実装や解析で特に重要なのが interface と endpoint です。interface は機能の単位であり、endpoint は実際の転送口です。多くの誤解は、「device がひとつなら機能もひとつ」と思ってしまうところから始まります。複合デバイスでは、ひとつの device に複数の interface があり、それぞれ別の class として host に見えることがあります。

この distinction は host 側ソフトウェアでも重要です。OS や user-space から見えるのは、しばしば「device 全体」ではなく「interface ごとの機能」です。したがって、device をひとつの黒箱として扱うより、「この interface は制御」「この endpoint は logging」のように責務を切って見たほうが、host 側の code structure も整理しやすくなります。

`TraceDock` でもこの考え方を使います。制御用の HID interface と、ログ転送用の bulk interface を分けることで、host 側での扱いがかなり明確になります。もし全部をひとつの vendor-specific interface に押し込めば、短期的には楽でも、host 側ツールや debug は重くなります。つまり interface の切り方は、そのまま保守性に効きます。

hub の存在も軽く見ないほうがよいところです。開発初期は直結でしか見ていなくても、実運用では hub 経由や dock 経由が普通に入ってきます。そうなると power distribution、signal path、timing の条件が変わり、直結では出なかった不具合が現れます。USB の support が難しいのは、トポロジが製品の外側にあることが多いからです。

USB のトポロジを考えるとき、壊れる場所も役割ごとに違います。hub 越しにだけ不安定なら signal path や power distribution を疑うべきかもしれませんし、特定 OS の host controller だけで失敗するなら driver や enumeration timing を疑うべきかもしれません。役割を分けて見ることは、そのまま観測点を増やすことでもあります。

実務で役立つ見方として、まず次の 3 つを意識しておくと切り分けが速くなります。

- 直結で再現するか
- hub 経由でだけ再現するか
- host を変えると再現性が変わるか

この 3 点だけでも、問題が device 内部に近いのか、バス条件に近いのかをかなり絞れます。USB を role の関係として見る価値は、ここにもあります。

この章の結論は単純です。USB を cable 1 本の話としてではなく、host/device/hub/interface/endpoint の関係として見ることです。これがないと、次章で扱う enumeration や descriptor も、単なる field の一覧に見えてしまいます。次の章では、attach から set configuration までの流れをたどりながら、USB 2.0 仕様 Chapter 9 の基本を `TraceDock` に結びつけます。
