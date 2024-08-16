---
title: "HubSpot CMSのテンプレート種別について"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [hubspot]
published: false
---
<!--まだ作成途中-->

## はじめに
HubSpotのCMS機能（現在はContent HubですがあえてCMSと呼んでます）では、1つのテーマの中に各種テンプレートがあって、そのテンプレートを使ってページを作成することができます。

https://developers.hubspot.jp/beta-docs/guides/cms/content/overview


WordPressでカスタムテーマを作ったり触ったりしたことがある方ならイメージ掴みやすいかもしれないです。

またHubSpotのCMSも歴史を重ねる毎にいろんなアップデートがあった（らしく）、この記事を書いてる2024/08現在ではテンプレートにも２種類のテンプレートがあります。
地味に混同しがちなので備忘録まとめです( ..)φ


## カスタムコードテンプレート
「カスタムコードテンプレート」(HTML+HubLテンプレートともいうらしい)と呼ばれるのが新しいタイプのテンプレートです。気持ちこっちの方が推奨されている気がします

特徴としてはテンプレートファイル自体は言語で作られているのですが、
dnd_areaタグをテンプレート内に入れることで編集画面上に自由にモジュールをドラッグアンドドロップすることができるようになります。

:::details 💡見分け方

:::

:::details 📖参考記事
https://knowledge.hubspot.com/ja/design-manager/build-a-custom-coded-template
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/types/html-hubl-templates
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/drag-and-drop/overview
:::

## ドラッグアンドドロップテンプレート
「ドラッグアンドドロップテンプレート」はカスタムコードテンプレートが生まれる前にあったテンプレートの形のようです。

ここネーミングがわかりづらすぎ問題なのですが、テンプレートをドラッグアンドドロップでつくれるよ！から来てる名前で、編集画面はカスタムコードテンプレートと大きくことなります。

またドラッグアンドドロップテンプレートにもフレキシブルカラムという自由に・・・
ですが、これは**上記のdnd_areaとは別物**です(+_+)

:::details 💡見分け方

:::

:::details 📖参考記事
https://knowledge.hubspot.com/ja/design-manager/structure-and-customize-template-layouts
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/types/drag-and-drop-templates
:::

## それぞれは互換性ないよ問題
https://www.pensees.co.jp/hubspot-tips/how-to-give-compatibility-dnd-template-and-not-one


## おわりに



