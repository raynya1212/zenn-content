---
title: "HubSpotブログに更新日を表示する方法｜タイムゾーン・ロケールも解説"
emoji: "📅"
type: "tech"
topics: [HubSpot,HubSpot CMS]
published: true
published_at: 2025-04-29
---

# HubSpotブログに更新日を表示する方法｜タイムゾーン・ロケールも解説

2025-04-29 · Reina

## はじめに 🌿

HubSpotのブログテンプレート（ボイラープレート）では、多くの場合、公開日は最初から表示されていますが、更新日は出力されていないケースもあります。

この記事では、ブログに「更新日」を正しく追加する方法を、タイムゾーンやロケール設定も含めてやさしく解説します✨

これからカスタマイズする人の参考になったらうれしいです！

![ChatGPT Image 2025年4月29日 12_40_12](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2012_40_12.png)

---

## この記事でわかること ✏️

* HubSpotブログで「更新日」を表示する方法 📅
* HubL関数とフィルターの使い方 📄
* タイムゾーンやロケール設定の注意点 ⚡

---

## 結論 🎯

HubSpotブログに「更新日」を出したいなら、

▶ **`content.updated` HubL関数と、`format_time`フィルターを使う！**

タイムゾーンとロケールをきちんと指定すれば、きれいな日付表示ができるよ✨

（参考：[HubSpot公式フィルタードキュメント](https://developers.hubspot.jp/docs/reference/cms/hubl/filters#format_time)）

## やり方 🛠️

### Step1：公開日を出す基本 ✨

ボイラープレートのブログテンプレートには、公開日を出す基本コードが入っているよ：

```html
<time datetime="{{ content.publish_date|escape_attr }}" class="blog-post__timestamp">
  {{ content.publish_date_localized|escape_html }}
</time>
```

まずはここをそのまま使えばOK！

### Step2：更新日を追加する 🔄

更新日は、次のように書くといいよ：

```html
<time datetime="{{ content.updated|escape_attr }}" class="blog-post__timestamp">
  {{ content.updated|format_time('MMM d, yyyy', 'Asia/Tokyo', 'en') }}
</time>
```

ポイントはここ：

* `content.updated` を使う！（`last_updated`ではないよ）
* `format_time`フィルターで**タイムゾーン（Asia/Tokyo）**と**ロケール（例：en-us）**を設定する！
* フォーマットパターンは [Unicode仕様](https://unicode.org/reports/tr35/tr35-dates.html#Date_Format_Patterns) に合わせて自由に指定できるよ。
* ロケールは[このページ](https://www.oracle.com/java/technologies/javase/java8locales.html)にあるものが使えるよ！

（わたしは "MMM d, yyyy" を指定して "May 3, 2025" みたいな見た目にしたかったから、**'MMM d, yyyy', 'Asia/Tokyo', 'en'** って設定したよ！）

### Step3：アイコン統一のポイント ✨

デザインに統一感を出したいときは、[Font Awesomeのアイコン](https://fontawesome.com/)を使うのもおすすめ🐱

例えば：

```html
<i class="fas fa-calendar-alt"></i>
```

みたいに書くけど、

* Font Awesomeは事前に`<head>`タグでCDNリンクを読み込んでおく必要があるよ！
* クラス（`class="fas fa-xxx"`）で好きなアイコンを指定できるよ！

こうすることで、全体のデザインがきれいにまとまるよ✨

## 注意ポイント ⚡

* HubSpotドキュメントをよく読むと、`content.updated`を使うべきことがわかる！（間違って`last_updated`を使わないように！）
* `format_time`と`format_datetime`の違いに注意！フィルタードキュメントを確認しながら使おう！
* タイムゾーンを指定しないとUTC基準になるので、日本時間にしたいなら必ず"Asia/Tokyo"を指定しよう！
* ロケールを指定しないと月名が数字になったりするので、英語表記にしたいなら"en"を明示的に指定しよう！

## まとめ ✨

HubSpotブログに「更新日」を追加するのは、 ポイントを押さえればとってもかんたんにできるよ！

公開日・更新日をきれいにそろえて、 ブログをもっと見やすく、使いやすくしていこうね✨

この記事が少しでも参考になったらうれしいな〜

---

## あわせて読みたい 📖

* [HubSpot CMSリファレンス｜HubL変数一覧](https://developers.hubspot.jp/docs/reference/cms/hubl/variables)
* [HubSpot CMSリファレンス｜HubLフィルター一覧](https://developers.hubspot.jp/docs/reference/cms/hubl/filters)