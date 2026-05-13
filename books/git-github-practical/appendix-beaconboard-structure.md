# Appendix A. BeaconBoard の全体構成

## この付録の役割

この付録は、本文中で断片的に登場する `BeaconBoard` の全体像をまとめて参照するためのものです。本書では各章で必要な履歴や運用断片だけを示しますが、それだけだと読者が「この変更はどの層の話だったか」「どの運用前提で rulesets や merge queue を考えていたか」を見失いやすくなります。

## `BeaconBoard` の前提

`BeaconBoard` は、Web アプリ、API、docs を持つ小規模なプロダクト用リポジトリです。意図的に次のような構成を持ちます。

- `web/`
- `api/`
- `docs/`
- `.github/workflows/`

チーム規模は 2 から 8 人程度を想定します。リリースは頻繁だが、main への直 push は行わず、Pull Request と CI を通して変更を流します。

## 想定ディレクトリ構成

```text
beaconboard/
  web/
  api/
  docs/
  .github/
    workflows/
  CODEOWNERS
  .github/PULL_REQUEST_TEMPLATE.md
```

## 運用前提

- 日常の開発は feature branch で行う
- main は保護されている
- Pull Request でレビューと必須チェックを通す
- 変更量が増えたら merge queue を導入する
- リリースはタグと release notes で管理する

## 各章との関係

- 2章: `web` と `api` の変更をどう分割してコミットするか
- 4章: Pull Request テンプレートとレビューの前提
- 6章: rulesets と権限設計をどう置くか
- 7章: `.github/workflows/` と merge queue の関係
- 9章: タグと releases をどこで扱うか
- 10章: shared branch 事故時の戻し方

## 付録 B / C との関係

付録 A は `BeaconBoard` の構造と運用前提を見るためのものです。付録 B では主要操作と GitHub 機能の対応を、付録 C では代表的な運用断片と復旧断片を確認できます。
