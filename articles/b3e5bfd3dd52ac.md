---
title: "[HubSpot]ドラッグ＆ドロップテンプレートで、簡単にカスタムテンプレートを作ってみよう"
emoji: "🖱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["HubSpot","CMS","HubSpot CMS"]
published: true
---

## はじめに
HubSpot CMS （Content Hub）では、"ページテンプレート" を用いてランディングページやウェブサイトページを作ることができます。

テンプレートは、基本的には デフォルトで用意されているテーマ内のもの や マーケットプレイスにあるテーマ内のもの を用いることが多いかと思います🎨

ただ、ここで少し問題となるのが **テーマテンプレートはテーマのスタイル設定やヘッダー・フッター設定を引き継いでしまう** という点です。

[こちら](https://hubspot.100inc.co.jp/what-is-hubspot-cms-template) のサイトにある図がわかりやすいのですが、テーマが上位にあるため、例えばテーマ側で設定したヘッダー（メニュー）は テーマ内のすべてのテンプレートに適用される という形になります。

![](https://hubspot.100inc.co.jp/hs-fs/hubfs/image-png-Jun-20-2024-06-39-25-1580-AM.png?width=4336&height=1604&name=image-png-Jun-20-2024-06-39-25-1580-AM.png)
*画像はこちら[^1]からお借りしました*

すると、ここで気になってくるのが、単発のセミナーのためのランディングページを作りたいけど、
- ヘッダーは不要
- ペライチのテンプレートがあれば大丈夫
- テーマ設定（グローバルコンテンツエディター[^2]）はメンバーが混乱するので、あまり触りたくない

といった場合です。

こういった場合、テンプレートをカスタム開発する という選択肢が出てきますが、カスタムコードテンプレートには HubL の知識が必要となってきます 💭

👆の課題を解決するために今回は **「ドラッグ＆ドロップテンプレート」** でシンプルなランディングページテンプレートを作成していこうとおもいます！[^3]

---
📌 カスタムコードテンプレート と ドラッグ＆ドロップテンプレート の概要・違い については👇の記事をご参照ください：
https://zenn.dev/raynya/articles/d60fb8ecc289ce

:::message alert
[HubSpot のドキュメント](https://developers.hubspot.jp/docs/cms/start-building/building-blocks/drag-and-drop/drag-and-drop-templates) でも記載があるように、ドラッグ＆ドロップテンプレートはメンバーシップなど一部機能をサポートしていないことに注意です。
:::
---

## この記事のゴール
👇のような、ウェビナーのランディングページを作っていきたいと思います：

![](/images/b3e5bfd3dd52ac/dnd_11.gif)

つくりはシンプルですが、テンプレートを用意することで、同じ構成を各セミナーに対して使えるようになります 🚀

## ドラッグ＆ドロップテンプレートの作り方
HubSpot の [コンテンツ] > [デザインマネージャー] を開き、[新規ファイルを作成] > [ドラッグ＆ドロップ] をクリックして作成します。

![](/images/b3e5bfd3dd52ac/dnd_01.png)

## 基本的な操作
テンプレート作成画面 右 にあるモジュールを
テンプレート作成画面 左 の画面に**ドラッグ＆ドロップ**することで、
テンプレートにモジュールを追加していきます🖱

![](/images/b3e5bfd3dd52ac/dnd_02.gif)

### テンプレートの構造
ちょっと👆の gif 画像だと薄くてわかりづらいのですが、テンプレート編集画面は
- ヘッダー
- 本文
- フッター
に構造がわかれているので、追加したい箇所にモジュールをドラッグ＆ドロップします🖱

今回サンプルで作ったテンプレートは1列で構成しているのですが、設置したモジュールの横にドラッグ＆ドロップすることで列を構成することも可能です：
![](https://knowledge.hubspot.com/hs-fs/hubfs/Knowledge_Base_Images/Content/Design_Manager/change-column-width.gif?t=1531240237399&width=2048&name=change-column-width.gif)
*HubSpot ナレッジベースから引用[^4]*

また、👇のようにモジュールをグループ化[^5]させることが可能です：
![](/images/b3e5bfd3dd52ac/dnd_03.gif)

グループ化は、この後登場するスタイリングにて活用ができます🎨

## フレキシブルカラムを追加する
フレキシブルカラムは、**ページ編集画面で自由にモジュールを追加/削除することを許可するエリア**です🐧

例えば👇では、フレキシブルカラムの中に フォームやリッチテキスト のモジュールを追加しています。

![](/images/b3e5bfd3dd52ac/dnd_04.gif)

これらのモジュールはフレキシブルカラム内に設置されているため、ページ編集画面側で削除したり、また他のモジュールを同じエリアの中に追加したり することが可能になります。

:::message
逆に、**フレキシブルカラム外に置かれているモジュールは、ページ編集画面で削除することができず、「静的モジュール」という扱い**になります。
:::

## デフォルトのコンテンツを用意する
会社名など、この場所には決まってこの文言を置きたい/この画像を設置したい といった場合は、モジュールの [編集] > [デフォルトのコンテンツ] で指定することが可能です。

![](/images/b3e5bfd3dd52ac/dnd_06.png)

## CSS/インラインスタイルを追加する
上記の流れで基本的な構成は作成できますが、このままだと何もスタイルの無い状態（とっても素朴な状態）になってしまいます🙈

ドラッグ＆ドロップテンプレートは CSS 記述したスタイルシートを添付したり、インラインスタイルでスタイルを記述したりすることができますが、今回は**お手軽さを重視して[インラインスタイル](https://developer.mozilla.org/ja/docs/Learn_web_development/Core/Styling_basics/Getting_started#%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB)中心で調整**していきます。[^6]

モジュールグループや各モジュールを選択して、[インラインスタイル] にスタイル設定を記述することができます：
![](/images/b3e5bfd3dd52ac/dnd_05.png)

ただ、既定のフォームモジュールでは同様に設定ができなかったため、テンプレート全体の [編集] > [追加の<head>マークアップ] に記載してみました：
![](/images/b3e5bfd3dd52ac/dnd_12.png)

```css:今回設定してみた内容
<style>
/* フォーム全体のラップ */
form.hs-form {
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  max-width: 600px;
  margin: 0 auto;
}

/* ラベル */
.hs-form label {
  font-weight: bold;
  margin-bottom: 0.5rem;
  display: block;
  color: #2c3e50;
}

/* 入力フィールド */
.hs-form input[type="text"],
.hs-form input[type="email"],
.hs-form textarea {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 1rem;
  margin-bottom: 1rem;
  box-sizing: border-box;
}

/* 送信ボタン */
.hs-form input[type="submit"] {
  background-color: #2c3e50;
  color: white;
  padding: 0.8rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.hs-form input[type="submit"]:hover {
  background-color: #e5941f;
}
</style>
```

テンプレートができたら、**忘れずに [変更を公開] をクリック**しましょう🚀

## ランディングページを作る
作成したテンプレートでランディングページを作るには、テンプレート選択画面で [その他] を選択し
![](/images/b3e5bfd3dd52ac/dnd_07.png =250x)
今回作成したテンプレートを選びます：
![](/images/b3e5bfd3dd52ac/dnd_08.png)

ページ編集画面はいつも通りの操作感ですが、[コンテンツ]を開くと 「静的モジュール」と「フレキシブルカラム」の範囲がどこか をみることができます 👀
![](/images/b3e5bfd3dd52ac/dnd_09.png)

**フレキシブルカラムのエリア内なら、👇のように自由にモジュールを追加したり削除したり並び替えたり することができます**🖱
![](/images/b3e5bfd3dd52ac/dnd_10.gif)

## おわりに
現在テンプレートをカスタム開発するとなると HubL+HTML のカスタムコードテンプレートを作成する というのが一般的かと思います💭

ドラッグ＆ドロップテンプレートは "新しいテンプレートには推奨されません"[^7] とされている一方、**コーディングをすることなくテンプレートをサクッとつくれる** という便利さもあります 🏃🏃

たしかに柔軟性に欠ける部分はありますが、地味に便利と感じる場面もあるので、必要に応じて活用できるといいですね 😺

---
https://knowledge.hubspot.com/ja/design-manager/structure-and-customize-template-layouts

https://developers.hubspot.jp/docs/cms/start-building/building-blocks/drag-and-drop/drag-and-drop-templates

[^1]: https://hubspot.100inc.co.jp/what-is-hubspot-cms-template

[^2]: https://knowledge.hubspot.com/ja/design-manager/use-global-content-across-multiple-templates#%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%82%A8%E3%83%87%E3%82%A3%E3%82%BF%E3%83%BC%E3%81%A7%E3%82%B0%E3%83%AD%E3%83%BC%E3%83%90%E3%83%AB%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%82%92%E7%B7%A8%E9%9B%86%E3%81%99%E3%82%8B

[^3]: 昔は[スターターテンプレート](https://knowledge.gate39media.com/how-do-i-create-a-landing-page-in-hubspot#:~:text=Create%20a%20landing%20page%20with%20a%20starter%20template)があったので、サクッと作りたいときは便利だったんですけどね… いつか忘れましたが、消えてしまいました

[^4]: https://knowledge.hubspot.com/ja/design-manager/structure-and-customize-template-layouts から引用

[^5]: https://developers.hubspot.jp/docs/cms/start-building/building-blocks/drag-and-drop/drag-and-drop-templates#%E3%82%B0%E3%83%AB%E3%83%BC%E3%83%97

[^6]: インラインスタイルとか HTML に直接スタイルを書くのは…というところもありますが、今回はサクッと作りたい気持ちを優先します🍁

[^7]: https://developers.hubspot.jp/docs/cms/start-building/building-blocks/drag-and-drop/drag-and-drop-templates#:~:text=%E3%83%89%E3%83%A9%E3%83%83%E3%82%B0%EF%BC%86%E3%83%89%E3%83%AD%E3%83%83%E3%83%97%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%AF%E3%80%81%E6%96%B0%E3%81%97%E3%81%84%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%AB%E3%81%AF%E6%8E%A8%E5%A5%A8%E3%81%95%E3%82%8C%E3%81%BE%E3%81%9B%E3%82%93%E3%80%82