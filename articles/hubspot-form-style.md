---
title: "【HubSpot】ボタンが変わらない、色が効かない…🤔フォームの見た目が思い通りにならないときに読む記事"
emoji: "🎨"
type: "tech"
topics: [HubSpot, HubSpot CMS]
published: true
published_at: "2025-04-12"
---


# 【HubSpot】ボタンが変わらない、色が効かない…🤔フォームの見た目が思い通りにならないときに読む記事 🎨

2025-04-12 · Reina

## 🌿 はじめに

「フォームのボタンだけ色が変わらない…」  
「ページエディターでは青くしたのに、ページではなぜかピンクのまま…」  
そんな風に、**フォームの見た目だけが思い通りにならない😿**ことってありませんか？

![ChatGPT Image 2025年4月6日 22_07_13](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%886%E6%97%A5%2022_07_13.png)

このモヤモヤ、実は**フォームの種類やスタイルの適用順**を知るとすっきり解決できるかもしれません。

この記事では、**HubSpotのフォームにスタイルがどう適用されるのか**を、  
実際の表示やインスペクトを使いながら、やさしく解き明かしていきます🕵️‍♀️✨

## 🧩 HubSpotのフォームには２種類ある

現在 HubSpotでフォームを作るとき、  
「フォームエディター（新）」と「旧フォームエディター」という2種類のフォームが用意されています。

![form-style_01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_01.png)

2つのフォームは使い方や設定画面も少し異なりますが、  
もっとも大きな違いは「マルチステップフォームが使えるかどうか」です。

| 特徴 | 旧フォーム | 新フォーム |
| --- | --- | --- |
| マルチステップ対応 | × | ○ |
| スタイル設定 （後述） | （HubSpotページに対しては） テーマ/ページで行う | エディター内で直接設定できる |

新しいフォームでは、**複数ステップで表示できるフォーム**が作れるようになったり、  
**ボタンの色や背景色などをフォームエディターで直接指定できる**ようになったりしています。

このあと紹介するスタイルの反映ルールにも、この「新旧の違い」が大きく関わってきます👀

* 📖 [新しいフォームエディターに関するナレッジベース](https://knowledge.hubspot.com/ja/forms/create-and-edit-forms)
* 📖 [従来のフォームエディターに関するナレッジベース](https://knowledge.hubspot.com/ja/forms/create-forms)

## 🎨 新 vs 旧、スタイル設定はどう反映される？

### 🧁 旧フォーム（旧フォームエディター）のスタイルの入り方

まずは従来のフォームエディターについて、従来のフォームエディターは👇にも書いてあるように フォームエディターの［スタイルとプレビュー］で指定された内容は HubSpot ページ には反映されません。

**👉** ここの設定は、外部ページに埋め込んだ時やスタンドアロンで表示したとき（https://share.hsforms.com で始まる共有ページ）に適用される！

![form-style_02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_02.png)

### 🍰 新フォーム（フォームエディター）のスタイルの入り方

次に、新しいフォームエディターについて。新しいフォームエディターでは、**従来のフォームエディターよりもスタイル設定が充実**しています。

また、これらの設定は、HTMLの要素にインラインで反映されます。※次のセクションで詳しくみていきます🏃

**👉** つまり、HubSpotページに埋め込んだ時にもフォームエディターのスタイル設定が干渉する可能性が出てくる ということ！

![form-style_03](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_03.png)

## 🔍 インスペクトしてみよう！どのスタイルが効いてる？

「スタイル設定したのに反映されない…」  
そんなときに頼りになるのが、**ブラウザのインスペクト（検証）ツール**です。

このツールを使えば、**実際にどのスタイルが適用されていて、どれが上書きされているのか**を、  
コードではなく“目で見て”確認することができます👀✨

ここでは、旧フォームと新フォームそれぞれのインスペクトの見え方を例に、  
**「勝っているスタイル」「負けているスタイル」の見つけ方**を見ていきましょう！

![ChatGPT Image 2025年4月6日 22_28_16](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%886%E6%97%A5%2022_28_16.png)


### 🧁 旧フォーム（旧フォームエディター）の場合

旧フォームでは、フォームの見た目に直接スタイルを設定することはできません。（HubSpotページに限っては）  
そのため、**フォームに反映されるスタイルは「テーマ」や「ページエディター」など、他の場所からの設定によって決まります。**

#### 🍪 実際に見てみよう

テストページでは、ページエディター > フォーム で   
①［モジュール］> ［背景］  
と  
② ［フィールド］ > ［背景］  
を指定しているのに、②は反映されてて①は反映されていません😣

①［モジュール］> ［背景］  
薄緑色にしたいのにグレーから変わらない…

![form-style_04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_04.png)

② ［フィールド］ > ［背景］  
こっちはベージュ色の設定が反映されている…

![form-style_05](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_05.png)

どちらも同じように設定しているのに… と思うかもしれませんが、まずはテストページをインスペクト🔎して確認してみましょう👇

---

🔗テストページ：<https://23823306.hs-sites.com/page-with-old-form>

---

① モジュールの背景は？

①のモジュールの背景をインスペクトしてみると、グレーの色は "`theme-overrides.css`" から来ていることがわかります

※ HubSpotページでは、xxx.min.css と表示されている場合でも xxx.css と同じと読み替えて大丈夫です🆗

![form-style_06](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_06.png)

"`theme-overrides.css`" にはテーマエディターで指定された値が反映されることが多いので、テーマエディターの設定を見に行くことにします 👀

テーマエディターの Form スタイルをみると。。。  
❗グレーの背景色が指定されていることがわかります

![form-style_07](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_07.png)

まとめ：

テーマエディターの設定が優先されて表示される

② フィールドの背景は？

一方②のフィールド（入力欄）の背景をインスペクトしてみると、

* ページエディター側で設定した色が反映されてて（＆ `!important` がついてる）
* "`theme-overrides.css`" のスタイル設定（白の背景色）が打ち消されている

ことがわかります

![form-style_08](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_08.png)

これについてテーマエディターを見てみると、［フィールド］の項目は［フォーム］（さっきの背景色）と違って🔄のマークがないため カスタマイズされていないことがわかります👀

テーマエディターで上書きされたスタイルがない ことから、ページエディター側の設定が反映されている と考えられます💡

![form-style_09](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_09.png)

まとめ：

テーマエディター側で上書きされているスタイルがなければ、ページエディターのスタイルが採用される

### 🎂 ポイント

旧フォームエディターの場合：ページで設定した色が効かないときは、  
まずは「**テーマ側ですでに指定されていないか？**」をチェックしてみると良いかもしれません🕵️‍♀️✨

### 🍰 新フォーム（フォームエディター）の場合

新フォーム（スタイル付きフォーム）では、フォームの編集画面で**見た目のスタイルを直接設定することができます。**

そして、この設定は**HTMLの各要素にインラインスタイル（style属性）として出力される**ため、  
旧フォームと違ってページ側にも反映される可能性があることに注意🌩️

![form-style_23](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_23.png)

#### 🍪 実際に見てみよう

今回用意したテストページでは、以下の５つの要素についてみていこうと思います

1. フォームの背景色
2. 「お問い合わせ」の文字色
3. フィールドのラベル（Eメール など）の文字色
4. 進捗バーの色
5. 「次へ」ボタン の色

![form-style_10](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_10.png)

---

🔗テストページ：<https://23823306.hs-sites.com/page-with-new-form>

---

1. フォームの背景色

フォームの背景色が指定されているところを探して、インスペクトしてみると

`#hs_cos_wrapper_main-module-6`

に対して色が指定されていることがわかります

![form-style_11](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_11.png)

[📖 ナレッジベース](https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%BE%E3%81%9F%E3%81%AF%E5%85%AC%E9%96%8B%E4%B8%AD%E3%81%AE%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E6%8C%87%E5%AE%9A%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B:~:text=%E3%82%BD%E3%83%BC%E3%82%B9%E5%90%8D%E3%81%8C%E3%83%9A%E3%83%BC%E3%82%B8URL%E3%81%A7%E3%80%81%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%8C%20%23hs_cos_wrapper_module_number%E3%82%92%E3%82%BF%E3%83%BC%E3%82%B2%E3%83%83%E3%83%88%E3%81%A8%E3%81%97%E3%81%A6%E3%81%84%E3%82%8B%E5%A0%B4%E5%90%88%E3%80%81%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%AF%E3%83%9A%E3%83%BC%E3%82%B8%E3%82%A8%E3%83%87%E3%82%A3%E3%82%BF%E3%83%BC%E3%81%AE%5B%20%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%20%5D%E3%82%BF%E3%83%96%E3%81%A7%E8%A8%AD%E5%AE%9A%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E5%8F%AF%E8%83%BD%E6%80%A7)を参照すると ソース名がページURLで、スタイルが #hs\_cos\_wrapper\_module\_numberをターゲットとしている場合、スタイルはページエディターの[ スタイル ]タブで設定されている可能性 があるとのこと💭

👉今回はその通りで、ページエディターの［スタイル］で背景色が指定されているのが反映されていました

![form-style_12](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_12.png)

2.「お問い合わせ」の文字色

「お問い合わせ」の文字色 をインスペクトしてみると、`element.style` に対して文字色が指定されていることがわかります

![form-style_13](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_13.png)

📖 [ナレッジベース](https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%BE%E3%81%9F%E3%81%AF%E5%85%AC%E9%96%8B%E4%B8%AD%E3%81%AE%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E6%8C%87%E5%AE%9A%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B:~:text=%C2%A0element.style%C2%A0%E3%81%8C%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%9A%E3%82%AF%E3%82%BF%E3%83%BC%E3%81%AE%E5%AE%A3%E8%A8%80%E3%81%AE%E6%A8%AA%E3%81%AB%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E5%A0%B4%E5%90%88%E3%80%81%E3%81%9D%E3%81%AECSS%E3%81%AF%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%81%A7%E8%A8%AD%E5%AE%9A%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%81%BE%E3%81%99%E3%80%82%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%A8%E3%81%AF%E3%80%81%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AEHTML%E3%82%BD%E3%83%BC%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%89%E3%80%81%E3%81%BE%E3%81%9F%E3%81%AF%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%AE%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%83%95%E3%82%A3%E3%83%BC%E3%83%AB%E3%83%89%E3%81%AB%E7%9B%B4%E6%8E%A5%E8%BF%BD%E5%8A%A0%E3%81%95%E3%82%8C%E3%82%8B%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%A7%E3%81%99%E3%80%82)を参照すると、

> *element.style がインスペクターの宣言の横に表示されている場合、そのCSSはインラインで設定されています。インラインスタイルとは、ページのHTMLソースコード、またはテンプレートのインラインスタイルフィールドに直接追加されるスタイルです。*

とのこと

👉 上記もヒントになりますが、  
実はこれはフォームエディターの中にあるリッチテキストで色が指定されているのです。。！👀（これは結構digらないとみつからないかも💎⛏️）

![form-style_14](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_14.png)

3. フィールドのラベル（Eメール など）の文字色

フィールドラベルについては、`#hs_cos_wrapper_main-module-6` の色指定が反映されているので、１と同じパターンですね👏

![form-style_15](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_15.png)

![form-style_16](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_16.png)

またフィールドラベルの色はフォームエディターで緑色を指定しているのですが、ページで指定している赤色が反映されています。

![form-style_17](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_17.png)

👉ページエディター側でスタイル設定されている場合は、フォームエディター側のスタイルよりも ページエディターのスタイル設定が優先されるようです

4. 進捗バーの色

進捗バーは新しいフォームならでは の項目ですね🌱

これについてもインスペクトしてみると、`[data-hsfc id=Renderer] .hsfc-ProgressBar` というクラス名でバーの背景色が指定されていることがわかります

![form-style_18](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_18.png)

👉 進捗バーの色はフォームエディター側でしか設定できないので、これはフォームエディターの設定からきていることになります。

また、`[data-hsfc-id=Renderer]` は新しいフォームエディターで指定されたスタイルに入るクラスのようです

![form-style_19](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_19.png)

**5. 「次へ」ボタン の色**

「次へ」ボタンの色についてもインスペクトしてみると、4 番と同様に が `[data-hsfc-id=Renderer]` いますね

![form-style_20](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/form-style_20.png)

一方で、`.button` に対する指定は打ち消されていることがわかります（ページ右上のボタンには反映されているみたい）

![form-style_22](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_22.png)

👉 実はこのページにはボタンの色をオレンジ色に指定するスタイルシートを添付しているのですが、新しいフォームエディターのスタイル指定はページ固有のスタイルシートよりも優先されることがあるようです💫

（ここの設定ではなく‥）

![trouble-shooting_07](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/trouble-shooting_07.png)

（ここの設定が反映されている）

![form-style_21](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/form-style_21.png)

### 🎂 ポイント

🔸新しいフォームは、今までのフォームと違ってフォームエディターのスタイル設定がHubSpotページにも影響する

🔸 **ページエディターで設定したスタイルは、フォームエディターの設定よりも優先される**

**🔸 ただし、ページに添付されているスタイルシート（CSSファイル）よりは、フォームエディターの設定が強いことも。**→ 特にテーマや外部CSSファイルで設定したスタイルは、フォームエディターに“負ける”ケースがあるので注意！

https://x.com/lemma_kanazawa/status/1904779125627511158?s=20


## 📝 まとめ

フォームのスタイルは、**見えないところで“出どころの戦い”が起きている**ことも多いです。  
今回の内容をふまえて、  
「なんで効かないの？」と迷ったときには、**フォームの種類や設定の場所を見直してみる**と、きっとヒントが見つかるはずです🕵️‍♀️✨

![ChatGPT Image 2025年4月12日 18_49_24](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8812%E6%97%A5%2018_49_24.png)