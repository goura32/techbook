# 3章 列挙、descriptor、USB 2.0 仕様 Chapter 9 の基本

USB を実装、解析、デバッグするときに最初の土台になるのが enumeration です。device が物理的につながったあと、host は何がつながったのかを知る必要があります。そのために descriptor を読み、address を割り当て、configuration を選びます。この一連の流れが曖昧だと、class や driver の話へ進んでも足場が不安定になります。

重要なのは、enumeration は「認識したら終わり」の儀式ではないということです。host と device が契約を結ぶ最初の場面です。device descriptor はデバイス全体の入口であり、configuration descriptor は構成全体を示し、interface descriptor は機能の単位を、endpoint descriptor は転送口の性質を表します。descriptor は説明文ではなく、host が振る舞いを決めるための材料です。

`TraceDock` を例にすると、host はまず device 全体を見て、続いて configuration tree を読み、その中に HID control interface と bulk logging interface があることを知ります。この構造が整っていれば、host 側ツールは「制御は HID、ログ取得は bulk」と自然に役割を分けられます。逆に descriptor が曖昧だと、実装は動いていても host 側で扱いにくくなります。

実際の流れをもう少し現実的に言うと、host は attach を検知したあと reset をかけ、まず短い device descriptor を読み、device address を割り当て、そのあと configuration tree 全体を取得します。ここで最初に返る descriptor が壊れていると、後ろにどれだけ立派な interface 設計があっても host には届きません。enumeration は階段状に進むので、前段が壊れると後段は一切見えない、という感覚を持つことが大切です。

典型的な control request の往復を、`TraceDock` のような device で単純化すると次のようになります。

```text
Host -> GET_DESCRIPTOR(Device, 8 bytes)
Device -> 最初の 8 bytes を返す
Host -> SET_ADDRESS
Device -> ACK
Host -> GET_DESCRIPTOR(Device, full)
Host -> GET_DESCRIPTOR(Configuration, partial/full)
Host -> SET_CONFIGURATION
```

この順番を覚える価値は、packet の暗記ではなく、どこで止まったときに何を疑うかが見えるようになることです。たとえば最初の 8 bytes すら読めないなら、class や driver の問題ではなく、もっと手前の bus reset、pull-up、power、cable、PHY 初期化を疑うべきです。逆に configuration tree までは読めるのに driver が当たらないなら、descriptor 内容や class 設計の問題へ寄りやすくなります。

USB 2.0 仕様の Chapter 9 を理解するときに特に大切なのは、request の流れです。setup stage、data stage、status stage という control transfer の基本形は、enumeration のあちこちで現れます。ここを知らないと、analyzer のログを見ても何が成功し、何が失敗したのかが追いにくくなります。本文では詳細な packet 列挙までは踏み込みませんが、どの段階で止まると何を疑うべきかは押さえます。

setup stage では host が何を欲しているかを宣言し、data stage で実データが流れ、status stage でその要求が閉じます。この 3 段階は単なる形式ではなく、「要求」「内容」「完了」の境界です。analyzer で見ると短い往復ですが、ここでの失敗は切り分けに直結します。setup packet 自体が妥当か、期待した長さの data が返っているか、status まで閉じているかを順に見るだけでも、かなりの情報が取れます。

descriptor を読めるようになる価値は大きいです。OS や driver のせいに見える問題のかなりの割合が、実際には descriptor の設計や整合性にあります。class の置き方、endpoint の転送方式、power 値、interface の切り方。こうした情報は host から見える唯一の正式な自己申告です。device が自分をどう名乗るかが、そのまま挙動に効きます。

特に誤りやすいのは、「device 全体の class と interface ごとの class のどちらへ意味を置くか」「configuration の電力値と実際の振る舞いが合っているか」「endpoint descriptor の transfer type と firmware 実装が一致しているか」です。ここがずれると、OS 側では違う class と解釈されたり、そもそも interface を開きにくくなったりします。descriptor はたいてい短いので軽く見られますが、ここが最も濃い契約書です。

`TraceDock` の configuration tree を文章で表すなら、こんな考え方になります。

- Device:
  - USB device 全体の識別
- Configuration 1:
  - Interface 0:
    - HID control
    - 小さな command / status
  - Interface 1:
    - Bulk logging
    - 継続的なログ転送

この分離があると、host 側は「最小限の制御は driver 友好的に」「大きなログは完全性重視で」という設計を自然に理解できます。逆に interface を分けずに vendor-specific のひと塊で出すと、device 側は簡単でも host 側は扱いづらくなります。

enumeration で止まるときの見方も、ある程度パターン化できます。

- reset 後にまったく反応しない:
  - power、pull-up、PHY、cable、port 側を疑う
- device descriptor の途中で崩れる:
  - descriptor 長、firmware 応答、max packet size 周辺を疑う
- configuration 取得後に class が不自然:
  - interface / endpoint descriptor の整合性を疑う
- set configuration 後にだけ問題が出る:
  - class 固有初期化、endpoint enable、host driver binding を疑う

こうした切り分けを持っているだけで、USB の debug はかなり機械的に進められます。付録 B には、`TraceDock` の descriptor と class の対応を整理しています。configuration tree を見失いそうになったときに戻ると役立ちます。次の章では、enumeration の先にある 4 種類の転送方式を整理し、どの機能をどの transfer に載せるべきかを見ていきます。
