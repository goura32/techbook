# 後付

## おわりに

Git と GitHub は、コードを書くための道具ではありません。変更を安全に流し、レビューし、戻せる形で残すための道具です。だからこそ、速く操作できることそのものよりも、どの変更をどの単位で流し、どこで止め、壊れたときにどう戻すかを考えられることのほうが長く効きます。

本書では、コミット、ブランチ、Pull Request、rulesets、merge queue、リリース、事故復旧までを、ひとつの変更管理の流れとして扱ってきました。Git の柔軟さは強みですが、その柔軟さを無制限に使うと、チームではすぐに履歴が読みにくくなり、レビューが詰まり、復旧も難しくなります。だからこそ、変更を小さく保ち、意図を明確にし、共有ブランチを守り、復旧できる形で流すことが大切になります。

`BeaconBoard` は小さな題材でしたが、そこで見てきた考え方は、より大きなプロダクトや複数チームの開発でもそのまま使えます。良いコミットは何か、レビューしやすい差分とは何か、保護設定はどこまで入れるべきか、例外運用はどう戻すか。こうした判断は派手ではありませんが、長く効きます。

ここで扱った考え方が、今後ほかの言語やツールを選ぶときにも、変更管理、公開面、更新運用を考えるための土台として残ればうれしく思います。

## 参考情報

### Git 公式情報

- Git Documentation  
  種別: 公式ドキュメント  
  URL: `https://git-scm.com/doc`
- Pro Git  
  種別: 公式書籍  
  URL: `https://git-scm.com/book/en/v2`

### GitHub 公式情報

- Creating rulesets for a repository  
  種別: 公式ドキュメント  
  URL: `https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/creating-rulesets-for-a-repository`
- Managing a merge queue  
  種別: 公式ドキュメント  
  URL: `https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue`
- Events that trigger workflows  
  種別: 公式ドキュメント  
  URL: `https://docs.github.com/en/actions/reference/events-that-trigger-workflows`

### そのほかの公式情報

- About releases  
  種別: 公式ドキュメント  
  URL: `https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases`
- About protected branches  
  種別: 公式ドキュメント  
  URL: `https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches`

## 著者紹介

大島 のりあ。ソフトウェア開発の現場で、言語やツールそのものの機能よりも、「どこで壊れやすく、どこで判断を誤りやすいか」を言葉にすることに強い関心を持つ技術書執筆者。アプリケーション実装に加えて、CLI、開発基盤、技術文書、モノレポ運用まで含め、変化の多いコードベースを長く保守できる形へ整えることを重視している。

シリーズ `技術の輪郭` では、文法の紹介や手順の列挙で終わらず、言語、OSS、規格、ファイル形式のそれぞれについて、「全体像をつかみ、実務で判断できるようになること」を目指している。特に、境界設計、公開 API、変更管理、更新に強いツールチェーン運用のような、技術が変わっても残り続ける論点を扱うことを大切にしている。

## 奥付

- 書名: `技術の輪郭 Git & GitHub`
- サブタイトル: `レビュー、保護設定、マージ戦略、事故復旧の実務`
- シリーズ名: `技術の輪郭`
- 著者名: `大島 のりあ`
- 発行日: 2026年5月13日（仮）
- バージョン: 初版
- 発行形態: Kindle Direct Publishing による電子書籍
- 連絡先または案内先: `noria.library@gmail.com`
