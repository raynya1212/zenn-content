---
title: "HubSpot×React：CTAボタン付きHeroモジュールに挑戦してみた【HubSpot×React Day8】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-09
---

# CTAボタン付きHeroモジュールにチャレンジしてみた【HubSpot×React Day8】

2025-05-09 · Reina

## はじめに 🌿
HubSpot の React モジュールで **ヒーローセクションに CTA ボタン**を入れたい！と思い、試した日の記録です 🐾

## この記事でわかること ✏️
- HubSpot CMS × React で CTA を使おうとしたときの実験記録
- `CTAField` を使おうとしたときに出てきたエラーと背景
- 今のところの代替案と、次に試したいことメモ

## 結論 🎯
- 2025/05/06 時点の検証では、**React モジュール内で `CTAField` は利用不可**。
- 代わりに **`UrlField`** を使えば、リンク先を指定できるボタンとしての表現は問題なく実現できる。

## やり方の概要 🛠️
- モジュールの `fields` に **`UrlField`** を追加し、`fieldValues.ctaUrl` として読み取る。
- そのリンク先を使ってカスタムの CTA ボタンを表示する方法で対応。

## つまずきポイント ⚡
- 参考: [Module/Theme fields の CTA](https://developers.hubspot.com/docs/reference/cms/fields/module-theme-fields#cta)
- 実際には以下のようなエラーが発生：
  ```
  SyntaxError: The requested module '@hubspot/cms-components/fields' does not provide an export named 'CTAField'
  ```
- 現行の `@hubspot/cms-components` に `CTAField` が含まれていない模様。

## まとめ ✨
- まずは **`UrlField` のボタン化**で実装自体は進める。
- 公式の更新や情報が揃ってきたら、**`CTAField` 方式**にも再トライしたい。

> ※ 本文中の HubSpot の埋め込みモジュール（`{% module_block %}`）は Markdown では通常リンクへ簡略化／もしくは削除しています。
