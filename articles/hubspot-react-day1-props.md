---
title: "propsって何？HubSpot×Reactでキャッチコピー表示してみた【HubSpot×React Day1】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-01
---


# propsって何？HubSpot×Reactでキャッチコピー表示してみた【Day1】
![](https://blog.ryamagami.com/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2022_54_22%20(1).png)
2025-05-01 · Reina

## はじめに 🌿

HubSpotでReactテーマを作ることには成功したけど、「モジュールをReactで書く」ってまだよくわからない…！ そんなわたしが、気ままにReactモジュールを学んでいくシリーズをはじめました📘

![ChatGPT Image 2025年5月1日 22_41_15 (1)](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2022_41_15%20(1).png)

---

## この記事でわかること ✏️

* HubSpotのReactモジュールってどうやって書くの？
* propsってなに？どうやって使うの？
* ページエディターとどうつながってるの？

---

## 結論 🎯

Reactモジュールは `props` を通じて、HubSpotエディターの入力を表示できる！ インラインスタイルで装飾すれば `index.jsx` だけで完結できてとってもシンプル💡

## 今日のモジュール 🛠️

今日はテキストフィールドに指定したキャッチコピーを表示させるシンプルなモジュール！

![react_day01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/react_day01.png)

モジュール (`src/theme/components/modules/CatchphraseDisplay/index.jsx`) のコードは以下：

```jsx
// HubSpotのCMSコンポーネントライブラリから、TextFieldとModuleFieldsをインポート
import { TextField, ModuleFields } from '@hubspot/cms-components/fields';

// ここがReactモジュールの本体！HubSpotから受け取るpropsとして fieldValues を受け取るよ
export const Component = ({ fieldValues }) => {
  // fieldValues から catchphrase という名前の値を取り出す（TextFieldのnameと一致している必要あり）
  const { catchphrase } = fieldValues;

  return (
    // インラインスタイルで装飾したボックスを表示（背景色・左線・文字色・余白・角丸）
    <div
      style={{
        backgroundColor: '#FEF3C7', // Tailwindの bg-orange-100 に相当
        borderLeft: '4px solid #F97316', // Tailwindの border-orange-500 に相当
        color: '#7C2D12', // Tailwindの text-orange-900 に相当
        padding: '1rem',
        borderRadius: '0.5rem',
      }}
    >
      <p
        style={{
          fontSize: '1.125rem', // Tailwindの text-lg
          fontWeight: 600, // Tailwindの font-semibold
          margin: 0, // 段落の余白をリセット
        }}
      >
        {/* ページエディターで入力されたキャッチコピーがここに表示される！ */}
        {catchphrase}
      </p>
    </div>
  );
};

// このモジュールに含めるフィールドを定義するエリア
export const fields = (
  <ModuleFields>
    {/* テキストフィールド。nameは fieldValues のキーとして使われる！ */}
    <TextField
      name="catchphrase" // fieldValues.catchphrase として使われるキー名
      label="キャッチコピー" // CMSエディタに表示されるラベル
      default="人生はReactのようなもの。状態次第で変わるのだ！" // 初期値（例文）
    />
  </ModuleFields>
);

// モジュールのメタ情報（エディター上で表示されるモジュール名など）
export const meta = {
  label: "キャッチコピー表示", // モジュールのラベル名
};

```

## モジュール構成の解説 🔍

#### propsって何？
>
> props (property) とは、Reactコンポーネントに**外から値を渡すための仕組み**
> HubSpotのReactモジュールでは、エディターで入力された値が`props`（正確には`fieldValues`というオブジェクト）として渡されます。

詳しくはこちら：[コンポーネントに props を渡す](https://ja.react.dev/learn/passing-props-to-a-component)


### ① `ModuleFields` ＝ モジュール全体のフィールド定義エリア

`fields.json` の React版みたいなイメージ！

* `ModuleFields` の中に、**このモジュールで使うフィールドたち**を並べて定義
* `TextField` の `name="catchphrase"` が「propsのキー」になる

### ② `export const Component = ({ fieldValues }) => { ... }`

ここで `fieldValues` という名前で、**CMS側から渡されるすべてのフィールド値**を受け取れる！

### ③ `const { catchphrase } = fieldValues;`

これはJavaScriptの**分割代入**。

```
const catchphrase = fieldValues.catchphrase;
```

と同じ意味だよ！

### ④ `{ catchphrase }` を return の中で使う！

```
return (
  <p>{catchphrase}</p>
);
```

これで React がキャッチコピーの文字列を画面に表示してくれる🌟

## 🌟 全体の流れ

```
<TextField name="catchphrase" />
↓
→ CMSエディタで入力 → 保存
↓
→ HubSpotがReactに `fieldValues = { catchphrase: "入力したテキスト" }` を渡す
↓
→ `const { catchphrase } = fieldValues` で取り出す
↓
→ return に `{catchphrase}` を書いて表示！  
  

```

## ひとこと⚡

Tailwindでスタイルをつけようとしたけど、ビルド＆importがうまくいかず断念…🥲 [このドキュメント](https://developers.hubspot.com/docs/reference/cms/react/styling#tailwind) を見ながら、またいつかチャレンジしたい！ 今回はインラインスタイルでうまくいったので、まずは大満足♡

## まとめ ✨

Reactモジュールの「最小のかたち」がわかって、とてもスッキリしたDay1でした！ propsを通じてフィールド値が表示できる、この感動…✨ 次回はちょっとだけ動きのあるUIにも挑戦したいな！

![ChatGPT Image 2025年5月1日 22_46_41 (1)](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2022_46_41%20(1).png)
