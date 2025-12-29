---
title: "【HubSpot】理想のコードブロックをつくるまでのハック集 🧩"
emoji: "🧩"
type: "tech"
topics: [HubSpot, HubSpot CMS]
published: true
published_at: "2025-04-22"
---


# 【HubSpot】理想のコードブロックをつくるまでのハック集 🧩

2025-04-22 · Reina

HubSpotのカスタムモジュールでコードブロックを表示するなら、[GitHubで配布されているやつ](https://github.com/TheWebTech/hs-code-block-web-component)をそのまま使うのが定番。

（👇でもその紹介をしています）

https://zenn.dev/raynya/articles/hubspot-codeblock



でも……ちょっと惜しいのよね！(`･ω･´)

「見た目が惜しい」「挙動が惜しい」などなど、使ってみると気になるポイントがじわじわ出てくるのです。

ということで今回は、**Reina的に理想のコードブロックを作るためにやったカスタムハック**を3つにしぼって紹介します〜！🛠️💖

![ChatGPT Image 2025年4月22日 22_37_28](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8822%E6%97%A5%2022_37_28.png)

## 🧠 モジュールのしくみざっくり解説

このコードブロックモジュールは、HubSpotのカスタムモジュールとして以下の3ステップで構成されています：

1. **fields.json** で設定したフィールド（言語・コード・ラベルなど）を表示する
2. **module.html** で各コードブロックの構造とラベル・ボタン表示を定義
3. **Prism.js + 自前JS + CSS** の三位一体連携で機能を実現！


``` html
<div class="label-bar">{{ module.block_label }}
  <button class="copy-button">📋 コピー</button>
</div>
<pre>
  <code class="language-{{ module.language }}">
    {{ module.code_snippet | escape | escape_jinjava }}
  </code>
</pre>
```

---

### 🔍 Prism.jsとは？

このモジュールのハイライト部分を支えてくれてるのが [Prism.js](https://prismjs.com/)！  
軽量＆シンプルで、**構文ハイライトを簡単に適用できるライブラリ**です。

* HTMLに `<code class="language-js">` のようにクラスをつけるだけ
* CSSテーマで雰囲気もカスタムしやすい
* ただし！ `pre` や `code` に強めのスタイルが効くので上書きは必須💥

特別な初期化処理は不要で、`Prism.highlightAll()` で一括ハイライトもできちゃう。

### 🧼 escape × escape\_jinjava ってなに？

HubSpot CMSでは、`{{ my_variable }}` といったHubL構文がそのままHTML上にあると「これはHubSpotのテンプレート変数だ！」💬 と勘違いされてしまいます。

それを防ぐために、

* `| escape` で **HTMLタグなどを文字として扱う**
* `| escape_jinjava` で **HubLの波かっこを文字として扱う**

という2段階エスケープが必要になります。

``` html
<code>{{ module.code_snippet | escape | escape_jinjava }}</code>
```

これで `{{ my_variable }}` のようなテンプレっぽい記述も**そのままコードとして安全に表示できる**ようになります！

---

### 🔧 ハックその1. Prism の挙動にふりまわされる件

![ChatGPT Image 2025年4月22日 22_36_27](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8822%E6%97%A5%2022_36_27.png)

Prism.js を使うと行番号や構文ハイライトは超かんたんに導入できるんだけど、**スタイルが強すぎる＆余白が勝手に入る問題**が出てきます。

特に困ったのはこの2つ：

* `pre[class*=language-]` に `margin-top` が入ってきちゃう
* `font-size` が `1rem` で上書きされてる（！）

``` css
pre,
pre[class*="language-"] {
  margin: 0 !important;
  font-size: 13px !important;
}
```

で対抗できたけど、**line-numberと文字のズレ**も地味に大変だったので……

> 最終的には「行番号はナシ！」で割り切りました。

泣く泣く。でも、スッキリした。

あともうひとつ地味に悩まされたのが、**コードの1行目に変な空白が入る問題**！

よ〜く見ると、`code` タグの中に余計なインデント（スペース）が入ってたのが原因でした。

``` html
<pre>
  <code> ← この空白が出力されちゃう！
    {{ module.code_snippet }}
  </code>
</pre>
```


対策としてはシンプルで、**`<code>` タグを1行で閉じる**のが正解💡

``` html
<pre><code>{{ module.code_snippet }}</code></pre>
```

これで「1行目だけなんかズレてる〜〜😭」みたいな地味バグが解消されたのでした◎

### 📎 ハックその2. コピー/アコーディオンは可愛く＆確実に

Prismにはコピー機能が入ってないので、**手作りしました**。でも以下のポイントが意外と大変：

* コピーボタンの配置（右上だと見切れた😭）
* コピー後のリアクション（✅ コピー済みに変更）
* ボタンの可愛さと色合いのバランス

CSSでぽこっと膨らむ `@keyframes poko-scale` アニメも添えて、**見た目もかわいく、ちゃんと動く**を目指しました💪

さらに今回は、「コードブロック自体を折りたためるようにしたい！」という野望も加わって、

* アコーディオン機能を `.code-body` に仕込む
* `.label-bar` をクリックすると開閉できるようにする
* クリック感が伝わるように三角マーク（`▾` / `▸`）も追加

という構造にリファクタリング！

コードブロックってたまに「長くなりすぎる」問題もあるから、**このアコーディオン機能はかなり便利＆見た目スッキリ**になりました🎀

### 🎨 ハックその3. 見た目のカスタム（ふわっと統一感）

ダークモードっぽいテーマが好きなので、背景は `#2d2d2d` に統一。  
ラベルのところも「labelが空なら背景なし」になるように：

``` css
.label-bar:empty {
  background: none;
  padding: 0;
  margin-bottom: 0;
}
```

さらに、コピーボタンの色もくすみピンクとグレーでやわらかく🩰  
折りたたみ対応もして、**スッキリ＆ふわかわ**に仕上げました♡

![ChatGPT Image 2025年4月22日 22_36_57](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8822%E6%97%A5%2022_36_57.png)

## 📦 最終コードまとめ（完成形）

[#vibe coding](https://zenn.dev/yoshiko/articles/my-vibe-coding) なので細かいところはあしからず…

``` json:meta.json
{"global":false,"host_template_types":["PAGE","BLOG_POST"],"label":"Code Block_v2","is_available_for_new_content":true,"description":"code block with Prism.js and copy button"}
```

```json:fields.json
[{"name":"block_label","label":"Label","type":"text"}, {"name":"language","label":"Language","type":"choice","display":"select","choices":[["html","HTML"],["css","CSS"],["javascript","JavaScript"],["jinja2","HubL"],["json","JSON"],["powershell","PowerShell"]],"default":"html"}, {"name":"code_snippet","label":"Code","type":"text","allow_new_line":true}]
```
```html:module.html
<!-- prism.js の読み込み -->
{{ require_css("https://cdn.jsdelivr.net/npm/prismjs/themes/prism-tomorrow.min.css") }}
{{ require_js("https://cdn.jsdelivr.net/npm/prismjs/prism.js") }}
{{ require_js("https://cdn.jsdelivr.net/npm/prismjs/components/prism-jinja2.min.js") }}
{{ require_js("https://cdn.jsdelivr.net/npm/prismjs/components/prism-json.min.js") }}
{{ require_js("https://cdn.jsdelivr.net/npm/prismjs/components/prism-powershell.min.js") }}

<div class="code-container">
  <div class="label-bar js-toggle">
    {{ module.block_label }}
    <button class="copy-button">📋 コピー</button>
  </div>
  <div class="code-body">
    <pre><code class="language-{{ module.language }}">{{ module.code_snippet | escape | escape_jinjava }}</code></pre>
  </div>
</div>

```
```css:module.css
  /* ========== Container ========== */

  .code-container {
  border-radius: 6px;
  background-color: #2d2d2d;
  overflow: hidden;
  margin-bottom: 1rem;
  }
  
  /* ========== Label Bar ========== */
  
  .label-bar {
  font-weight: bold;
  color: #cbd5e1;
  font-size: 12px;
  background-color: #2d2d2d;
  padding: 0.4rem 1rem;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  transition: background 0.2s ease;
  }
  
  .label-bar::before {
  content: "▾";
  margin-right: 0.5rem;
  transition: transform 0.2s ease;
  }
  
  .label-bar.collapsed::before {
  transform: rotate(-90deg);
  }
  
  .label-bar:hover {
  background-color: #3a3a3a;
  }
  
  /* ========== Collapsible Body ========== */
  
  .code-body {
  max-height: 1000px;
  opacity: 1;
  transition: max-height 0.3s ease, opacity 0.3s ease;
  overflow: hidden;
  }
  
  .code-body.collapsed {
  max-height: 0;
  opacity: 0;
  padding: 0 !important;
  pointer-events: none;
  }
  
  /* ========== Code Block ========== */
  
  .code-body pre,
  pre[class*="language-"] {
  margin: 0 !important;
  font-family: Menlo, Monaco, 'Courier New', monospace;
  line-height: 1.5;
  font-size: 13px !important;
  position: relative;
  color: #f8f8f2;
  background-color: #2d2d2d;
  padding: 0 !important;
  white-space: pre;
  }
  
  .code-body code,
  code[class*="language-"] {
  display: block;
  min-height: 1em;
  padding: 0.2rem 1rem !important;
  white-space: pre;
  }
  
  /* ========== Copy Button ========== */
  
  .copy-button {
  font-size: 0.75rem;
  color: #e5d9e7;
  background: #2d2d2d;
  border: 1px solid #2d2d2d;
  border-radius: 0.25rem;
  padding: 0.2rem 0.5rem;
  transition: all 0.2s ease;
  }
  
  .copy-button:hover {
  background: #3b3b3b;
  }
  
  .copy-button.poko {
  animation: poko-scale 200ms ease;
  }
  
  /* ========== Animation ========== */
  
  @keyframes poko-scale {
  0%   { transform: scale(1); }
  50%  { transform: scale(1.15); }
  100% { transform: scale(1); }
  }
```
```js:module.js
document.addEventListener("DOMContentLoaded", () => {
  // アコーディオン開閉処理
  document.querySelectorAll(".label-bar").forEach(label => {
    label.addEventListener("click", () => {
      label.classList.toggle("collapsed");
      const codeBody = label.nextElementSibling;
      if (codeBody && codeBody.classList.contains("code-body")) {
        codeBody.classList.toggle("collapsed");
      }
    });
  });

  // コピー処理
  document.querySelectorAll(".copy-button").forEach(button => {
    button.addEventListener("click", (e) => {
      e.stopPropagation(); // アコーディオンとバッティングしないように！
      const codeEl = button.closest(".code-container").querySelector("code");
      if (codeEl) {
        navigator.clipboard.writeText(codeEl.textContent);
        button.textContent = "✅ コピー済み";
        button.classList.add("poko");
        setTimeout(() => {
          button.textContent = "📋 コピー";
          button.classList.remove("poko");
        }, 1200);
      }
    });
  });
});

```



## 📝 おわりに

最終的には「タブ切り替え」や「line-number」などを**あえて削ってシンプルに**したけど、  
その分「見た目の可愛さ」「コピーの確実さ」に全振りして、**実用性重視なモジュール**ができました！

次はこのコードブロックを活かして、ブログテーマ全体のダークモードもやりたいな〜💭  
ぜひ、この記事がどなたかのカスタムモジュール沼の道しるべになりますように🐾