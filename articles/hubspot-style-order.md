---
title: "【HubSpot】なんでスタイル設定が効かないの？ページの見た目が決まる順番を知ろう 🕵️‍♀️"
emoji: "🕵️‍♀️"
type: "tech"
topics: [HubSpot, CSS]
published: true
published_at: "2025-04-06"
---

# 【HubSpot】なんでスタイル設定が効かないの？ページの見た目が決まる順番を知ろう 🕵️‍♀️

2025-04-06 · Reina

### 🌱 はじめに
HubSpotでページを作っているときに、「スタイル設定したのに、なんでページには反映されないの…？💭」
そんな風に思ったこと、ありませんか？

ボタンの色を変えたはずなのに、なぜか変わってくれない。
フォームの見た目を整えたのに、ページではまったく違う表示になってる。
思った通りの見た目にならないとき、
「設定のしかたを間違えたのかな？」と不安になることもあると思います。

でも実は、スタイル設定がうまく反映されないのは“設定のしかた”ではなく、“どこで設定したか”が原因のことが多いんです。

この記事では、HubSpotでページの見た目がどうやって決まっているのかを、
公式ナレッジの内容をもとにわかりやすく整理し、実例付きで紹介していきます。
後半には、「反映されない！」というときに役立つ練習問題も用意しました🕵️‍♀️

「なんで反映されないの？」のモヤモヤを、“順番”の視点からスッキリ解消していきましょう！

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%886%E6%97%A5%2015_48_52.png?width=700&height=700&name=ChatGPT%20Image%202025%E5%B9%B44%E6%9C%886%E6%97%A5%2015_48_52.png)

### 🎯 見た目の正体は順番にあり！HubSpotスタイルの仕組み
「スタイル設定したのに効かない…」というときに見直したいのが、そのスタイルがどこで設定されたものかということ。

HubSpotでは、スタイルの“出どころ”によって反映される優先順位が決まっているんです。
この優先順位は、HubSpotナレッジベースでも次のように紹介されています👇

HubSpot内のCSSスタイル指定は、後述するように、それがHubSpotで設定されている場所に基づいて順序正しく適用されます。例えば、グローバルスタイルシートで設定されたスタイル指定は、特定のページで設定されたスタイル指定によって上書きされます。 

https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling

### 🔢 HubSpotのスタイル適用の順番
詳しい説明/定義はナレッジベースの方にありますが、ざっくりと理解するなら👇のイメージになります！

| 優先度 | スタイルの出どころ | 説明 |
|-------|------------------|------|
| 🥇 最強 | モジュールCSS | require_css や CSSフィールド |
| 🥈 | テンプレートに添付されたCSS | テンプレートに紐づくCSS |
| 🥉 | ページ設定のCSS | ページの[設定]タブで読み込まれるスタイルシート |
| ④ | ページ設定のCSS | サイト全体やドメイン単位で読み込まれるスタイルシート |
| ⑤ | <style> タグで直接書かれたCSS | ページ設定やテンプレートの <head> に書いたスタイル |
| ⑥ 最弱 | ページエディターのスタイル | ページビルダーで追加したスタイル。!important が付くこともあるけど他に負けることも多い |

🧩 ポイント！

スタイルが反映されない理由は「書き方のミス」ではなく、「もっと強いスタイルが上書きしているから」というケースがとても多いです

だからこそ、「どこで設定されたスタイルか？」を把握することが、トラブル解決の第一歩！

👀 次のセクションでは、インスペクトツールを使って「どのスタイルが効いてるのか？」を実際に見ていきます💻✨

 ## 🧐 インスペクトツールで “誰が勝ってるか” を見てみよう
 
 スタイル設定が反映されないとき、まずやってみたいのが「インスペクト（デベロッパーツール）」での確認です。
 
 インスペクトを使うと、**どのスタイルが効いていて、どのスタイルが効いていないか**が視覚的にわかります。  
 それによって、「設定したのに反映されない！」の原因が見えてくるんです。
 
 ### 🔧 インスペクトってどう使うの？
 
 1. Chromeなどのブラウザでページを開いて、右クリック > [検証] をクリック（もしくは ブラウザ右上の [...] > [その他のツール] > [デベロッパーツール] をクリック）
 2. [Elements] タブを開き、左上の矢印↖️をクリック
 3. 気になる要素にマウスオーバーしながら、どんなスタイルが適用されているかを確認します
 
 ![trouble-shooting_01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/trouble-shooting_01.png)
 
 > ＊操作としては、[ナレッジベースに掲載されているGIF](https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%BE%E3%81%9F%E3%81%AF%E5%85%AC%E9%96%8B%E4%B8%AD%E3%81%AE%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E6%8C%87%E5%AE%9A%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B:~:text=Google%20Chrome%E3%81%A7,%E3%82%AF%E3%83%AA%E3%83%83%E3%82%AF%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)がわかりやすかったので引用します👇
 >
 > ![inspect-live-page-cursor](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/Imported%20sitepage%20images/inspect-live-page-cursor.gif)
 
 ### 👀 ここで見るべきポイントはこの2つ！
 
 #### ✅ ① Stylesタブ：どのスタイルが当たってるか
 
 * 実際に**有効になっているスタイルは黒字**で表示されます
 * **打ち消されているスタイルは打ち消し線で消されて**で表示されます（これが「効いてないスタイル」！）
 * それぞれのスタイルの右側には、**どこから読み込まれたものか**も書いてあります
 
 ![trouble-shooting_02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/trouble-shooting_02.png)
 
 #### ✅ ② Computedタブ：最終的に効いている値
 
 * たとえば「背景色（background-color）」が何色になっているかを確認できます
 * どの設定が最終的に選ばれたのかが一目瞭然！
 
 ![trouble-shooting_03](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/trouble-shooting_03.png)
 
 ## 🔍 練習問題で体感してみよう！
 
 スタイルが反映されないとき、  
 「どのスタイルが当たっていて、どれが無視されているのか？🔍」を実際に見てみると理解がぐっと深まります。
 
 ここでは、**よくある3つの“効かないスタイル設定”の例**を取り上げて、  
 インスペクトでどこを見るべきかも一緒に確認してみましょう。
 
 ---
 
 ### ✅ 練習問題 ①：ページエディターで設定した色が反映されない Part1
 
 **状況：**  
 ページエディターの「スタイル」タブで、ボタンの背景色を青に設定。  
 でもページを開いてみると、なぜか紫のまま…。
 
 ![trouble-shooting_05](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/trouble-shooting_05.png)
 
 **テストページ ：**
 
 👇のテストページで実際にインスペクトしてみましょう！🔍
 
 <https://23823306.hs-sites.com/css_trouble-shooting_01>
 
 **チェックポイント：**
 
 * インスペクトで、ボタンの `background-color` がどのCSSから来ているかを見る
 * [ナレッジベースの記載](https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%BE%E3%81%9F%E3%81%AF%E5%85%AC%E9%96%8B%E4%B8%AD%E3%81%AE%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E6%8C%87%E5%AE%9A%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B:~:text=CSS%E3%83%AB%E3%83%BC%E3%83%AB%E3%81%AE%E5%8F%B3%E5%81%B4%E3%81%AB%E3%81%AF%E3%80%81%E3%81%9D%E3%81%AE%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E5%90%8D%E3%81%AE%20%E3%82%BD%E3%83%BC%E3%82%B9%E5%90%8D%20%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%80%81%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%8C%E3%81%A9%E3%81%93%E3%81%8B%E3%82%89%E7%94%9F%E6%88%90%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E3%81%8B%E3%81%8C%E3%82%8F%E3%81%8B%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82%E3%82%BD%E3%83%BC%E3%82%B9%E5%90%8D%20%E3%81%AE%E4%B8%8A%E3%81%AB%E3%83%9E%E3%82%A6%E3%82%B9%E3%83%9D%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%82%92%E7%BD%AE%E3%81%8F%E3%81%A8%E3%80%81%E5%AE%8C%E5%85%A8%E3%81%AA%E5%90%8D%E7%A7%B0%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)を参考にしつつ、どのソースが影響しているか確認してみましょう
 
 :::details Answer
テストページをインスペクトしてみると、ボタンの紫色はCSSファイルから来ていることがわかります📃

![](https://blog.ryamagami.com/hs-fs/hubfs/trouble-shooting_06.png?width=1700&height=578&name=trouble-shooting_06.png)

⏬

実はこのページにはスタイルシートが添付されているので、こっちのスタイルが優先された という結果になります！

ページに添付されたスタイルシート > ページエディターの設定

![](https://blog.ryamagami.com/hs-fs/hubfs/trouble-shooting_07.png?width=1700&height=938&name=trouble-shooting_07.png)

 
:::

 ### ✅ 練習問題 ②：ページエディターで設定した色が反映されない Part2
 
 **状況：**  
 ページエディターの「スタイル」タブで、ボタンの背景色を青に設定。  
 でもページを開いてみると、なぜかピンクのまま…。
 
 ![trouble-shooting_04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/trouble-shooting_04.png)
 
 **テストページ ：**
 
 👇のテストページで実際にインスペクトしてみましょう！🔍
 
 <https://46553999.hs-sites.com/css_trouble-shooting_02>
 
 **ヒント：**
 
 * 調べ方は①と同じ！
 * [スタイルのソースがページURLの場合はどんな時](https://knowledge.hubspot.com/ja/design-manager/understand-the-source-of-a-page-s-styling#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%BE%E3%81%9F%E3%81%AF%E5%85%AC%E9%96%8B%E4%B8%AD%E3%81%AE%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E6%8C%87%E5%AE%9A%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%99%E3%82%8B:~:text=%E3%82%BD%E3%83%BC%E3%82%B9%E5%90%8D%E3%81%8C%E3%83%9A%E3%83%BC%E3%82%B8URL%E3%81%AE%E5%A0%B4%E5%90%88%E3%80%81CSS%E3%81%AF%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AB%20%3Cstyle%3E%20%E3%82%BF%E3%82%B0%E5%86%85%E3%81%8B%E3%82%89%E5%8F%96%E5%BE%97%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82%E4%BE%8B%E3%81%88%E3%81%B0%E3%80%81%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%AF%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%BE%E3%81%9F%E3%81%AF%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%AE%E3%83%98%E3%83%83%E3%83%89HTML%E3%81%A7%E8%A8%AD%E5%AE%9A%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)か 見当をつけてみましょう（なんとなくこれかな？でOK！🌱）
 
 :::details Answer
テストページをインスペクトしてみると、background-color rgb(243, 165, 159) (#F3A59F)  が適用されています

そしてスタイルのソースにはページURL🔗があります

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/trouble-shooting_08.png?width=1700&height=710&name=trouble-shooting_08.png)

⏬

こういう時チェックするべきなのが、テーマ設定！

テーマエディターのスタイル設定 > buttons を確認すると、同じ #F3A59F が背景色に指定されています

👉 つまり、色はココから来ている！ということがわかります

テーマエディターの設定 > ページエディターの設定

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/trouble-shooting_09.png?width=1700&height=718&name=trouble-shooting_09.png)

 
:::
 
 ### ✅ 練習問題 ③：ヘッダーHTMLに書いたスタイルが効いてない？
 
 **状況：**  
 ② のページについて、テーマ設定を変えずにボタンの色を変えたいので  
 ページの設定 > [Head の HTML] にスタイルを追加してみた
 
 （ページ内のボタンをオレンジ色にしたい！）
 
 ![trouble-shooting_10](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/trouble-shooting_10.png)
 
 けど、色を変えたいボタンの色が反映されてない & 違うボタンの色が変わっちゃった…
 
 ![trouble-shooting_11](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/trouble-shooting_11.png)
 
 **テストページ ：**
 
 👇のテストページで実際にインスペクトしてみましょう！🔍
 
 <https://46553999.hs-sites.com/css_trouble-shooting_03>
 
 **ヒント：**
 
 * Head の HTML で指定されているクラス と 実際に適用されているクラス を比較してみると…？🔃
 
:::details Answer
テストページをインスペクトしてみると、.hs-button というクラスが変えたいボタンに適用されていることがわかります🖱️

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/trouble-shooting_12.png?width=1700&height=526&name=trouble-shooting_12.png)

一方で、Head の HTML では .button のクラスが指定されています👀

ページ右上の Contact us のボタンは .button のクラスが使われているので、オレンジ色に変わった ということですね🍊

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/trouble-shooting_09.png?width=1700&height=718&name=trouble-shooting_09.png)

⏬

Head の HTML で指定するクラスを.hs-button に変えると、変えたいボタンだけ変更できていることがわかります

テストページ：

https://46553999.hs-sites.com/css_trouble-shooting_04
:::

 ---
 
 ## 📝 まとめ
 
 「スタイル設定したのに反映されない…」  
 そんなとき、つい「自分の設定ミスかな？」と不安になってしまうこともあるかもしれません。
 
 でも、今回見てきたように、**ページの見た目は“どこでスタイルを設定したか”によって優先順位がある**ため、  
 正しく設定していても**別のスタイルが勝っている**だけ、ということがよくあります。
 
 ### 💡 今回の記事でわかったことまとめ
 
 * HubSpotでは、**スタイルの出どころによって“効きやすさ”に順番がある**
 * インスペクトツールを使えば、「どのスタイルが効いてるのか」がちゃんと見える
 
 ![ChatGPT Image 2025年4月6日 17_58_32](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%886%E6%97%A5%2017_58_32.png)
 
 「CSSが効かない！」「設定したのに見た目が変わらない…」  
 そんなときこそ、"**スタイルの優先順位" や "スタイルの出どころ"を見にいく視点**を持って、  
 まずはインスペクトで落ち着いて観察してみてください🕊️
