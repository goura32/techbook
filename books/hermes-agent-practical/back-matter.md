# 後付

## おわりに

Hermes Agent の価値は、何でもできそうに見えることではありません。何をどの profile に任せ、どこで context を固定し、どこまで外部能力を足し、どこで安全装置を置くかを、自分の運用に合わせて決められることにあります。

本書では、インストール、CLI、Skills、MCP、Profiles、Memory、Checkpoints、Cron、セキュリティまでを、ひとつの運用基盤として扱ってきました。Hermes Agent は柔軟ですが、その柔軟さを無制限に広げると、文脈が混ざり、権限が広がり、障害時に戻りづらくなります。だからこそ、責務を分け、状態を分け、外部能力を明示的に足し、復旧可能な運用を先に整えることが大切になります。

`ForgeFlow` は小さな題材でしたが、そこで見てきた考え方は、より大きな開発チームや長時間稼働するエージェント運用でもそのまま使えます。どこで approvals を入れるか、どこまで memory を広げるか、どの作業を cron に任せるか、どこで profile を分けるか。こうした判断は派手ではありませんが、長く効きます。

ここで扱った考え方が、今後ほかのエージェントや開発支援ツールを選ぶときにも、権限、文脈、更新運用を考えるための土台として残ればうれしく思います。

## 参考情報

### Hermes Agent 公式情報

- Hermes Agent GitHub Repository  
  種別: 公式リポジトリ  
  URL: `https://github.com/NousResearch/hermes-agent`
- Hermes Agent Documentation  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/`
- Hermes Agent v0.13.0 Release Notes  
  種別: 公式リリースノート  
  日付: 2026年5月7日  
  URL: `https://github.com/NousResearch/hermes-agent/releases/tag/v2026.5.7`

### 主要機能の公式情報

- Profiles: Running Multiple Agents  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/user-guide/profiles/`
- MCP (Model Context Protocol)  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/`
- Checkpoints and /rollback  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/user-guide/checkpoints-and-rollback`
- Scheduled Tasks (Cron)  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/`
- Memory Providers  
  種別: 公式ドキュメント  
  URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers/`

## 著者紹介

大島 のりあ。ソフトウェア開発の現場で、言語やツールそのものの機能よりも、「どこで壊れやすく、どこで判断を誤りやすいか」を言葉にすることに強い関心を持つ技術書執筆者。アプリケーション実装に加えて、CLI、開発基盤、技術文書、モノレポ運用まで含め、変化の多いコードベースを長く保守できる形へ整えることを重視している。

シリーズ `技術の輪郭` では、文法の紹介や手順の列挙で終わらず、言語、OSS、規格、ファイル形式のそれぞれについて、「全体像をつかみ、実務で判断できるようになること」を目指している。特に、境界設計、公開 API、変更管理、更新に強いツールチェーン運用のような、技術が変わっても残り続ける論点を扱うことを大切にしている。

## 奥付

- 書名: `技術の輪郭 Hermes Agent`
- サブタイトル: `Skills、MCP、Memory、Profiles、運用設計までを実務でつなぐ`
- シリーズ名: `技術の輪郭`
- 著者名: `大島 のりあ`
- 発行日: 2026年5月13日（仮）
- バージョン: 初版
- 発行形態: Kindle Direct Publishing による電子書籍
- 連絡先または案内先: `noria.library@gmail.com`
