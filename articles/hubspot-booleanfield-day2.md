---
title: "チェックひとつで背景色が変わる！HubSpotのBooleanFieldで学ぶ条件レンダリング【HubSpot×React Day2】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot,React,HubSpot CMS]
published: true
published_at: 2025-05-03
---



# チェックひとつで背景色が変わる！HubSpotのBooleanFieldで学ぶ条件レンダリング【Day2】

2025-05-03 · Reina

## はじめに 🌿

HubSpotのReactテーマで、チェックボックス（BooleanField）を使って、背景色が切り替わる通知ボックスを作ってみたよ！

「Booleanってなに？」「true/falseで見た目が変わるってどういうこと？」って思ってたわたしも、やってみたらなるほどがいっぱいでした〜✨

![ChatGPT Image 2025年5月3日 12_06_07 (1)](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%883%E6%97%A5%2012_06_07%20(1).png)  

---

## この記事でわかること ✏️

* BooleanField（チェックボックスのフィールド）の使い方
* 三項演算子を使った条件レンダリングのしくみ
* CMSエディターとReactのpropsがどうつながってるか

---

## 今日のモジュール 🛠️

今日は、以下のような「通知ボックス」を作ったよ！

* メッセージを入力するテキストフィールド
* 「目立たせる」にチェックを入れると背景色が変わる

![react-day02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/react-day02.gif)



### 🔍 BooleanFieldってなに？

- HubSpot CMSで「ON/OFF」「Yes/No」みたいな状態を切り替えるチェックボックスのこと！

- React側では fieldValues.highlight という boolean型（true/false） の props として渡ってくるよ！
（boolean型＝「はい or いいえ」などの択一選択をtrue/falseで表すデータ型。HubSpotのページエディターではチェックボックスとして表示されます）

- チェックが入ってると true、入ってないと false

サンプルコード：
```jsx:index.jsx
// HubSpot CMS用のReactコンポーネント群を読み込む
import { TextField, BooleanField, ModuleFields } from '@hubspot/cms-components/fields';

// Reactコンポーネントを定義（HubSpotから渡されるfieldValuesを受け取る）
export const Component = ({ fieldValues }) => {
  // fieldValuesから「message」と「highlight」を取り出す（propsの分割代入）
  const { message, highlight } = fieldValues;

  return (
    // 通知ボックス全体の見た目を制御するdiv
    <div
      style={{
        // highlightがtrueのときは青背景、それ以外は透明にする
        backgroundColor: highlight ? '#DBEAFE' : 'transparent',
        // 外枠を青色に
        border: '1px solid #60A5FA',
        // 内側の余白
        padding: '1rem',
        // 角を丸くする
        borderRadius: '0.5rem',
      }}
    >
      {/* 通知メッセージのテキストを表示 */}
      <p style={{ margin: 0 }}>{message}</p>
    </div>
  );
};

// HubSpotのモジュールエディターに表示されるフィールド定義
export const fields = (
  <ModuleFields>
    {/* 通知メッセージのテキストフィールド */}
    <TextField
      name="message"
      label="通知メッセージ"
      default="これは通知です！"
    />
    {/* チェックボックスでON/OFFを切り替えるBooleanField */}
    <BooleanField
      name="highlight"
      label="目立たせる"
      default={false} // デフォルトは「チェックなし（false）」
    />
  </ModuleFields>
);

// モジュールのメタ情報
export const meta = {
  label: '通知ボックス',
};
```

---

## 三項演算子ってなに？ 🤔

今回の肝はこれ！👇

```
backgroundColor: highlight ? '#DBEAFE' : 'transparent'
```

これは「三項演算子」と呼ばれる書き方で、ざっくり言うと：

> ✅ **highlightがtrueなら → '#DBEAFE'（青）**   
> ❌ **falseなら → 'transparent'（透明）**

という条件分岐！

### if文と比べると…
```
let bgColor;  
if (highlight) {  
bgColor = '#DBEAFE';  
} else {  
bgColor = 'transparent';  
}
```

React公式のドキュメントにも詳しくあるのでおすすめ👇   
🔗 [React公式：三項演算子での条件レンダリング](\"https://ja.react.dev/learn/conditional-rendering#conditional-ternary-operator-\")

---

## まとめ ✨

CMSで「チェックを入れるかどうか」→ Reactで「見た目を変える」っていう流れがやっとつかめてきた！   
BooleanFieldって仕組みがわかると、UIに変化をつけるのがすごくラクになる感じがします。

次は、ボタンで何かを切り替えるようなインタラクティブなモジュールにも挑戦してみたいなと思います（Day3につづく…🌿）
