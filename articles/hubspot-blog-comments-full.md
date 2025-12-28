---
title: "【HubSpot】HubSpotブログにコメント欄をつけてみた！出てこないときの対処とカスタマイズ記録🐾"
emoji: "💬"
type: "tech"
topics: [HubSpot, Blog, HubL, CSS]
published: true
published_at: 2025-04-20
---

# 【HubSpot】HubSpotブログにコメント欄をつけてみた！出てこないときの対処とカスタマイズ記録🐾

2025-04-20 · Reina

HubSpotのブログに「コメント欄を表示したい！」と思って設定をONにしたのに、あれ？なんか出てこない…？そんなことがありました🙀

![ChatGPT Image](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8820%E6%97%A5%2017_08_13.png?width=700&height=700&name=ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8820%E6%97%A5%2017_08_13.png)

今回は、コメント欄の表示でちょっとつまずいたれいなが、いろいろ調べて気づいたことや、カスタマイズのメモをまとめてみたよ〜🐾

📚 ブログコメントに関する基本的な設定は以下ナレッジベースにまとまってます

https://knowledge.hubspot.com/ja/blog/set-up-and-moderate-your-blog-comments


---

### ✅ コメント欄が表示される条件
HubSpotブログでコメント欄をちゃんと表示させるには、いくつかの条件があるみたい。
- ブログ設定で「コメントを許可」する
- 管理画面 → [設定] → [ウェブサイト] → [ブログ] → [コメント] のところで「コメントを許可」にしておく必要あり！

![](https://blog.ryamagami.com/hs-fs/hubfs/Imported%20sitepage%20images/turn-on-commenting.png?width=2478&height=562&name=turn-on-commenting.png)

- テンプレートにモジュールを設定する
- `@hubspot/blog_comments` モジュールをテンプレートに書いておく必要があるよ〜

https://developers.hubspot.jp/docs/cms/reference/hubl/tags/standard-tags#%E3%83%96%E3%83%AD%E3%82%B0%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%88

---

### 🧰 コメント欄ってどうやって表示されてるの？

HubSpotのコメント欄は、 `@hubspot/blog_comments` っていう専用モジュールを使って表示されてる💡
これをテンプレートに書くだけで、名前・メール・コメント欄までまるっと自動で出してくれる親切設計！

実はこのモジュール、裏側では「マーケティング > フォーム」に自動で作成された専用のコメント用フォームを見に行ってます👇（たとえば以下）

![](https://blog.ryamagami.com/hs-fs/hubfs/Imported%20sitepage%20images/edit-your-comment-form.png?width=1702&height=558&name=edit-your-comment-form.png)


だから開発側としては、テンプレートにモジュールを入れるだけでOKなのが楽ちんだし、マーケ側にとっても、フォームにある項目の変更にはコードをいじる必要なし🌱

---

### 🐞 出てこないときはココをチェック！
「あっ、書いたのに出てこない…💦」ってときに見ておきたいチェックリストはこちら👇

| 確認ポイント | 内容 |
|---|---|
| ブログ設定 | コメント機能そのものがONになってるか（テンプレで `{{ group.allow_comments }}` を出力してチェックもあり） |
| テンプレート | モジュール `@hubspot/blog_comments` をちゃんと書いてるか |
| CSSの影響 | `.blog-comments` に `display: none;` とかが当たってないか、インスペクトで見てみよう |

---

### 🎨 コメント欄の見た目をちょこっと整えたい
コメント欄って便利なんだけど、送信ボタンのサイズや色がちょっと目立ちすぎちゃうことも…！
そんなときは、CSSで「コメント欄だけ」見た目を整えるのがコツ🍀 たとえばボタンを私好みに調整すると、こんな感じ👇

```css
.blog-comments .actions input.hs-button.primary[value="Submit Comment"] {
  font-size: 15px;
  padding: 0.5em 1.4em;
  background-color: #E5B5B0;
  color: #fff;
  border: none;
  border-radius: 12px;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.1);
  transition: background-color 0.2s ease, transform 0.15s ease;
  margin-bottom: 1em;
}

.blog-comments .actions input.hs-button.primary[value="Submit Comment"]:hover {
  background-color: #d29e99;
  transform: translateY(-1px);
  box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.15);
}
  
```

`.blog-comments` の中にあるボタンだけに限定してるから、他のボタン（例えばお問い合わせとか）には影響しないよ🐈

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8820%E6%97%A5%2017_20_54.png?width=700&height=700&name=ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8820%E6%97%A5%2017_20_54.png)

---

### ✨ 最後にひとこと
HubSpotのコメント欄、書けばすぐ出るかと思いきや、意外と「いくつかの条件」がそろわないと出てこないのがちょっとややこしかったり💦
でも、ひとつずつ確認していけば大丈夫！久しぶりに設定変更して「あれれ？」なことがあったら、この記事にあるステップを試してみてください🐾💬
