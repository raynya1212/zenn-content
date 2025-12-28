---
title: "【HubSpot】HubSpotページに自動目次を追加する方法【コピペOK】"
emoji: "📑"
type: "tech"
topics: [HubSpot, HubSpot CMS, JavaScript, CSS]
published: true
published_at: 2025-03-22
---

# 【HubSpot】HubSpotページに自動目次を追加する方法【コピペOK】

2025-03-22 · Reina

📌この記事は ChatGPT がメインで書きつつ、筆者が加筆しています。

### 🧭 はじめに：この記事でできること

HubSpotブログに、やさしい見た目＆アコーディオン対応の目次を追加する方法を紹介します。

HubSpotのブログには、悲しいことに **標準で目次を追加する機能がない** のです😢
一応、`<a href="#見出し">` のようにアンカーリンクを手動で書く方法👇もあるけれど、
記事が長くなったり更新が多いと、毎回リンクやIDをつけるのは結構大変…💦

https://info.nextmode.co.jp/blog/hubspot-table-of-contents


そこで今回は、**見出し（h2・h3）から自動で目次を作る方法**を紹介します🪄
読者にもやさしく、自分にもやさしい、**目次カスタマイズ**です🌷

✅ 完成系はこんな感じ！🧠

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/hubspot-toc.gif?width=1454&height=890&name=hubspot-toc.gif)

※ この記事で紹介しているコードは、私のブログの配色や雰囲気に合わせたスタイルになっています🍑
サイトのテーマやブランドカラーに合わせて、**フォントサイズや色は自由にアレンジ**してください◎

---

### ✅ 目次の仕様（今回のゴール）
- `h2`・`h3` をもとに自動で生成される
- `<details>` / `<summary>` を使って **アコーディオン形式**
- 「Read On」や関連記事の見出しは目次に含めない
- H3 は階層的に表示＋フォントサイズ小さめ
- サイトカラーに沿ったやさしい配色 🎀

---

### 🧩 実装手順（ざっくり）
- 目次を表示する **カスタムモジュール** を作る
- **HTML / JS / CSS** を設定（モジュールのフィールドは不要）
- モジュールをブログテンプレートに挿入
- デザインや色を自分のサイトに合わせて調整

> カスタムモジュールの作り方は👇の記事を参考に〜（外部リンク）

https://blog.ryamagami.com/hubspot-custom-module


---

#### 📄 1. module.html

```html
<details class="toc-accordion" open>
  <summary class="toc-summary">
    <span class="toc-arrow" aria-hidden="true"></span>
    目次
  </summary>
  <div class="toc-wrapper">
    <div class="toc-title">このページの見出し</div>
    <ul id="toc" class="toc-list"></ul>
  </div>
</details>
```

---

#### 🎨 2. module.css

> ※ 色やフォントサイズは好きな値に変更してください～

```css
.toc-accordion {
  margin-bottom: 2rem;
  font-family: "Noto Sans JP", sans-serif;
}

/* ▼ 目次トグル */
.toc-summary {
  background-color: #f0e7d8;
  color: #958e86;
  padding: 0.5rem 1rem;
  font-size: 14px;
  font-weight: bold;
  border-left: 4px solid #e5b5b0;
  cursor: pointer;
  list-style: none;
  outline: none;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.toc-summary::-webkit-details-marker { display: none; }

/* ▶ / ▼ の切り替えマーク */
.toc-arrow::before {
  content: "▼";
  display: inline-block;
  transform: rotate(0deg);
  transition: transform 0.2s ease;
  font-size: 12px;
}

.toc-accordion:not([open]) .toc-arrow::before { content: "▶"; }

/* ▼ 目次本体 */
.toc-wrapper {
  padding: 1rem;
  background-color: #ffffff;
  border: 1px solid #f0e7d8;
  border-top: none;
}

/* タイトル */
.toc-title {
  font-weight: bold;
  margin-bottom: 0.5rem;
  color: #e5b5b0;
  font-size: 13px;
}

/* 見出しリストのベース */
.toc-list,
.toc-sublist {
  list-style: none;
  padding-left: 0;
  margin: 0;
  line-height: 1.8;
}

/* H2見出し（大） */
.toc-h2 > a {
  display: block;
  font-weight: bold;
  color: #958e86;
  margin: 0.25rem 0;
  text-decoration: none;
  font-size: 14.5px;
}

/* H3見出し（サブ） */
.toc-h3 a {
  color: #958e86;
  text-decoration: none;
  font-size: 13px;
  margin-left: 0.5rem;
  display: block;
}

.toc-h3 a:hover {
  text-decoration: underline;
  color: #e5b5b0;
}
```

---

#### 💻 3. JavaScript（目次の中身を自動生成）

```js
document.addEventListener("DOMContentLoaded", function () {
  const tocContainer = document.getElementById("toc");

  const articleBody = document.querySelector(".blog-post__body");
  if (!articleBody) return;

  const headers = articleBody.querySelectorAll("h2, h3");
  let currentH2 = null;
  let tocHTML = "<ul class='toc-list'>";

  headers.forEach((header, index) => {
    const text = header.innerText.trim();
    if (text.toLowerCase().includes("read on")) return;

    const id = `section-${index}`;
    header.id = id;

    if (header.tagName === "H2") {
      if (currentH2) tocHTML += "</ul></li>";
      tocHTML += `<li class="toc-h2">
        <a href="#${id}">${text}</a>
        <ul class="toc-sublist">`;
      currentH2 = header;
    } else if (header.tagName === "H3") {
      tocHTML += `<li class="toc-h3"><a href="#${id}">${text}</a></li>`;
    }
  });

  if (currentH2) tocHTML += "</ul></li>";
  tocHTML += "</ul>";
  tocContainer.innerHTML = tocHTML;
});
```

### 🛠️補足）この JS は何をしてるの？

##### （1）ページが読み込まれたら動き始める
```js
document.addEventListener("DOMContentLoaded", function () { ... });

```

→ ブログページのHTMLが読み込まれたあとに、目次を作る処理をスタートするよ！

##### （2）目次を入れる場所を探す
```js
const tocContainer = document.getElementById("toc");
```
→ `<div id="toc">` に、あとで作った目次を入れるよ〜って準備してる🌿

##### （3）記事本文だけに限定して、h2 と h3 を探す
```js
const articleBody = document.querySelector(".blog-post__body");
const headers = articleBody.querySelectorAll("h2, h3");
```
→ 関連記事とかサイドバーじゃなく、本文だけを対象にしてるのがポイント！

##### （4）見出しごとに ID を自動でつける
```js
const id = `section-${index}`;
header.id = id;
```
→ アンカーリンクで飛べるように、見出しに `id="section-1"` みたいなIDを自動付与するよ🛫

##### （5）見出しの構造をもとに、目次のHTMLを作っていく
- `h2` のとき：新しいセクションを作って、その中に `h3` を入れられるように準備
- `h3` のとき：ひとつ上の `h2` にぶら下げるようにリストを追加

##### （6）最後に、目次のHTMLをまとめて表示！
```js
tocContainer.innerHTML = tocHTML;
```
→ `tocHTML` という **文字列のかたち**で作った目次（`<ul>〜</ul>`）を、HTMLとしてページに差し込む処理。
（＝目次の中身（HTML）を、実際に表示する場所に入れてる処理）

---

### 🧠 なぜこの実装にしたのか？

#### 🍃 1. `<details>` / `<summary>` を使う理由
- JavaScript不要でアコーディオンが使える（標準タグ）
- ボタンスタイルの競合を避けられる（テンプレート側のCSSと干渉しないように）

#### 🛡️ 2. `document.querySelector('.blog-post__body')` に限定
- 関連記事やサイドバーの見出しを除外
- `content.post_body` だけを対象にできるから、安心して運用できる！

#### 🎨 3. 色やフォントサイズをカスタマイズ
- ブランドカラーに合わせることで、サイト全体に自然になじむ
- **H2 > H3** の階層差が見た目で伝わりやすくなる

---

### 🌟 サイトに応じて変えるべきポイント

| 項目 | 調整ポイント |
|---|---|
| `querySelector` | `.blog-post__body` 以外のクラスを使っている場合は変更する |
| 色・フォントサイズ | サイトのブランドカラーに合わせて、CSSの色コードを置き換える |
| `<details open>` / `<details>` | 初期状態（開閉）を変更する場合は `open` を付け外しする |

---

### ☁️ おわりに：やさしい目次で読みやすさUP！
目次があることで、読者は記事全体の流れを把握しやすくなります👀
特に長めの記事を書くことが多い人にはとってもおすすめの実装！

もちろん、アンカーリンクで設定するのも💯なんですが、ポチポチ書き換えるのが面倒…という方はこの **カスタムモジュール** で作る方法もぜひ取り入れてみてください🫶✨
