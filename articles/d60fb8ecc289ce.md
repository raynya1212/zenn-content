---
title: "HubSpot CMSのテンプレート種別について"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [hubspot]
published: true
---

## はじめに
HubSpotのCMS機能（現在はContent HubですがあえてCMSと呼んでます）では、1つのテーマの中に各種テンプレートがあって、そのテンプレートを使ってページを作成することができます。

https://developers.hubspot.jp/beta-docs/guides/cms/content/overview


WordPressでカスタムテーマを作ったり触ったりしたことがある方ならイメージ掴みやすいかもしれないです。

またHubSpotのCMSも歴史を重ねる毎にいろんなアップデートがあった（らしく）、この記事を書いてる2024/08現在ではテンプレートにも２種類のテンプレートがあります。
地味に混同しがちなので備忘録まとめです( ..)φ

:::message
(2026.01) 執筆途中で眠っていたのをみつけました。せっかくなので、公開します
:::

## カスタムコードテンプレート
「カスタムコードテンプレート」(HTML+HubLテンプレートともいうらしい)と呼ばれるのが新しいタイプのテンプレートです。気持ちこっちの方が推奨されている気がします

特徴としてはテンプレートファイル自体は言語で作られているのですが、
dnd_areaタグをテンプレート内に入れることで編集画面上に自由にモジュールをドラッグアンドドロップすることができるようになります。

![開発者ドキュメントより引用](https://cdn2.hubspot.net/hubfs/53/image1-2.png)

:::details 📖参考記事
https://knowledge.hubspot.com/ja/design-manager/build-a-custom-coded-template
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/types/html-hubl-templates
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/drag-and-drop/overview
:::

## ドラッグ＆ドロップテンプレート
「ドラッグ＆ドロップテンプレート」はカスタムコードテンプレートが生まれる前にあったテンプレートの形のようです。

ここネーミングがわかりづらすぎ問題なのですが、テンプレートをドラッグアンドドロップでつくれるよ！から来てる名前で、テンプレートの編集画面はカスタムコードテンプレートと大きく異なります。

![開発者ドキュメントより引用](https://knowledge.hubspot.com/hs-fs/hubfs/Knowledge_Base_Images/Content/Design_Manager/add-drag-drop-module.gif?t=1531240237399&width=2048&name=add-drag-drop-module.gif)

またドラッグアンドドロップテンプレートにもフレキシブルカラムというものが用意されています。これはテンプレート内で一部ドラッグアンドドロップでの編集を許可する箇所となります。

![開発者ドキュメントより引用](https://designers.hubspot.com/hubfs/image3-2.png)

しかし、これは**上記のdnd_areaとは別物**であることに注意です(+_+)


:::details 📖参考記事
https://knowledge.hubspot.com/ja/design-manager/structure-and-customize-template-layouts
https://developers.hubspot.jp/beta-docs/guides/cms/content/templates/types/drag-and-drop-templates
:::

## それぞれは互換性ないよ問題
以下記事でも紹介があるように、カスタムコードテンプレート と ドラッグアンドドロップテンプレートにはそれぞれ**互換性がありません**

そのため、過去にドラッグアンドドロップテンプレートで作成したページテンプレートをカスタムコードテンプレートで構築されているテーマに組み込みたい… ということを直接することはできないので注意が必要です ><

https://www.pensees.co.jp/hubspot-tips/how-to-give-compatibility-dnd-template-and-not-one


## おわりに
現在 HubSpot のテーマを開発する際はカスタムコードテンプレートを用いるのが一般的です。
ただ、過去に作成されたテーマがドラッグ＆ドロップテンプレートである という場合もあるでしょう💭

また、一枚ペラのテンプレートであればドラッグ＆ドロップテンプレートの方がコードを意識せずに容易に作成できるという点もあります。



