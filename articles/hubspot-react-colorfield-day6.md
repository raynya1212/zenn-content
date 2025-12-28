---
title: "背景色を自由に変えられるHubSpot React モジュールのColorフィールドの使い方【HubSpot × React DAY6】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot,React,HubSpot CMS]
published: true
published_at: 2025-05-07
---

# 背景色を自由に変えられるHubSpot React モジュールのColorフィールドの使い方【DAY6】

2025-05-07 · Reina

## はじめに 🌿

HubSpot×Reactモジュール強化月間DAY6！  
今回はHubSpotモジュールでよく登場する背景色を指定するフィールド（Colorフィールド）の使い方を学んでいきます🎨

![ColorFieldで背景色を指定するReactコンポーネントと、色選択UIを紹介する女性のイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/react-day6-intro-colorfield.png)

---

## この記事でわかること ✏️

* color フィールドの書き方 💡
* 背景色を React の `style` で反映する方法 🎨
* オプショナルチェイニングで値を安全に取り出すテクニック 🛠️

---

## 結論 🎯

背景色を指定するフィールドは `ColorField` を使う！   
そして、`ColorField` で定義したカラーは `fieldValues.bg_color?.color` と書くだけで簡単に背景色を動的に変えられるよ！

---

## やり方 🛠️

### Step1：ColorField を定義しよう

```jsx
<ColorField
  name="bg_color"
  label="背景色"
  required={false}
  locked={false}
  default={{
    color: '#ff0000',
    opacity: 100,
  }}
/>
```

HubSpot CMS × Reactモジュールでは、`@hubspot/cms-components/fields` から簡単にフィールドを呼び出せるよ。

### Step2：背景色を style に渡そう

```jsx
style={{
  backgroundColor: bg_color?.color,
  padding: '1rem',
}}
```

このとき注意したいのが、`bg_color` の中身がオブジェクトであること！   
React の `style` 属性に直接渡すためには `bg_color.color` を使う必要があるよ。

👉 `?.color` についてはこのあと解説！

---

## サンプルコード（解説付き）💻

以下は今回作成した `index.jsx` のコードだよ！コメントも一緒にどうぞ📝

```jsx:index.jsx
import {
  ModuleFields,
  TextField,
  ColorField
} from '@hubspot/cms-components/fields';

// コンポーネント本体
export function Component({ fieldValues }) {
  const { text, bg_color } = fieldValues;

  return (
    <div
      style={{
        // ColorFieldの中のcolorプロパティだけを使う！
        backgroundColor: bg_color?.color,
        padding: '1rem',
      }}
    >
      <p>{text}</p>
    </div>
  );
}

// フィールド定義
export const fields = (
  <ModuleFields>
    <TextField
      name="text"
      label="テキスト"
      default="Color fields provide a color picker interface for content creators."
    />
    <ColorField
      name="bg_color"
      label="背景色"
      required={false}
      locked={false}
      default={{
        color: '#ff0000',
        opacity: 100,
      }}
    />
  </ModuleFields>
);

// モジュールのメタ情報
export const meta = {
  label: '06_背景色フィールド',
};
```

プロジェクトをデプロイしてモジュールを確認すると、モジュール内で背景色が選択できるようになっていることがわかります🎨✨

![react_day06](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_day06.gif)

---

## 詰まったところ ⚡

最初、`backgroundColor: bg_color` のように書いたけど、うまく反映されなかったの！   
調べてみると、`bg_color` はオブジェクトで `{ color: '#xxxxxx', opacity: 100 }` の形になっていることがわかって、   
正しくは `bg_color?.color` という形でオプショナルチェーンを使う必要があったんだ💡


## オプショナルチェーン

## （optional chaining）って？
```
bg_color?.color
```

これは「bg\_color が null や undefined じゃなかったら color を取り出してね」という便利な書き方！


| 書き方 | 意味 |
| --- | --- |
|`bg_color.color` | `bg_color` が null だとエラーになる ❌ || `bg_color?.color` | `bg_color` があれば `.color` を取り出す ✅ |


ReactやJSでよく使われる、安全第一な書き方になるよ

---

## まとめ ✨

ColorFieldを使うと、背景色を直感的にカスタマイズできて、しかもコードもシンプル！   
`bg_color?.color` のような安全なアクセス方法を覚えておけば、エラーにも強くなるよ💕

次回 DAY7 は… 🖼️画像付きプロフィールカードを作るよ！imageフィールドを使ってプロフィールを可愛く表示してみよう✨

![ReactでColorFieldを使って背景色のカスタマイズができた女性がガッツポーズしているイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/react-day6-summary-illustration.png)

---

## あわせて読みたい 📖

* [HubSpot CMS Reference|Module and theme fields>Color](https://developers.hubspot.com/docs/reference/cms/fields/module-theme-fields?#color)
* [オプショナルチェーン (?.)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Optional_chaining)