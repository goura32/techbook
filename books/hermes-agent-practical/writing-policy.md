# Writing Policy

## この書籍で重視すること

- 話題性より継続運用
- デモより権限制御と事故防止
- 比較より Hermes Agent の実践導入
- 最新機能の列挙より、導入判断と運用原則

## 本文を書く前の確認

1. `shared/style-guide.md` を確認する
2. `shared/terminology.md` を確認する
3. `shared/common-explanations.md` を確認する
4. 他書の `outline.md` と `status.md` を確認する
5. Git 基礎、CLI 基礎、設定管理の一般論を重複記述しないよう確認する
6. Hermes Agent の時点依存情報は公式 docs と公式 releases で確認する

## この書籍固有のルール

- バージョン変化が速いため、UI の見た目より概念と判断軸を優先する
- 外部モデルや外部ツールの比較は必要最小限に留める
- セキュリティ関連の説明は、便利さだけで締めずリスクも対で書く
- Profiles は人格の違いとしてではなく、責務と権限の分離として説明する
- Skills はプロンプト集ではなく、再利用可能な手順資産として扱う
- Memory、Gateway、Cron のような長期状態を持つ機能は、便利さより削除、停止、監査の導線を先に書く
- 比較章では OpenCode / OpenClaw の機能紹介を広げすぎず、採用条件の違いに絞る
