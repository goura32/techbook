# Writing Policy

## この書籍で重視すること

- 操作手順ではなく判断理由
- 履歴の読みやすさとチーム運用
- 失敗例と復旧例の具体性
- GitHub の時点依存機能は公式 docs を前提に扱う

## 本文を書く前の確認

1. `shared/style-guide.md` を確認する
2. `shared/terminology.md` を確認する
3. `shared/common-explanations.md` を確認する
4. 他書の `outline.md` と `status.md` を確認する
5. レビュー、設定、CI の一般論が他書と重複しないよう確認する
6. GitHub の rulesets や merge queue は公式 docs で時点確認する

## この書籍固有のルール

- GUI と CLI の両方を扱う場合でも、まず概念を先に説明する
- 危険な操作は復旧手順とセットで説明する
- GitHub の画面説明に依存しすぎず、概念が古びにくい書き方を優先する
- Git コマンドは「安全な場面」と「共有ブランチでは避ける場面」を明記する
- merge queue や rulesets のような比較的新しい機能は、最新版紹介ではなく採用判断として書く
