---
title: "HubSpot でカスタムモジュールを作ってみよう 🎨"
emoji: "📝"
type: "tech"
topics: [HubSpot, CMS]
published: true
published_at: 2025-03-09
---

# HubSpot でカスタムモジュールを作ってみよう 🎨

2025-03-09 · Reina

### はじめに

HubSpot ページを作ってる中で、よくさわるのが「モジュール」。デフォルトやテーマで用意されているものも多いけど、なんかやりたいことができるモジュールがない…という時もありますよね。
そんな方に向けて、今回はHubSpotでカスタムモジュールを作成する方法を詳しく解説します。カスタムモジュールを使えば、ページのデザインや機能を自由にアレンジでき、より魅力的なコンテンツを提供することが可能になります。
今回はシンプルな内容&ステップバイステップで進めていきますので、一緒にチャレンジしてみましょう！

### モジュールって何？

この記事を読んでいる方には今更だと思いますが、HubSpot ページにおける「モジュール」はページを構成するためのパーツにあたるものです。（マーケティングEメールや見積もりテンプレートでも使われますが…今回はページにフォーカスします）
例えば、テキストブロックや画像ギャラリー、フォームなど、ページに必要な要素をモジュールとして組み込むことで、簡単にカスタマイズが可能になります。

📖 詳しく解説されているサイト : [HubSpotのCMSテンプレート、テーマとは？モジュールの違いや使用方法など解説](https://hubspot.100inc.co.jp/what-is-hubspot-cms-template)

### モジュールを作ってみよう

さっそくモジュール作りをはじめましょう！今回はチーム紹介に使えるモジュールを作ってみます。

【完成図】
会社のHPによくあるチーム紹介に使えるようなモジュールを作ってみます👯

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-24.png?width=1920&height=912&name=custom_module-24.png)

※基本的にはナレッジベース/Dev Docと似た内容になりますが、ドキュメントではHubSpot CLIなどの難しい部分にも触れています。ここではそういった複雑なことは気にせず、シンプルに進めていきましょう！

📖 公式ドキュメント
- [モジュールの作成と編集](https://knowledge.hubspot.com/ja/design-manager/create-and-edit-modules?__hstc=160394018.09ee7a69e432fad97dcc315634383374.1740919460200.1741093688795.1741096327854.5&__hssc=160394018.9.1741096327854&__hsfp=1996724206)
- [モジュールの概要](https://developers.hubspot.jp/docs/guides/cms/content/modules/overview)
- [モジュールを使ってみる](https://developers.hubspot.jp/docs/guides/cms/content/modules/quickstart)

#### モジュールのガワとナカミ

モジュールを作成する前に、押さえておきたい重要な概念を説明します。（これを知っておくと、手順がスムーズに進むかなと！）
モジュールにはガワとナカミがあります。
- ガワはユーザーがページエディターで操作する部分（UIの部分）
![](https://blog.ryamagami.com/hs-fs/hubfs/custom_module-25.png?width=1066&height=748&name=custom_module-25.png)
- ナカミはモジュールの動作を決める部分（コードの部分）
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-26.png?width=1078&height=596&name=custom_module-26.png)

モジュールはガワとナカミどちらか、では動かなくて、合わせて設定してあげることでやりたいことができるようになります。これらをそれぞれ次のステップで作っていきます💪

#### ステップ 1 : モジュールをデザインマネージャーで作成する

まず最初にモジュールそのものを作りましょう。
- HubSpot の［コンテンツ］> ［デザインマネージャー］をクリックして、デザインマネージャーの画面に移動します
- ［ファイル］> ［新規ファイル］をクリック
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/image-png.png?width=984&height=502&name=image-png.png)
- ［モジュール］を選びます
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-2.png?width=958&height=814&name=custom_module-2.png)
- モジュールを使う場所や名前を指定します。今回はページに使いたいので［サイトページ］を選択。名前は好きなお名前をつけましょう
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-3.png?width=858&height=1028&name=custom_module-3.png)
- ファイルの場所は（ルートフォルダー内）のままで大丈夫です。カスタムテーマを作ってたら、そこの moduleフォルダを指定してもOK
- ［作成］ボタンを押します

モジュールのファイルができました！
![](https://blog.ryamagami.com/hs-fs/hubfs/custom_module-4.png?width=1326&height=1128&name=custom_module-4.png)

まだ中身は空っぽですが、ここからがスタートです！

#### 補足）モジュールファイルの構成

モジュールファイルは
- module.html
- module.css
- module.js
の3つで構成されています。

💡ざっくり理解
- module.html 👉 モジュールのコンテンツを指定する
- module.css 👉 モジュールの見た目を指定する
- module.js 👉 モジュールに動きをつける

📖ちゃんと学びたい方はこっち見てね : [ モジュールファイル](https://developers.hubspot.jp/docs/reference/cms/modules/files)

#### ステップ 2 : モジュールができることを指定する

では次にモジュールのナカミ（とガワ）を作っていきます。

今回のモジュールでは
① 顔写真
② 名前
③ 役職
④ 自己紹介
⑤ 関連リンクに飛ばすためのリンク
の5つの要素が設定できるようにしましょう。

##### ① 顔写真
- モジュールエディターの右にある［フィールド］> ［フィールドを追加］をクリック
- 候補が表示されるので、［画像］を選択
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-5.png?width=1634&height=904&name=custom_module-5.png)
- 2. の操作で"フィールド" （＝ページエディターで触れるガワの方）ができました。ただ、この状態では実際のナカミがないのでナカミも設定します
- フィールド作成後の状態で：［HubL変数］の下にある［コピー］をクリック > ［スニペットをコピー］を選択
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-6.png?width=1634&height=714&name=custom_module-6.png)
- コピーしたスニペットを［module.html］の欄にペーストします
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-7.png?width=1634&height=616&name=custom_module-7.png)
- これで、モジュールの動きを指定するナカミも設定ができました
- 最後にわかりやすいように名前をつけておきます。✏マークをクリックして、「顔写真」と入力します
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-10.png?width=1634&height=454&name=custom_module-10.png)

##### ② 名前
- 基本的な手順は①と同じ。②は［テキスト］フィールドを追加します
- 今回②でも［テキスト］フィールドを使うため、HubL変数名は "name_field" とします
- スニペットは①で貼り付けたコードの次の行に貼り付けましょう
- フィールドの名前は「名前」に変更しましょう
![](https://blog.ryamagami.com/hs-fs/hubfs/custom_module-11.png?width=1634&height=600&name=custom_module-11.png)

##### ③ 役職
- ②と同じ手順で、［テキスト］フィールドをもう1つ追加します
- HubL変数名を"role_field" に変更し、スニペットを貼り付けます
- フィールドの名前は「役職」に変更しましょう
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-12.png?width=1900&height=546&name=custom_module-12.png)

##### ④ 自己紹介
- 自己紹介は複数行入力したいので、［リッチテキスト］フィールドを追加します
![](https://blog.ryamagami.com/hs-fs/hubfs/custom_module-12.png?width=1284&height=870&name=custom_module-12.png)
- 同様にスニペットを貼り付けて、名前は「自己紹介」とします

##### ⑤ リンク
- ［URL］フィールドを追加します
- 以下内容を追加します（"実績紹介" やクラスの部分は好きな文言でOK、フィールドの名前を変えていたら の値を更新しておきましょう）
```html
<a href="{{ module.url_field }}" class="module_button" target="_blank" rel="noopener">
 実績紹介
</a>
```

ここまでできたら一旦編集画面右上にある［変更を公開］ボタンを押しておきましょう！
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-17.png?width=1840&height=220&name=custom_module-17.png)

※応用編：既定値を設定しよう
ここでは、ページの編集者がそのフィールドにどんな画像/テキストを指定したらいいかイメージがつきやすいように既定値を設定することができます。
例：画像フィールドの場合）
［コンテンツオプション］> ［デフォルトの画像］の部分で、任意の画像を指定します。
![](https://blog.ryamagami.com/hs-fs/hubfs/custom_module-8.png?width=1368&height=774&name=custom_module-8.png)
（子猫はかわいいなあ…）
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-14.png?width=1548&height=668&name=custom_module-14.png)
モジュールをプレビューすると、デフォルトに指定した子猫ちゃんが何も設定せずとも表示されていることがわかります。（かわいい）
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-15.png?width=1568&height=968&name=custom_module-15.png)

#### ステップ 3 : モジュールをプレビューしてみる

一旦モジュールをプレビューして、どんな動きになるか見てみましょう🔍
エディター右上の［プレビュー］をクリックします。
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-17.png?width=1840&height=220&name=custom_module-17.png)

左側にさっきステップ2で設定したフィールドが表示されるので、画像を選んだりテキストを設定してみたりしましょう。👇の感じで右側にコンテンツが表示されたら、成功✨✨
![]([custom_module-18](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-18.gif?width=3599&height=2426&name=custom_module-18.gif))

#### ステップ 4 : モジュールをおめかしする

ここまでのステップで充分モジュールとして機能します！
が、今の状態だと見た目としては殺風景なので、CSS を指定しておめかしをしましょう🎀

##### 【CSS の指定方法】

CSS の指定方法としては、モジュールの module.css に追記する <style> タグに書く などありますが、今回は[ require_cssブロック と scope_css タグ を使った方法](https://developers.hubspot.jp/docs/reference/cms/modules/files#require_css%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)で書いてみます。

💡 require_cssブロック と scope_cssタグ を使った方法だと、CSS をモジュールのみ に適用させることができるのでページ自体のCSSに影響を受けないのがおすすめポイントです

##### 【CSS を設定しよう】
- モジュールの module.html の最後に {% require_css %} と {% end_require_css %} を追加します
- 追加したタグの中に、<style> タグを追加
- <style> と </style> の間に、{% scope_css %} {% end_scope_css %} を追加
![](custom_module-20)
```html
 {% scope_css %} {% end_scope_css %}
```
- の間にCSSを指定していきます
- 今回はHTMLタグ含めて👇の設定で指定してみましたが、好きなデザインに調整してみましょう

```css
{# 画像 #}
<div class="module_img">
{% if module.image_field.src %}
	{% set sizeAttrs = 'width="{{ module.image_field.width|escape_attr }}" height="{{ module.image_field.height|escape_attr }}"' %}
	{% if module.image_field.size_type == 'auto' %}
		{% set sizeAttrs = 'width="{{ module.image_field.width|escape_attr }}" height="{{ module.image_field.height|escape_attr }}" style="max-width: 100%; height: auto;"' %}
	{% elif module.image_field.size_type == 'auto_custom_max' %}
		{% set sizeAttrs = 'width="{{ module.image_field.max_width|escape_attr }}" height="{{ module.image_field.max_height|escape_attr }}" style="max-width: 100%; height: auto;"' %}
	{% endif %}
	 {% set loadingAttr = module.image_field.loading != 'disabled' ? 'loading="{{ module.image_field.loading|escape_attr }}"' : '' %}
	<img src="{{ module.image_field.src|escape_url }}" alt="{{ module.image_field.alt|escape_attr }}" {{ loadingAttr }} {{ sizeAttrs }}>
{% endif %}
</div>
{# 名前 #}
<div class="module_name">
{% inline_text field="name_field" value="{{ module.name_field }}" %}
</div>
{# 役職 #}
<div class="module_role">
{% inline_text field="role_field" value="{{ module.role_field }}" %}
</div>
{# 自己紹介 #}
<div class="module_description">
{% inline_rich_text field="richtext_field" value="{{ module.richtext_field }}" %}
</div>
{# URL #}
<a href="{{ module.url_field }}" class="module_button" target="_blank" rel="noopener">
  実績紹介
</a>

{# CSS #}
{% require_css %}
<style>
  {% scope_css %}
    .module_img img {
      width: 150px;  
      height: 150px; 
      border-radius: 50%; 
      object-fit: cover; 
      border-color:#EFBC9B; 
      margin-bottom: 10px;
    }
    .module_name {
      font-size: 2rem;
      font-weight: bold;
      color: #EFBC9B;
      margin-bottom: 10px;
      /* 名前下の区切り線 */
      border-bottom: 2px solid #FBF3D5; 
      padding-bottom: 4px; 
      width: fit-content;
    }
    .module_role {
      font-size: 1.5rem;
      font-weight: semi-bold;
      color: #9CAFAA;
      margin-bottom: 10px;
    }
    .module_description {
      margin-bottom: 10px;
    }
    .module_button {
      display: inline-block;
      padding: 10px 20px;
      background-color: #9CAFAA;
      color: white;
      text-decoration: none;
      border-radius: 5px;
      font-weight: bold;
      text-align: center;
     }
    .module_button:hover {
      background-color: #D6DAC8;
      }
  {% end_scope_css %}
</style>
{% end_require_css %}
```

##### 応用編：［スタイル］タブを定義しよう
通常モジュールを使っていると、ページエディター上の［スタイル］タブでボタンの色や文字の大きさなどを自由に指定できるものがあるかと思います。
これには、スタイルフィールドをモジュール内で指定する必要があります。今回の記事では省きますが、👇のサイトが詳しく説明されているので興味がある方はみてみてください。

📖[【HubSpot】オリジナルのモジュールにスタイルグループを定義する](https://innova-magazine.jp/module-style-custom/)

#### ステップ 5 : 作ったモジュールをページに追加する

テストページを作成して、今までに作ったモジュールが出てくるか見てみましょう！

※ルートフォルダにおいてた場合は［その他のモジュール］、テーマの中に置いてる場合は［テーマモジュール］に出てくるはずです🕵 
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-22.png?width=1920&height=854&name=custom_module-22.png)

早速ページに追加して、いろいろとさわってみましょう〜（我が家のにゃんずを設定してみました😺）
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/custom_module-23.png?width=1920&height=994&name=custom_module-23.png)

### おわりに

カスタムモジュールの作成は、最初は少しハードルが高く感じるかもしれませんが、その分、ページのデザインや機能を自由にカスタマイズできる力を手に入れることができます。

今回のステップバイステップガイドを通じて、基本的なモジュールの作成方法を学びましたが、これを基にさらに複雑なカスタマイズにも挑戦してみてください。自分だけのオリジナルなページを作り上げる楽しさを、ぜひ体験してみてくださいね！🏆
